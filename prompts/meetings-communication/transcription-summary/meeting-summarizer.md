# Resumidor de Reuniões

> **Categoria**: meetings-communication/transcription-summary
> **Versão**: 1.0.0

---

## Quando Usar

- Após reuniões para criar resumo executivo
- Para processar transcrições automáticas
- Para compartilhar key points com ausentes
- Para documentar decisões tomadas

## Quando NÃO Usar

- Para criar user stories (use discussion-to-stories)
- Para extrair apenas action items (use action-extractor)

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{TRANSCRICAO}}` | Texto da reunião | [transcrição completa] | Sim |
| `{{TIPO_REUNIAO}}` | Tipo da reunião | "Planning", "Retrospectiva", "Discovery" | Não |
| `{{PARTICIPANTES}}` | Lista de participantes | "João (PO), Maria (Dev)" | Não |

---

## Prompt

```text
Atue como um **Analista de Negócios Senior** especializado em documentação e comunicação executiva.

<objetivo>
Criar um resumo executivo claro e acionável da reunião que permita a qualquer pessoa entender rapidamente o que foi discutido e decidido.
</objetivo>

<raciocinio>
Ao processar a transcrição:

1. IDENTIFICAÇÃO DE TÓPICOS
   - Quais foram os principais assuntos discutidos?
   - Qual a ordem cronológica vs. ordem de importância?

2. EXTRAÇÃO DE DECISÕES
   - Quais decisões foram tomadas?
   - Quem tomou cada decisão?
   - Há decisões pendentes?

3. IDENTIFICAÇÃO DE AÇÕES
   - Quais ações foram definidas?
   - Quem é responsável por cada ação?
   - Há prazos definidos?

4. PONTOS DE ATENÇÃO
   - Há riscos mencionados?
   - Há bloqueios identificados?
   - Há divergências não resolvidas?
</raciocinio>

<tarefa>
Resuma a seguinte transcrição de reunião:

**Transcrição:**
```
{{TRANSCRICAO}}
```

**Tipo de reunião:** {{TIPO_REUNIAO}}
**Participantes:** {{PARTICIPANTES}}
</tarefa>

<formato-de-resposta>
# Resumo da Reunião

**Data:** [data se mencionada]
**Tipo:** {{TIPO_REUNIAO}}
**Participantes:** {{PARTICIPANTES}}
**Duração:** [se mencionada]

---

## TL;DR (Resumo em 2-3 linhas)
[Resumo ultra-conciso dos pontos principais]

---

## Tópicos Discutidos

### 1. [Tópico Principal 1]
- **Contexto**: [Por que foi discutido]
- **Pontos-chave**:
  - [Ponto 1]
  - [Ponto 2]
- **Conclusão**: [O que foi decidido ou acordado]

### 2. [Tópico Principal 2]
...

---

## Decisões Tomadas

| # | Decisão | Responsável | Impacto |
|---|---------|-------------|---------|
| D1 | [Decisão] | [Quem decidiu] | [Alto/Médio/Baixo] |

---

## Action Items

| # | Ação | Responsável | Prazo | Status |
|---|------|-------------|-------|--------|
| A1 | [Ação] | [Nome] | [Data] | Pendente |

---

## Pendências e Bloqueios

| Item | Tipo | Impacto | Próximo Passo |
|------|------|---------|---------------|
| [Item] | Pendência/Bloqueio | [Descrição] | [O que fazer] |

---

## Riscos Identificados

| Risco | Probabilidade | Impacto | Mitigação Sugerida |
|-------|--------------|---------|-------------------|
| [Risco] | Alta/Média/Baixa | Alto/Médio/Baixo | [Ação] |

---

## Próximos Passos
1. [Próximo passo 1]
2. [Próximo passo 2]

---

## Próxima Reunião
- **Data sugerida**: [se mencionada]
- **Pauta proposta**: [tópicos para próxima]
</formato-de-resposta>
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Muito longo | Reunião muito extensa | Peça foco em decisões e ações |
| Faltam detalhes | Transcrição incompleta | Forneça contexto adicional |
| Confuso | Múltiplos assuntos | Indique o tópico principal |

---

## Prompts Relacionados

- [../action-items-extractor/action-extractor.md](../action-items-extractor/action-extractor.md) - Foco em ações
- [../user-stories-generator/discussion-to-stories.md](../user-stories-generator/discussion-to-stories.md) - Criar stories
