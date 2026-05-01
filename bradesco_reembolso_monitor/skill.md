# Skill: bradesco_reembolso_monitor
# Versão: 1.0
# Propósito: MONITORAR e ACOMPANHAR reembolsos do plano Bradesco Saúde
# ⚠️ Esta skill NÃO é para solicitar reembolsos — apenas para rastrear os existentes.

## Como usar

1. Faça login manualmente no portal Bradesco Saúde
2. Copie e cole o bloco abaixo no chat com Claude (extensão Chrome)
3. Substitua os parâmetros pelos valores desejados
4. Claude irá executar o fluxo automaticamente

---

## Comando da Skill

```
SKILL: bradesco_reembolso_monitor
VERSÃO: 1.0

CONTEXTO:
- Site: https://wwws.bradescosaude.com.br
- Já estou logado. Pode assumir a aba.

BENEFICIÁRIOS E CARTÕES (substitua pelos seus dados reais):
- titular:     XXXXXXXXXXXXXXXXXX  (ex: João Silva - Titular)
- dependente1: XXXXXXXXXXXXXXXXXX  (ex: Maria Silva - Dependente)
- dependente2: XXXXXXXXXXXXXXXXXX  (ex: Pedro Silva - Dependente)

AÇÃO: [catalogar | corrigir | continuar | gerar_json | dashboard]

PARÂMETROS:
- beneficiario: [titular | dependente1 | dependente2 | todos]
- data_inicio:  MM/YYYY  (ex: 01/2025)
- data_fim:     MM/YYYY  (ex: 12/2025)
- corrigir_protocolos: []  (lista de protocolos com data_competencia = null)

INSTRUÇÃO ESPECIAL: (opcional)
```

---

## Ações disponíveis

| Ação | Descrição |
|------|-----------|
| catalogar | Busca mês a mês, abre cada Carta Resultado, extrai data_competencia, gera JSON |
| corrigir | Reabre protocolos específicos para capturar data_competencia faltando |
| continuar | Retoma a partir do último mês processado |
| gerar_json | Reconstrói o JSON a partir dos dados já coletados na sessão |
| dashboard | Consolida todos os JSONs e gera dashboard HTML |

---

## Exemplos de uso

### Catalogar tudo desde janeiro de 2025:
```
SKILL: bradesco_reembolso_monitor
AÇÃO: catalogar
beneficiario: todos
data_inicio: 01/2025
data_fim: 12/2025
```

### Catalogar apenas um beneficiário:
```
SKILL: bradesco_reembolso_monitor
AÇÃO: catalogar
beneficiario: titular
data_inicio: 01/2025
data_fim: 04/2026
```

### Corrigir protocolos com data_competencia faltando:
```
SKILL: bradesco_reembolso_monitor
AÇÃO: corrigir
beneficiario: titular
corrigir_protocolos: ["YYYY.XXXXXXXXXX.NN", "YYYY.XXXXXXXXXX.NN"]
```

### Gerar dashboard consolidado:
```
SKILL: bradesco_reembolso_monitor
AÇÃO: dashboard
beneficiario: todos
```

---

## Estrutura do JSON gerado

```json
{
  "metadata": {
    "gerado_em": "DD/MM/YYYY HH:MM",
    "beneficiario": "nome_beneficiario",
    "periodo": "MM/YYYY - MM/YYYY",
    "total_registros": 0,
    "total_solicitado": 0.00,
    "total_reembolsado": 0.00,
    "diferenca": 0.00
  },
  "reembolsos": [
    {
      "protocolo": "YYYY.XXXXXXXXXX.NN",
      "data_solicitacao": "DD/MM/YYYY",
      "data_competencia": "DD/MM/YYYY",
      "beneficiario": "nome_beneficiario",
      "tipo_atendimento": "Consultas | Terapias Multidisciplinares | ...",
      "valor_solicitado": 0.00,
      "valor_reembolsado": 0.00,
      "diff": 0.00,
      "status": "Pago | Pago Parcialmente | Em Análise | Não autorizado",
      "status_categoria": "pago | pago_parcial | em_analise | rejeitado",
      "carta_resultado": "disponivel | null",
      "acao_necessaria": false,
      "motivo_rejeicao": null,
      "motivo_entendido": null
    }
  ]
}
```

---

## Campos importantes

- **data_solicitacao**: Data em que o reembolso foi solicitado no portal
- **data_competencia**: Data real do atendimento/terapia (extraída da Carta Resultado, campo "Data do Evento") — diferente da data de solicitação!
- **motivo_rejeicao**: Preenchido automaticamente para registros "Não autorizado"
- **motivo_entendido**: Deixado como null para que o usuário confirme manualmente após revisar

---

## Notas técnicas

- O portal limita "Solicitações Recentes" a 10 itens — por isso a busca é feita mês a mês
- A seleção de cartão requer injeção via JavaScript (o campo hidden bs-reembolso-cartao-id não é populado pelo clique visual)
- Registros "Em Análise" não têm Carta Resultado ainda — data_competencia será capturada em execução futura
- Após processar as cartas de um mês, use "Nova Pesquisa" para retornar ao formulário de busca
