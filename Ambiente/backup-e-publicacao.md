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
4. Publicar primeiro no Git e depois na nuvem.
5. Confirmar repositório e destino de nuvem quando ainda não estiverem identificados. Não presumir provedor, visibilidade pública ou pasta de destino.
6. Verificar cada envio e distinguir arquivos salvos localmente, commits locais e uploads confirmados.
