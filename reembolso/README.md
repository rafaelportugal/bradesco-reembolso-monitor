# reembolso/

Esta pasta é onde você deve colocar os arquivos JSON exportados do portal Bradesco Saúde.

## Como usar

1. Baixe os arquivos JSON gerados pelo script de catalogação (um por beneficiário)
2. Coloque-os nesta pasta localmente no seu computador
3. Abra `report/index.html` no navegador
4. Clique em **"🗂️ Abrir Pasta"** e selecione esta pasta — todos os JSONs serão carregados de uma vez
5. Ou clique em **"📄 Selecionar Arquivos"** para escolher arquivos individuais

## Nomes esperados dos arquivos

| Arquivo | Beneficiário |
|---------|-------------|
| `theo_reembolsos_catalog_*.json` | Theo |
| `rafael_reembolsos_catalog_*.json` | Rafael |
| `nathalia_reembolsos_catalog_*.json` | Nathalia |

> **Nota:** Esta pasta está no `.gitignore` para proteger dados pessoais. Os JSONs reais **não são versionados** no repositório — apenas o `README.md` de instruções é mantido aqui.

## Estrutura do JSON

Cada arquivo JSON deve conter um array de objetos com a seguinte estrutura:

```json
[
  {
    "protocolo": "2025.0001234567.00",
    "data_solicitacao": "10/01/2025",
    "data_competencia": "05/01/2025",
    "beneficiario": "nome_beneficiario",
    "tipo_atendimento": "Psicoterapia",
    "valor_solicitado": 200.00,
    "valor_reembolsado": 180.00,
    "diff": -20.00,
    "status": "Pago",
    "status_categoria": "pago",
    "carta_resultado": "disponivel",
    "acao_necessaria": false,
    "motivo_rejeicao": null,
    "motivo_entendido": null
  }
]
```

Consulte `bradesco_reembolso_monitor/exemplo_json.json` para um exemplo completo com dados fictícios.
