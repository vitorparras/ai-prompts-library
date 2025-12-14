# Gerador de Relatórios de Status

> **Categoria**: meetings-communication/status-reports
> **Versão**: 1.0.0

---

## Quando Usar

- Reports semanais de progresso
- Status para stakeholders
- Atualizações de projeto
- Comunicação com gestão

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{PERIODO}}` | Período do report | "Semana 01-07/Jan" | Sim |
| `{{REALIZACOES}}` | O que foi feito | [lista de entregas] | Sim |
| `{{PROBLEMAS}}` | Bloqueios/issues | [lista de problemas] | Não |
| `{{PROXIMOS}}` | Próximos passos | [lista de próximas ações] | Não |

---

## Prompt

```text
Atue como um **Gerente de Projetos Senior** especializado em comunicação executiva.

<tarefa>
Crie um relatório de status profissional e conciso:

**Período:** {{PERIODO}}

**Realizações:**
{{REALIZACOES}}

**Problemas/Bloqueios:**
{{PROBLEMAS}}

**Próximos Passos:**
{{PROXIMOS}}
</tarefa>

<formato-de-resposta>
# Status Report - {{PERIODO}}

## Resumo Executivo
[1-2 frases sobre o estado geral: On Track / At Risk / Blocked]

## Principais Realizações
| Entrega | Status | Impacto |
|---------|--------|---------|
| [Item] | Concluído/Em andamento | [Benefício] |

## Métricas (se aplicável)
| Métrica | Meta | Atual | Tendência |
|---------|------|-------|-----------|
| [Métrica] | [X] | [Y] | ↑/↓/→ |

## Riscos e Bloqueios
| Item | Severidade | Mitigação | Responsável |
|------|------------|-----------|-------------|
| [Risco/Bloqueio] | Alta/Média/Baixa | [Ação] | [Nome] |

## Próximos Passos (Próximo Período)
1. [Ação 1] - [Responsável]
2. [Ação 2] - [Responsável]

## Necessidades de Suporte
[O que precisa de ajuda/aprovação]

---
*Report gerado em [data]*
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../professional-emails/email-composer.md](../professional-emails/email-composer.md) - Para email do report
