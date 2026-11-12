# Instruções para sessões nesta pasta

## Início de cada nova sessão

1. Responder em português e ler README.md, Jira/conexao-e-filtro.md e Processamento/esb-e-yoke.md. Ler os arquivos de Chamados conforme necessário para a tarefa.
2. Na primeira interação, consultar o Jira pelo plugin Atlassian usando o filtro abaixo e apresentar os chamados atribuídos ao usuário com link, título, status e prioridade. A autorização para esta consulta de leitura é permanente. Não limitar à data de criação de hoje: incluir pendências anteriores que correspondam ao filtro. Seguir a paginação até completar os resultados.
3. Usar dados atuais do Jira, nunca apresentar a lista do backup como atual. Se a integração estiver indisponível, informar a limitação sem inventar resultados. Se a primeira mensagem trouxer outra tarefa, incorporar uma lista breve sem impedir sua execução; respeitar instruções posteriores do usuário.

Site: https://equals.atlassian.net

```jql
assignee = currentUser()
AND type IN ("Implantação de ERP", "Implantação de Meios de Pagamento", Atendimento, "Process Flow")
AND status IN (Backlog, "Backlog Priorizado", "Em progresso")
ORDER BY created DESC
```

O filtro exclui chamados em Aguardando Validação IMP. Não ampliar seus status sem solicitação.

Quando o filtro retornar somente um chamado, já ler a solicitação, os campos e os comentários e conferir a situação no banco (PLT e CRC), sem aguardar outro pedido. Apresentar o pedido junto do diagnóstico atual. Essa rotina autoriza consultas, não cópias nem alterações no Jira/banco.

## Durante o trabalho

- Consultar CRCPROCESSAMENTO por IDPROCESSAMENTO DESC, salvo instrução contrária.
- Após o usuário confirmar envio para processamento/reprocessamento, iniciar uma espera de 10 minutos na conversa ativa e então conferir CRC e PLT status 1,2,3,8, sem precisar de novo pedido. Se o usuário pedir conferência antes, consultar imediatamente. Não afirmar que existe agendamento em segundo plano se não houver ferramenta que o tenha criado; espera ativa depende da sessão continuar aberta.
- Seguir as regras ESB e Yoke documentadas em Processamento/esb-e-yoke.md, incluindo envio em blocos e caminhos distintos.
- Preservar a distinção entre informação consultada, informação relatada pelo usuário e inferências.
- Não armazenar senhas, tokens ou conteúdo de credenciais nos arquivos.
- Consultas são de leitura. Preparar comandos não autoriza executar exclusões, cópias ou alterações no Jira/banco.
- Não conectar às máquinas por SSH/SFTP. Gerar scripts locais e comandos SCP/SSH para o usuário executar. Seguir Processamento/acesso-e-execucao.md: script local, SCP para /ops/davi_tmp/ com nome único, SSH para execução, conferência no banco e limpeza dos temporários.
- Limpar somente temporários gerados para a tarefa depois da conferência, por caminhos exatos e sem recursão ou curingas. O usuário executa comandos remotos. Preservar arquivos operacionais, backups, credenciais, chaves e configurações de conexão.
- No backup Markdown, registrar somente procedimentos e caminhos de configuração necessários, sem segredos, históricos de chamados ou resultados operacionais.

## Quando o usuário avisar que vai sair

- Atualizar em Markdown o padrão de trabalho e os aprendizados reutilizáveis, agrupados por assunto em Jira e Processamento.
- Registrar procedimentos, preferências, regras de seleção e consultas padrão. Não salvar dossiês de chamados, listas de pendências ou resultados operacionais no backup, salvo solicitação específica. Scripts são temporários de trabalho; limpar somente os gerados para a tarefa após conferir o resultado, conforme o procedimento de execução.
- Atualizar o índice README.md se criar novos assuntos. Não afirmar que o backup é transcrição literal se for um resumo.
- Verificar os arquivos gravados e informar o resultado. Se a escrita estiver bloqueada, informar que não foi salvo.
- Não há detecção automática de fechamento do aplicativo: executar este procedimento quando o usuário avisar.
