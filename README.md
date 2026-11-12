# Contexto de trabalho — atualizado em 12/11/2026

Padrao de trabalho para Jira e processamento Equals: procedimentos e aprendizados, sem dossies de chamados.

## Preferências do usuário

- Ao usuário avisar que vai encerrar a sessão, atualizar a documentação, criar commit com identidade pessoal e enviar para `https://github.com/Rullck/CODEX.git`, verificando o push. Esta autorização de envio ao Git vale para os próximos encerramentos. A nuvem permanece pendente de definição de destino.
- Conversar em português.
- Consultar CRCPROCESSAMENTO sempre por IDPROCESSAMENTO DESC, salvo orientação diferente.
- Preparar comandos de cópia em blocos, inclusive para milhares de arquivos; o usuário executa no terminal.
- Quando o usuário avisar que vai sair, atualizar este backup Markdown por assunto.
- Iniciar sessões nesta pasta para carregar AGENTS.md: ler o contexto e consultar os chamados atuais pelo filtro salvo na primeira interação.
- Não há detecção automática de fechamento do aplicativo; o usuário precisa avisar que vai sair.
- Não copiar credenciais para o backup.
- Não conectar às máquinas pelo assistente. Preparar script local, SCP, SSH para o usuário executar e limpeza dos temporários após conferência no banco; usar `/ops/davi_tmp/` na ops.
- Preservar credenciais, chaves, configurações de conexão, arquivos operacionais e backups durante a limpeza.
- A CRC confirma processamento; status da PLT isoladamente não confirma.
- Quando houver somente um chamado no filtro, já ler solicitação e comentários e consultar PLT/CRC, apresentando o diagnóstico sem esperar novo pedido.
- Após confirmação de envio, aguardar 10 minutos na conversa ativa e conferir CRC e sobras na PLT; atender imediatamente a pedidos de conferência antecipada. Não há agendamento persistente com o aplicativo fechado.
- Manter o catálogo de rotas Yoke incremental em Processamento/Yoke/rotas.md; registrar novas rotas e a origem da confirmação sem criar histórico de chamados.
- Depois de TODO reprocessamento, conferir PLT status 1,2,3,8 e entregar os IDARQUIVO das sobras ao usuário para ele setar 9. Segundo ele, podem travar o automático; normalmente não sobram. Não alterar banco por iniciativa própria.

## Backup

Guardar procedimentos e aprendizados reutilizaveis. Nao salvar historico de chamados, resultados ou pendencias, salvo pedido especifico. Scripts e manifestos sao arquivos de trabalho.

- AGENTS.md: regras da pasta.
- Jira/conexao-e-filtro.md: filtro, conexao e escrita.
- Processamento/esb-e-yoke.md: investigacao, selecao, rotas e conferencia apos reprocessamento.
- [Processamento/Yoke/rotas.md](Processamento/Yoke/rotas.md): catálogo incremental de adquirentes/layouts, rotas Yoke e caminhos de reprocessamento.
- Ambiente/codex-terminal.md: ambiente.
- [Processamento/acesso-e-execucao.md](Processamento/acesso-e-execucao.md): acessos, SCP/SSH pelo usuário e limpeza segura de temporários.
- [Processamento/disponibilizacao-sftp.md](Processamento/disponibilizacao-sftp.md): reentrada por upload, nomes sem hash, download e perfis YADE/JobScheduler.
- [Ambiente/backup-e-publicacao.md](Ambiente/backup-e-publicacao.md): seleção do conteúdo para Git e nuvem, sem credenciais ou resultados de tarefas.

Revisão de encerramento em 12/11/2026: padrões e catálogo conferidos. Este backup guarda a maneira de trabalhar, não resultados ou pendências de atendimentos. Na próxima abertura, consultar novamente o Jira para obter a situação atual.
