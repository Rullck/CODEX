# Acesso e execução pelo usuário

## Responsabilidades

- O assistente não conecta às máquinas: prepara scripts e comandos; o usuário executa SCP e SSH e compartilha a saída.
- Consultas de leitura ao banco e ao Jira continuam autorizadas conforme AGENTS.md. Escritas no Jira exigem autorização de escopo. Nunca executar exclusões ou alterações no banco apenas por preparar comandos.
- Conferir comando, seleção, origens e destino antes da execução. Não remover arquivos operacionais, origens ou backups; não sobrescrever destinos existentes. Movimentações operacionais precisam de pedido explícito.

## Sequência padrão

1. Consultar o banco e gerar um script local somente com arquivos selecionados. Se houver exclusão prévia feita pelo usuário, conferir a CRC antes de gerar o envio.
2. Validar nomes e caminhos exatos, leitura das origens, existência e escrita do destino e ausência de conflitos, antes da primeira cópia.
3. Gerar nome remoto único em `/ops/davi_tmp/` e enviar o script por SCP.
4. O usuário executa por SSH, como `davi.soares`, usando `sudo -iu services.usr`.
5. Conferir CRC e PLT após a execução. Cópia concluída não comprova processamento; seguir a regra de espera ativa e conferência em esb-e-yoke.md.
6. Depois da conferência do resultado no banco, limpar somente os temporários gerados para a tarefa, locais e remotos, pelos caminhos exatos. O usuário executa a limpeza remota.

## Máquinas

| Nome | Host | Acesso |
|---|---|---|
| ops | `10.10.20.65` | `davi.soares`, depois `sudo -iu services.usr` |
| jobscheduler | `10.10.20.24` | `davi.soares` com chave quando exigida, depois `sudo -iu services.usr` |
| OCI | `10.10.70.154` | `filemgmt-user` com chave privada |

- Chave local informada para OCI: `C:\Users\davi.soares\Documents\filemgmt-user_id_rsa`. Confirmar existência e validade antes do uso; nunca copiar seu conteúdo ao backup.
- A localização da chave de jobscheduler precisa ser informada pelo usuário; não presumir o mesmo arquivo da OCI.
- Autenticação SSH e sudo podem pedir a senha do usuário pessoal em etapas separadas. Não colocar senhas em argumentos, scripts, variáveis persistentes ou Markdown.

## Modelo PowerShell: envio e execução

Substituir o nome do script local e o identificador da tarefa. Conferir previamente se `/ops/davi_tmp/` existe e é acessível para ambos os usuários. O script enviado não deve conter segredos.

```powershell
$scriptLocal = 'C:\caminho\script-da-tarefa.sh'
$scriptRemoto = '/ops/davi_tmp/codex-' + [guid]::NewGuid().ToString('N') + '.sh'

scp "$scriptLocal" "davi.soares@10.10.20.65:$scriptRemoto"
if ($LASTEXITCODE -ne 0) {
    throw 'Falha ao enviar o script. Execução interrompida.'
}

ssh -t davi.soares@10.10.20.65 "chmod 644 '$scriptRemoto' && sudo -iu services.usr bash '$scriptRemoto'"
if ($LASTEXITCODE -ne 0) {
    throw 'Execução remota falhou. Conferir a saída antes de reenviar.'
}
```

- Usar scripts shell com finais de linha LF. Preparar cópias em blocos e mostrar progresso; a ordem do envio não garante a ordem de consumo.
- No PowerShell, `>>` indica comando ainda incompleto, frequentemente por aspas ou chaves não fechadas. A presença de `throw` no bloco não significa que houve erro; conferir a saída efetiva.
- Conservar o caminho remoto exato até a limpeza. Não procurar temporários para apagar por padrões ou curingas.

## Limpeza depois da conferência

Somente após conferir o resultado no banco, validar o caminho remoto registrado, arquivo comum, ausência de link e propriedade do usuário antes de remover. Modelo para os novos scripts:

```powershell
if ($scriptRemoto -notmatch '^/ops/davi_tmp/codex-[0-9a-f]{32}\.sh$') {
    throw 'Caminho inesperado. Nada será excluído.'
}

ssh davi.soares@10.10.20.65 "if [ -f '$scriptRemoto' ] && [ ! -L '$scriptRemoto' ] && [ -O '$scriptRemoto' ]; then rm -- '$scriptRemoto'; else echo 'Nada excluido: arquivo ausente, link ou proprietario diferente.'; fi"
```

Para a limpeza local, conferir o caminho absoluto dentro da pasta de trabalho e remover somente os arquivos gerados para a tarefa, com `Remove-Item -LiteralPath`, sem recursão ou curingas. Não limpar enquanto houver necessidade de diagnóstico ou conferência pendente.

## Configurações de conexão preservadas

- Banco: `C:\Users\davi.soares\.oracle\producao.json` e `producao.credential.xml` no mesmo diretório; carregar via `Import-Clixml`, usando a senha somente em memória e sem impressão.
- Jira: autenticação pelo plugin Atlassian para `https://equals.atlassian.net`; não copiar tokens para o backup.
- Credencial SSH local anteriormente criada: `C:\Users\davi.soares\.ssh-ops.credential.xml`. É configuração protegida do Windows, não temporário de tarefa; sua existência não autoriza o assistente a conectar às máquinas.
- Preservar credenciais, chaves, configurações SSH e demais configurações de conexão durante toda limpeza.
