# 🔍 Bradesco Reembolso Monitor

> **Skill para monitorar e acompanhar reembolsos do plano Bradesco Saúde usando Claude AI**

⚠️ **Esta skill NÃO solicita reembolsos** — ela rastreia e cataloga reembolsos já existentes no portal.

---

## O que faz

- Cataloga todos os reembolsos por beneficiário e período
- Extrai a **data de competência** real (data da consulta/terapia) da Carta Resultado — não apenas a data de solicitação
- Gera um JSON estruturado com todos os dados
- Identifica reembolsos rejeitados e captura o motivo
- Suporta múltiplos beneficiários (titular + dependentes)
- Gera dashboard HTML consolidado

## Por que isso é necessário

O portal Bradesco Saúde limita a tela de "Solicitações Recentes" a apenas **10 itens**. Esta skill contorna isso fazendo buscas mês a mês via pesquisa avançada, garantindo que nenhum reembolso seja perdido.

---

## Como usar

### Pré-requisitos

- Extensão [Claude Chrome Extension](https://chrome.google.com/webstore/detail/claude-ai/...) instalada
- Acesso ao portal [Bradesco Saúde](https://wwws.bradescosaude.com.br)
- Conta ativa no plano Bradesco Saúde

### Passo a passo

1. **Faça login** manualmente no portal Bradesco Saúde
2. **Abra o Claude** na extensão Chrome
3. **Cole o comando** abaixo no chat, substituindo os dados fictícios pelos seus:

```
SKILL: bradesco_reembolso_monitor
VERSÃO: 1.0

CONTEXTO:
- Site: https://wwws.bradescosaude.com.br
- Já estou logado. Pode assumir a aba.

BENEFICIÁRIOS E CARTÕES:
- titular:     XXXXXXXXXXXXXXXXXX  (ex: João Silva - Titular)
- dependente1: XXXXXXXXXXXXXXXXXX  (ex: Maria Silva - Dependente)
- dependente2: XXXXXXXXXXXXXXXXXX  (ex: Pedro Silva - Dependente)

AÇÃO: catalogar

PARÂMETROS:
- beneficiario: todos
- data_inicio: 01/2025
- data_fim: 04/2026
```

---

## Estrutura do repositório

```
bradesco-reembolso-monitor/
├── README.md                           # Este arquivo
└── bradesco_reembolso_monitor/
    ├── skill.md                        # Skill completa com todos os comandos e ações
    └── exemplo_json.json               # Exemplo da estrutura de dados gerada
```

---

## Dados gerados (exemplo fictício)

```json
{
  "metadata": {
    "beneficiario": "joao_silva",
    "periodo": "01/2025 - 04/2026",
    "total_registros": 12,
    "total_solicitado": 4500.00,
    "total_reembolsado": 3800.00,
    "diferenca": -700.00
  },
  "reembolsos": [
    {
      "protocolo": "2025.0001234567.00",
      "data_solicitacao": "15/01/2025",
      "data_competencia": "10/01/2025",
      "beneficiario": "joao_silva",
      "tipo_atendimento": "Consultas",
      "valor_solicitado": 350.00,
      "valor_reembolsado": 280.00,
      "diff": -70.00,
      "status": "Pago",
      "status_categoria": "pago"
    }
  ]
}
```

---

## Ações suportadas

| Ação | Descrição |
|------|-----------|
| `catalogar` | Busca mês a mês e extrai todos os dados |
| `corrigir` | Recupera data_competencia de protocolos específicos |
| `continuar` | Retoma de onde parou |
| `gerar_json` | Gera/atualiza o JSON da sessão atual |
| `dashboard` | Gera dashboard HTML consolidado |

---

## Notas de privacidade

- **Nunca** commite seus números de cartão reais neste repositório
- O repositório usa apenas dados fictícios como exemplo
- Seus dados reais ficam apenas no chat local com Claude
- O JSON gerado é baixado para o seu computador

---

## Contribuindo

Sinta-se livre para abrir issues ou PRs com melhorias!

---

*Desenvolvido para uso com [Claude AI](https://claude.ai) + extensão Chrome*
