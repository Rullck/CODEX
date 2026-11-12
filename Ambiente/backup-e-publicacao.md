# Backup e publicação

## Conteúdo mantido

- Registrar somente procedimentos reutilizáveis, preferências, seleção de arquivos, consultas padrão e caminhos das configurações necessários para retomar o trabalho.
- O Markdown é um resumo de trabalho, não transcrição literal da conversa.
- Não incluir senhas, tokens, conteúdo de chaves privadas, arquivos de credencial, históricos de chamados, listas de pendências, resultados operacionais ou anexos de clientes.
- Caminhos de configurações e referências a credenciais locais são permitidos; os arquivos de credencial permanecem fora do backup.
- Scripts, manifestos e relatórios gerados para tarefas são temporários de trabalho. Não enviá-los ao Git ou à nuvem como documentação; limpar somente após conferir o resultado no banco.
- Preservar credenciais, chaves e configurações de conexão durante a limpeza. Os acessos locais e plugins independem do Markdown.

## Preparação do envio

1. Atualizar e verificar os Markdown por assunto e o índice README.md.
2. Selecionar explicitamente a documentação; não incluir a pasta inteira. A pasta legada Chamados e os arquivos operacionais ficam fora da publicação, salvo pedido específico.
3. Conferir que os arquivos selecionados não contêm segredos nem resultados de tarefas. A documentação contém hosts e caminhos internos e deve ser tratada como material interno.
4. Quando o usuário avisar que vai sair, atualizar a documentação necessária, criar commit e fazer push para `https://github.com/Rullck/CODEX.git`. O usuário autorizou o envio ao Git em todos os encerramentos; não pedir nova autorização para esse mesmo destino e escopo.
5. Usar a identidade pessoal configurada somente no repositório: `Davi <rullckm@gmail.com>`. Conferir o remoto e a identidade antes de publicar; não reutilizar identidade corporativa. Não usar push forçado nem incluir arquivos operacionais. Se houver divergência remota, conferir antes de integrar.
6. A publicação na nuvem continua pendente de identificar serviço e destino. Não presumir que push ao Git também salve na nuvem pessoal.
7. Verificar cada envio e distinguir arquivos salvos localmente, commits locais e uploads confirmados. Confirmar igualdade do commit local e da referência remota após o push.
