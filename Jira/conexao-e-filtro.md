# Jira: conexão e filtro

## Conexão

- Site: https://equals.atlassian.net
- Usuário conectado pelo plugin Atlassian: davi.soares.
- Account ID observado: 712020:49b629d0-51a4-4b14-bc89-b3eb87f1fb20.
- Cloud ID observado: ab00e570-4c7e-4aee-afc6-399df538aced.
- Usar o plugin Atlassian autorizado; usuário decidiu não continuar pela autenticação local/token.
- Pasta .jira contém config.json e credential.xml, ambos legíveis. Nunca exibir ou copiar os segredos.
- Tentativas locais HTTP falharam por proxy 127.0.0.1 e erro Windows TLS SEC_E_NO_CREDENTIALS. Isso não provou senha incorreta. Usuário esclareceu que Jira não requer VPN.

## Filtro padrão de “meus chamados”

Se houver somente um chamado no resultado completo do filtro, ler automaticamente a solicitação, os campos e os comentários e consultar PLT/CRC do contrato. Responder com o pedido e a situação no banco, sem esperar que o usuário peça cada etapa. Manter todas essas ações em leitura.

https://equals.atlassian.net/issues?jql=assignee+%3D+currentUser%28%29%0AAND+type+IN+%28%22Implanta%C3%A7%C3%A3o+de+ERP%22%2C+%22Implanta%C3%A7%C3%A3o+de+Meios+de+Pagamento%22%2C+Atendimento%2C+%22Process+Flow%22%29%0AAND+status+IN+%28Backlog%2C+%22Backlog+Priorizado%22%2C+%22Em+progresso%22%29%0AORDER+BY+created+DESC&referrer=quick-find

```jql
assignee = currentUser()
AND type IN ("Implantação de ERP", "Implantação de Meios de Pagamento", Atendimento, "Process Flow")
AND status IN (Backlog, "Backlog Priorizado", "Em progresso")
ORDER BY created DESC
```

## Escrita pelo plugin, testada em 12/11/2026

- Há ferramentas para comentar e transicionar com o usuário conectado davi.soares. Exigir autorização de escopo do usuário; autorização de leitura e preparação não autoriza escrita automaticamente.
- Permissão consultada: padrão Allow low-risk actions; Atlassian Use my default. Não foi alterada a configuração de permissões.
- Para menção, ler getContentFormatGuide e usar HTML: `<span data-type="mention" data-user-id="<AAID real>">@Nome</span>`; nunca inventar accountId.
- Usar cloudId UUID explícito nas escritas: ab00e570-4c7e-4aee-afc6-399df538aced. Com hostname, duas tentativas de comentário retornaram autenticação aceita/repita; com UUID a gravação funcionou. Não concluir que o comentário existe só pelo aceite da autenticação; verificar sucesso/ID e evitar duplicar.
- Para procurar duplicidade operacional, pesquisar contrato OU filiação no texto, sem limitar responsável, tipo, data ou status; conferir campos e comentários. Cópias/clones podem preservar menções ao contrato errado. Não atribuir autoria de processamento apenas pela coincidência de horário.

- Confirmar o relator no Jira antes de mencionar; pode ser uma conta de integracao.
- Comentario e transicao dependem do escopo autorizado; uma autorizacao pontual nao vale para todos os chamados.
- Consultar as transicoes disponiveis e selecionar o destino Aguardando Validacao IMP; nao fixar IDs entre fluxos.
- Comentário padrão breve, quando autorizado: “Processamento realizado, favor validar”. Mencionar o relator quando solicitado e incluir orientações específicas de configuração quando combinadas.
- Se a validação não estiver disponível no backlog, usar a transição para Em progresso e consultar novamente as transições antes de ir para Aguardando Validação IMP. Executar apenas dentro da autorização recebida.
- Uma autorização para comentar/mover depois do processamento continua válida quando a conferência posterior confirmar sucesso; não pedir autorização outra vez para a mesma ação já combinada. Antes disso, conferir CRC e sobras na PLT.

## Comentários e validação

- Preferir comentários breves e usar o texto exato fornecido pelo usuário. Exemplos aceitos: “Arquivo complementar de uma linha processado com sucesso”, “Arquivo movido com sucesso, favor validar.” e “Arquivos processados favor validar”. Não acrescentar IDs, diagnósticos ou menções sem necessidade.
- Consultar as transições do chamado atual. Para implantação, usar Aguardando Validação IMP quando disponível. Fluxos de Atendimento podem oferecer AGUARDANDO VALIDAÇÃO - EXTERNA; não presumir que todos usem o mesmo destino.
- Se necessário passar por Em progresso para liberar validação, consultar novamente as transições após essa mudança. Não reutilizar IDs fixos de outro chamado.
