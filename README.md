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
- **Gera dashboard HTML interativo** com gráficos, filtros e tabela detalhada

---

## Como usar

### 1. Executar a skill (coletar os dados)

1. Faça login manualmente no portal Bradesco Saúde
2. Abra o Claude na extensão Chrome
3. Cole o comando abaixo no chat, substituindo os dados fictícios pelos seus:

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

O Claude irá gerar e baixar um arquivo JSON com todos os reembolsos.

### 2. Visualizar o dashboard

1. Baixe ou clone este repositório
2. Abra o arquivo `report/index.html` no seu navegador (duplo clique — sem servidor necessário)
3. Clique em **"Carregar JSON"** e selecione o arquivo gerado pelo Claude
4. Explore os dados com gráficos, filtros e tabela interativa

---

## Dashboard (`report/index.html`)

O dashboard é um arquivo HTML + JavaScript puro, sem dependências além de Chart.js (carregado via CDN).

**Funcionalidades:**

- **Cards de resumo**: total de registros, valor solicitado, valor reembolsado, diferença, contagem por status
- **Abas por beneficiário**: filtra tudo para titular, dependente1, dependente2 ou todos
- **Gráfico de barras**: quantidade de reembolsos por mês (data de competência)
- **Gráfico de rosca**: distribuição por status (Pago, Pago Parcial, Em Análise, Rejeitado)
- **Gráfico de comparação**: valor solicitado vs reembolsado por mês
- **Tabela detalhada**: ordenável por qualquer coluna, com filtro por texto, status, ano e tipo de atendimento
- **Badges de alerta**: ⚠️ para rejeitados com motivo, 🔔 para registros que precisam de ação
- **Drag & drop**: arraste o JSON diretamente na tela
- **Privacidade**: todos os dados ficam 100% no seu navegador — nada é enviado a servidores

---

## Estrutura do repositório

```
bradesco-reembolso-monitor/
├── README.md
├── bradesco_reembolso_monitor/
│   ├── skill.md               ← Skill completa com todos os comandos e ações
│   └── exemplo_json.json      ← Exemplo da estrutura de dados (dados fictícios)
└── report/
    └── index.html             ← Dashboard HTML interativo
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

## Ações suportadas pela skill

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
- O dashboard abre localmente — nenhum dado sai do seu navegador

---

*Desenvolvido para uso com [Claude AI](https://claude.ai) + extensão Chrome*
