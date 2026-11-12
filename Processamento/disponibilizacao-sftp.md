# Disponibilização de arquivos via SFTP

## Seleção e entrada

1. Identificar cliente, contrato, adquirente, caixa postal e período no Jira. Interpretar “hoje” conforme a data da solicitação e o contexto confirmado pelo usuário.
2. Consultar a PLT e distinguir recebimento (`DTRECEBIMENTO`) da data coberta pelo arquivo, sequência ou data no nome. Retroativos recebidos hoje podem cobrir períodos anteriores.
3. Usar `PATHBACKUP` para a origem arquivada e a caixa postal confirmada. O destino de disponibilização é `/filemgmt/<caixapostal>/download/`.
4. Quando autorizado reenviar pela entrada, consultar `PLTHISTORICOARQUIVO` em status 1: `PATHANTERIOR` identifica o upload; `PATH` pode ser arquivamento. Isso é reentrada no fluxo do contrato, não cópia direta para download.
5. Para reentrada no upload, remover do nome arquivado o prefixo e a hash: usar a nomenclatura original após o delimitador da hash, preservando o conteúdo. Não remover hashes por substituição genérica sem conferir o formato do nome.
6. Um caminho lógico `/filemgmtNew/<caixapostal>/upload` pode estar montado na ops como `/filemgmtNew-papgeral/<caixapostal>/upload`. Confirmar o caminho real e as permissões; não presumir que todas as montagens existam.
7. O usuário executa os comandos via SCP/SSH conforme acesso-e-execucao.md. Não criar diretórios nem alterar configurações remotas sem autorização.

## Conferência

- Pesquisar reentrada pelo nome original, incluindo possíveis prefixos e sufixos acrescentados pela plataforma. Não depender exclusivamente do nome arquivado exato.
- Confirmar cliente, contrato, adquirente, status e caminho na PLT. Não chamar status 9 de duplicado: 0 é duplicado; 9 é ignorado para processamento. Não inferir o significado dos status 6, 10 e 11 sem evidência do fluxo.
- O fluxo pode atualizar o registro existente na PLT em vez de criar um novo ID. Comparar também `DTULTIMOSTATUS`, nome e caminho dos IDs selecionados.
- Confirmar arquivo no download e comparar tamanho e, quando possível, SHA-256 com a origem. A cópia no upload, isoladamente, não confirma entrega ao cliente.

## Perfis YADE/Jade e JobScheduler

- Pasta de configurações na jobscheduler: `/filemgmt/equals.com.br/jade-configs/`.
- Reutilizar o INI indicado pelo usuário e substituir somente a caixa postal nos pontos solicitados. O `-profile` do `.job.xml` deve corresponder exatamente à seção do INI, inclusive maiúsculas e minúsculas.
- Preservar horários, filtros, hosts e opções do modelo, salvo pedido de alteração. Se `remove_files = true` estiver no modelo, esclarecer que o job prevê remoção ao executar; não introduzir essa opção por iniciativa própria.
- Quando solicitado apenas o texto, entregar o INI/XML na conversa sem acessar nem modificar o servidor.
