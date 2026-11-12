# Consulta e reprocessamento: ESB e Yoke

## Acesso ao banco

- Configuração existente: C:\Users\davi.soares\.oracle\producao.json; propriedades host, port, user, service.
- Credencial existente: C:\Users\davi.soares\.oracle\producao.credential.xml, carregar via Import-Clixml.
- Python com oracledb disponível. Consultas anteriores usaram schema EQUALS, conexão via configuração e credencial local, SET TRANSACTION READ ONLY e call_timeout de 20000 ms.
- Manter senha apenas em memória durante conexão e remover variáveis temporárias; não registrar valores.
- Trabalho realizado até aqui no banco foi somente leitura; exclusões e cópias foram realizadas pelo usuário, não pelo assistente.

## Fluxo padrão

Executar acessos às máquinas somente pelo usuário, seguindo [acesso-e-execucao.md](acesso-e-execucao.md). O assistente prepara scripts e consulta o banco/Jira; não conecta às máquinas.

1. Ler Jira e identificar contrato, cliente, arquivo solicitado e período.
2. Consultar EQUALS.CRCPROCESSAMENTO por IDCONTRATO com ORDER BY IDPROCESSAMENTO DESC, sempre como padrão.
3. Para correção a partir de um arquivo, selecionar do processamento mais recente até o arquivo citado, inclusive. Não limitar apenas ao mesmo dia nem ignorar arquivos IF que estejam no intervalo.
4. Cruzar NMARQUIVO com PLTARQUIVO para IDARQUIVO e PATHBACKUP, de onde serão feitas as cópias.
5. Identificar mecanismo e destino conforme abaixo.
6. Preparar cp em blocos para o usuário executar após as exclusões necessárias e a substituição pelo corrigido. Não executar exclusões ou envios sem instrução do usuário.
7. Conferir novamente CRC por IDPROCESSAMENTO DESC, FLRESULTADO, DTINICIO, DTFINAL e DSRESULTADO. Comparar DTINICIO com o comentário relevante do relator quando solicitado.
8. Depois de TODO reprocessamento, consultar a PLT do contrato com FLSTATUS IN (1,2,3,8). Se houver sobras, entregar ao usuário os IDARQUIVO, nomes e status para ele setar 9; segundo o usuário, essas sobras podem travar o processamento automático, embora normalmente não sobrem. Não alterar status no banco por conta própria. Não encerrar a conferência apenas pelo sucesso na CRC.

## Prazo de conferência após envio

- Sempre que o usuário confirmar envio dos arquivos para processamento/reprocessamento, contar 10 minutos e conferir automaticamente, na conversa ativa, a CRC (resultado, término e erros) e a fila da PLT em status 1,2,3,8. Informar todos os IDARQUIVO restantes.
- Contar a partir do envio confirmado, não da geração dos comandos. Preparar cp não comprova execução.
- Se o usuário pedir para conferir antes, consultar imediatamente. Sem evidência de sucesso na CRC, não comentar processamento concluído nem mover para validação.
- Não prometer timer persistente ou execução com aplicativo fechado sem ferramenta de agendamento confirmada. Se usar espera ativa, mantê-la interrompível e com intervalos de até 60 segundos.

## Regras consolidadas em 12/11/2026

- A CRC é a referência para confirmar processamento. Status 4 na PLT, isoladamente, não comprova processamento na CRC. PLT serve para recebimento, localização, seleção e identificação de pendências.
- Conferência pós-reprocessamento obrigatória (leitura): `SELECT IDARQUIVO, NMARQUIVO, FLSTATUS FROM EQUALS.PLTARQUIVO WHERE IDCONTRATO = :contrato AND FLSTATUS IN (1,2,3,8) ORDER BY IDARQUIVO DESC`. Entregar todos os IDs encontrados, sem truncar silenciosamente.
- Pesquisa de arquivos na error: não restringir ao contrato; usar NRFILIACAOCONTRATO ou NMARQUIVO LIKE '%<filiacao>%' / '%<CNPJ com zero inicial>%' e LOWER(PATH) LIKE '%error%' ou LOWER(PATHBACKUP) LIKE '%error%'. Histórico de passagem por error não significa erro atual no processamento. No caso Cielo, usuário explicou que faltava cadastro do contrato antes de mover os arquivos.
- Significado de status 6 na PLT não foi confirmado no dicionário nesta conversa; não chamar automaticamente de erro ou duplicado.
- Usuário aceitou testes de existência da pasta (`test -d`), escrita no destino (`test -w`) e leitura de todas as origens (`test -r`) antes dos cp, com `set -eu`. Esses testes não alteram arquivos. Pode entregar um .sh; quando ele pedir, apresentar o bloco diretamente na conversa.
- Não garantir ordem de consumo apenas por ordenar cp; conferir sequência e DTINICIO na CRC.
- Se sobrarem arquivos na PLT, cruzar cada sequência/dia com a CRC e confirmar que outro arquivo foi finalizado com sucesso para aquele dia; em seleções por tamanho, conferir também que o processado tinha tamanho igual ou maior. Entregar os IDARQUIVO das sobras, mantendo a alteração para 9 a cargo do usuário.

### Shellbox: normal versus retroativo

- Para a mesma sequência/dia, comparar SHELLBOX e SHELLBOX-RETROATIVO e selecionar um único arquivo, o de maior NRTAMANHOARQUIVO. Selecionar pelo dia em NRSEQUENCIA, não pela DTGERACAOARQUIVO: retroativos podem ter geração posterior à data coberta.
- Enviar em sequência crescente a partir da data solicitada até o diário. Verificar lacunas de dias.
- Em empate de tamanho, o script de 12/11 preferiu registro não duplicado, depois recebimento mais recente e maior IDARQUIVO. Esse desempate foi escolha da implementação, não regra expressa pelo usuário.
- A seleção de 12/11 comparou todos os status e incluiu ignorados/duplicados quando vencedores pelo critério de tamanho. Sempre explicitar isso e conferir as versões menores remanescentes na PLT após processar.

## ESB

- Buscar PLTHISTORICOARQUIVO por IDARQUIVO e FLSTATUS = 3; usar PATHANTERIOR como evidência de rota.
- CDESB na CRC ajuda a confirmar máquina, por exemplo ESBORA02.
- Para ESBORA02, usuário determinou usar o caminho direto /fileprocessing-ora-esb02/... nos comandos, em lugar do alias simbólico /fileproc-ora-esb02/....
- Senff: /fileprocessing-ora-esb02/crc/toprocess/senff/.
- No histórico com status 1: PATH é arquivamento, PATHANTERIOR identifica a entrada de upload. Senff: /filemgmt/senff.com.br/upload.
- Usuário prefere copiar em blocos mesmo quando são milhares de arquivos. Copiar em sequência não garante ordem de consumo; pode haver ordenação por nome e concorrência. Não insistir em envio unitário como padrão.
- No caso Senff, bloco foi preparado SENFF antes de SENFF_IF em cada dia, mas ESB consumiu IF primeiro. Causa exata não comprovada; usuário considera provável listagem por nome ascendente. Não atribuir certeza à máquina nem garantir ordem por cp.

## Yoke

Consultar o catálogo [Yoke/rotas.md](Yoke/rotas.md) para o mapeamento de adquirente/layout → rota → caminho. Acrescentar novas rotas e atualizar confirmações nesse catálogo, evitando listas duplicadas neste documento.

- Identificar adquirente em NMFILAPROCESSAMENTO, por exemplo PRODUCAO-REPROCESSAMENTO-YOKE-SMALL-santander-pix.fifo -> santander-pix.
- Destino padrão informado pelo usuário:

```text
/fileproc-ora-esb04/crc/microservice/<adquirente>/reprocessing_files/
```

- Manter fileproc neste padrão Yoke, conforme instruído; não aplicar automaticamente a troca para fileprocessing do ESBORA02.

## Status confirmados no dicionário do banco

PLTARQUIVO.FLSTATUS: 0 duplicado; 1 aguardando processamento; 2 enviado para processamento; 3 em processamento; 4 processado com sucesso; 5 processado com erro; 7 implantação manual; 8 implantação automática; 9 ignorado para processamento.

CRCPROCESSAMENTO.FLRESULTADO: 0 erro; 1 sucesso.

Sucesso técnico não comprova integridade das vendas nem cobertura completa do período solicitado.

## Venda interna / VIT

- Nas consultas de venda interna, usar `IDCLIENTE` como parâmetro e `FLORIGEM = 'I'` na CRC e na PLT. Não substituir por `IDCONTRATO`; ele pode estar nulo nesse fluxo.
- Consultar CRC por `IDPROCESSAMENTO DESC`. Um arquivo com sucesso técnico pode ter uma transação ausente; distinguir resultado do processamento e validação da transação.
- Arquivos complementares podem ser recusados por sequência já registrada. Conferir a mensagem e o processamento referenciado antes de definir uma correção; não excluir o original nem alterar sequência automaticamente.

## Filiações, seleção e conferência

- Obter filiações na `EQUALS.CRCCONTRATO` pelos IDs dos contratos: `IDADQUIRENTE`, `NRFILIACAOVAN`, `NRFILIACAOCONTRATO`, `NMMATRIZ` e `NRCNPJFILIACAO`, quando preenchido.
- O mesmo CNPJ pode aparecer em outras adquirentes. Limitar a seleção operacional à adquirente e aos diretórios corretos. Preservar zeros iniciais do CNPJ em pesquisas no nome.
- Para investigar error, usar `(LOWER(PATH) LIKE '%error%' OR LOWER(PATHBACKUP) LIKE '%error%')`; não restringir ao contrato quando registros ainda podem estar sem vínculo. Se ampliar a busca, separar resultados de outras adquirentes.
- Registro no banco não garante existência física. O usuário executa a checagem dos arquivos correspondentes à seleção; ignorar arquivos que não batam com o banco e relatar origens ausentes, sem inventar retenção ou exclusão como causa.
- Após reentrada, conferir os próprios IDs selecionados: nome, vínculo, caminho e status podem ser atualizados sem novo ID.
- Em erros de sequência já registrada, cruzar arquivo e contrato com o processamento referenciado e confirmar sucesso final. Totais iguais de sucessos e arquivos não bastam para comprovar cobertura.
- Conferir sempre PLT status 1, 2, 3 e 8. Separar pendência ativa, erro atual e erro de tentativa repetida com sucesso já confirmado. Não alterar status no banco por iniciativa própria.

## Ordem Nupay

Para envio de múltiplas naturezas Nupay, usar a ordem confirmada pelo usuário: `SALE-PAYMENTS`, `SALE-ADJUSTMENTS`, `SETTLEMENT-PAYMENTS`, `SETTLEMENT-ADJUSTMENTS`; dentro de cada grupo, `NMARQUIVO ASC`. Preservar envio em blocos. Conferir envios automáticos para evitar copiar novamente arquivos já enviados ou em processamento. A ordem de cópia não garante ordem de consumo da Yoke.
