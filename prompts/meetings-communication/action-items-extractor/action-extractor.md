# Extrator de Action Items

> **Categoria**: meetings-communication/action-items-extractor
> **Versão**: 1.0.0

---

## Quando Usar

- Após reuniões para extrair tarefas
- Para processar notas de reunião
- Para criar lista de follow-ups
- Para alimentar ferramentas de gestão (Jira, Trello, etc.)

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{TEXTO}}` | Transcrição ou notas | [texto da reunião] | Sim |
| `{{FORMATO_SAIDA}}` | Formato desejado | "lista", "tabela", "jira" | Não |

---

## Prompt

```text
Atue como um **Gerente de Projetos Senior** especializado em extração e organização de tarefas.

<objetivo>
Extrair TODAS as ações mencionadas ou implícitas no texto, organizando-as de forma clara e acionável.
</objetivo>

<raciocinio>
Ao analisar o texto, identifique:

1. AÇÕES EXPLÍCITAS
   - Frases como "vou fazer", "preciso", "temos que"
   - Compromissos assumidos
   - Tarefas delegadas

2. AÇÕES IMPLÍCITAS
   - Problemas mencionados que precisam de solução
   - Decisões que requerem implementação
   - Dependências mencionadas

3. RESPONSÁVEIS
   - Quem se comprometeu?
   - Quem foi designado?
   - Se não claro, quem seria o responsável lógico?

4. PRAZOS
   - Prazos explícitos mencionados
   - Urgência implícita
   - Dependências de prazo
</raciocinio>

<tarefa>
Extraia os action items do seguinte texto:

```
{{TEXTO}}
```

Formato de saída: {{FORMATO_SAIDA}}
</tarefa>

<formato-de-resposta>
## Action Items Extraídos

### Alta Prioridade (Urgente)
| # | Ação | Responsável | Prazo | Contexto |
|---|------|-------------|-------|----------|
| 1 | [Ação clara e específica] | [Nome] | [Data/ASAP] | [De onde veio] |

### Média Prioridade
| # | Ação | Responsável | Prazo | Contexto |
|---|------|-------------|-------|----------|

### Baixa Prioridade / Nice to Have
| # | Ação | Responsável | Prazo | Contexto |
|---|------|-------------|-------|----------|

---

## Ações Sem Responsável Definido
[Ações que precisam de alguém designado]

| Ação | Sugestão de Responsável | Motivo |
|------|------------------------|--------|

---

## Dependências Identificadas
[Ações que dependem de outras]

| Ação | Depende de | Bloqueada por |
|------|------------|---------------|

---

## Para Jira/Trello (se solicitado)
```
[Formato de importação para ferramentas]
```
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../transcription-summary/meeting-summarizer.md](../transcription-summary/meeting-summarizer.md) - Resumo completo
- [../user-stories-generator/discussion-to-stories.md](../user-stories-generator/discussion-to-stories.md) - Criar stories
