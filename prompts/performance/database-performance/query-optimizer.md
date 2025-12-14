# Otimizador de Queries

> **Categoria**: performance/database-performance
> **Versão**: 1.0.0

---

## Quando Usar

- Queries lentas identificadas
- Otimização de relatórios
- Alto consumo de banco
- Análise de EXPLAIN plans

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{QUERY}}` | Query a otimizar | [SQL ou código ORM] | Sim |
| `{{SCHEMA}}` | Schema das tabelas | [DDL ou descrição] | Não |
| `{{VOLUME}}` | Volume de dados | "tabela com 10M registros" | Não |
| `{{EXPLAIN}}` | Output do EXPLAIN | [plano de execução] | Não |

---

## Prompt

```text
Atue como um **DBA Senior** especializado em otimização de queries.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente.
</contexto>

<raciocinio>
Analise a query considerando:

1. PLANO DE EXECUÇÃO
   - Há table scans desnecessários?
   - Índices estão sendo usados?
   - Ordem de JOINs é eficiente?

2. ESTRUTURA DA QUERY
   - Há SELECT * desnecessário?
   - Subqueries podem ser JOINs?
   - CTEs ou temp tables ajudariam?

3. ÍNDICES
   - Índices necessários existem?
   - Índices compostos seriam melhores?
   - Índices covering possíveis?

4. N+1 E BATCHING
   - Há pattern N+1?
   - Pode ser consolidado?

5. PAGINAÇÃO
   - Paginação está implementada?
   - Offset vs Keyset pagination?
</raciocinio>

<tarefa>
Otimize a seguinte query:

```sql
{{QUERY}}
```

**Schema:**
```sql
{{SCHEMA}}
```

**Volume:** {{VOLUME}}

**EXPLAIN (se disponível):**
```
{{EXPLAIN}}
```
</tarefa>

<formato-de-resposta>
# Otimização de Query

## Análise da Query Original

### Problemas Identificados
| # | Problema | Impacto | Severidade |
|---|----------|---------|------------|
| 1 | [Problema] | [Descrição do impacto] | 🔴/🟠/🟡 |

### Análise do Plano de Execução
[Se EXPLAIN foi fornecido, análise detalhada]

---

## Query Otimizada

```sql
-- Query otimizada
[nova query]
```

### Mudanças Realizadas

| # | Mudança | Motivo | Ganho Esperado |
|---|---------|--------|----------------|
| 1 | [Mudança] | [Por que] | [X%/Xms] |

---

## Índices Recomendados

```sql
-- Índices a criar
CREATE INDEX idx_[nome] ON [tabela]([colunas]);

-- Justificativa: [por que este índice ajuda]
```

---

## Comparação

| Métrica | Antes | Depois | Melhoria |
|---------|-------|--------|----------|
| Tempo estimado | Xms | Yms | Z% |
| Linhas lidas | X | Y | Z% |
| Uso de índice | Não/Parcial | Sim | - |

---

## Alternativas Consideradas

### Alternativa 1: [Nome]
```sql
[query alternativa]
```
**Prós**: [vantagens]
**Contras**: [desvantagens]
**Por que não escolhida**: [motivo]

---

## Observações para Produção
- [Consideração sobre lock]
- [Consideração sobre migração de índice]
- [Horário recomendado para executar DDL]
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../optimization/performance-analyzer.md](../optimization/performance-analyzer.md) - Análise geral
- [../profiling/bottleneck-finder.md](../profiling/bottleneck-finder.md) - Identificar gargalos
