# Catálogo de rotas Yoke

Consulta rápida: localizar a adquirente ou o layout na tabela e usar o destino de reprocessamento. Acrescentar novas linhas conforme as rotas forem identificadas. Este catálogo guarda padrões, não históricos de chamados.

## Rotas conhecidas

| Adquirente / layout | Nome da rota | Caminho de reprocessamento | Confirmação |
|---|---|---|---|
| Cielo v15 | `cielov15` | `/fileproc-ora-esb04/crc/microservice/cielov15/reprocessing_files/` | Informado pelo usuário e observado na fila da CRC |
| Banrisul / Vero / Banricompras | `banrisul` | `/fileproc-ora-esb04/crc/microservice/banrisul/reprocessing_files/` | Informado pelo usuário e observado na fila da CRC |
| Shellbox | `shellbox` | `/fileproc-ora-esb04/crc/microservice/shellbox/reprocessing_files/` | Informado pelo usuário e observado na fila da CRC |
| Nupay | `nupay` | `/fileproc-ora-esb04/crc/microservice/nupay/reprocessing_files/` | Informado pelo usuário, diretório conferido pela ops e fila `PRODUCAO-REPROCESSAMENTO-YOKE-SMALL-nupay.fifo` observada na CRC |
| Pix Santander | `santander-pix` | `/fileproc-ora-esb04/crc/microservice/santander-pix/reprocessing_files/` | Informado pelo usuário e observado na fila da CRC |
| Layout Equals — inclui Personal Card nesse layout | `layout-equals` | `/fileproc-ora-esb04/crc/microservice/layout-equals/reprocessing_files/` | Informado pelo usuário e observado na fila da CRC |

## Como incrementar

1. Buscar primeiro a adquirente e o layout nesta tabela; não criar rotas pelo nome comercial presumido.
2. Adicionar uma linha com adquirente/layout, nome exato da rota, caminho completo e origem da confirmação.
3. Se o usuário informar a rota, registrar como informada. Após observar a fila na CRC, atualizar a confirmação para incluir essa evidência.
4. Se surgir outro layout/versão para a mesma adquirente, manter uma linha separada quando a rota for diferente.
5. Manter regras de seleção e conferência em [esb-e-yoke.md](../esb-e-yoke.md). Não incluir contratos, IDs de arquivos ou listas de atendimentos neste catálogo.

Modelo de nova linha:

```text
| Adquirente / layout | `nome-exato-da-rota` | `/fileproc-ora-esb04/crc/microservice/nome-exato-da-rota/reprocessing_files/` | Informado pelo usuário / observado na CRC |
```

## Padrão e identificação

Destino: `/fileproc-ora-esb04/crc/microservice/<rota>/reprocessing_files/`.

Exemplo de fila observada: `PRODUCAO-REPROCESSAMENTO-YOKE-SMALL-shellbox.fifo`. Conferir `NMFILAPROCESSAMENTO` na CRC para identificar a rota efetiva; não presumir o porte da fila para outras rotas.

Manter o prefixo `fileproc` no Yoke. A regra de usar `fileprocessing` no ESBORA02 não se aplica automaticamente aqui.

### Layout Equals / Personal Card

O nome da adquirente pode ser Personal Card enquanto a rota é `layout-equals`; identificar o layout do arquivo. Segundo o usuário, configurar o arquivo esperado por dia no retaguarda como “Único” em vez de “Layout Equals” pode deixar esses arquivos ignorados (status 9). Verificar a configuração antes de atribuir uma causa a todos os arquivos com esse status; alteração no retaguarda depende de ação autorizada.
