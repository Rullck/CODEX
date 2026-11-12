# Ambiente local de trabalho

## Retomar a sessão

No PowerShell, abrir a pasta de trabalho antes de iniciar o Codex:

```powershell
Set-Location 'C:\Users\davi.soares\Documents\GPT_BACKUP'
codex
```

- Ler AGENTS.md e o contexto indicado. Consultar Jira e banco novamente; não apresentar resultados de arquivos antigos como atuais.
- O usuário usa comandos e menus em inglês. Não presumir controles visuais que o assistente não consiga observar.
- Permissões efetivas dependem da sessão. Não afirmar que uma opção previamente habilitada continua ativa sem verificar.

## Imagens e saídas de execução

- O usuário pode compartilhar saídas de terminal e informar caminhos de capturas locais. Não solicitar senhas, tokens ou conteúdo de chaves nessas saídas.
- Para capturar uma área da tela no Windows: Win + Shift + S. Abrir a imagem local fornecida antes de interpretar seu conteúdo.
- O assistente não conecta às máquinas: seguir [acesso-e-execucao.md](../Processamento/acesso-e-execucao.md) para preparar scripts e comandos para o usuário.

## Encerramento e backup

- Não existe detecção automática de fechamento do aplicativo. Quando o usuário avisar que vai sair, atualizar somente os procedimentos e aprendizados necessários, por assunto, e verificar os arquivos gravados.
- Seguir [backup-e-publicacao.md](backup-e-publicacao.md) para selecionar o conteúdo antes de enviar ao Git e à nuvem.
