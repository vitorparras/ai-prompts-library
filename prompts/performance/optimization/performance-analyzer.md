# Analisador de Performance

> **Categoria**: performance/optimization
> **Versão**: 1.0.0

---

## Quando Usar

- Análise completa de performance de código
- Antes de deploy de código crítico
- Após identificar problemas de latência
- Revisão de código com requisitos de SLA

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código a analisar | [código fonte] | Sim |
| `{{VOLUME}}` | Volume de dados esperado | "1M registros, 10k req/s" | Não |
| `{{SLA}}` | Requisitos de performance | "P99 < 200ms" | Não |

---

## Prompt

```text
Atue como um **Performance Engineer Senior** especializado em otimização de sistemas.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente.
</contexto>

<raciocinio>
Analise sistematicamente:

1. COMPLEXIDADE ALGORÍTMICA
   - Qual o Big O de cada operação?
   - Há oportunidades de redução?

2. MEMÓRIA
   - Há alocações desnecessárias?
   - Memory leaks potenciais?

3. I/O
   - Operações síncronas que deveriam ser async?
   - Batching possível?

4. BANCO DE DADOS
   - Queries N+1?
   - Índices necessários?
   - Over-fetching?

5. CACHE
   - O que poderia ser cacheado?
   - TTL apropriado?

6. CONCORRÊNCIA
   - Paralelização possível?
   - Thread safety?
</raciocinio>

<tarefa>
Analise a performance do código:

```
{{CODIGO}}
```

**Volume esperado:** {{VOLUME}}
**SLA:** {{SLA}}
</tarefa>

<formato-de-resposta>
# Análise de Performance

## Resumo Executivo
[Status: OTIMIZADO / PRECISA ATENÇÃO / CRÍTICO]
[Principais achados em 2-3 linhas]

---

## Análise de Complexidade

| Operação | Complexidade | Impacto (N={{VOLUME}}) | Meta |
|----------|--------------|----------------------|------|
| [Op] | O(?) | [tempo estimado] | O(?) |

---

## Gargalos Identificados

### 🔴 [Crítico] [Título]
**Tipo**: [CPU/Memória/I-O/Banco]
**Impacto**: [Estimativa de latência/throughput]
**Localização**: [arquivo:linha]

**Código Atual**:
```[linguagem]
[código problemático]
```

**Código Otimizado**:
```[linguagem]
[código melhorado]
```

**Ganho Esperado**: [X% mais rápido / Xms reduzidos]

**Justificativa**: [Por que a otimização funciona]

---

### 🟠 [Alto] [Título]
...

---

## Recomendações de Cache

| Dado | Estratégia | TTL | Invalidação |
|------|------------|-----|-------------|
| [Dado] | [In-memory/Distributed] | [tempo] | [evento] |

---

## Recomendações de Índices

```sql
-- Índices sugeridos
CREATE INDEX [nome] ON [tabela]([colunas]);
```

---

## Estimativa de Impacto

| Otimização | Antes | Depois | Melhoria |
|------------|-------|--------|----------|
| [Opt 1] | Xms | Yms | Z% |

---

## Plano de Implementação

| Prioridade | Otimização | Esforço | Impacto |
|------------|------------|---------|---------|
| 1 | [Opt] | [Baixo/Médio/Alto] | [%] |

---

## Monitoramento Sugerido
[Métricas para acompanhar após otimização]
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../profiling/bottleneck-finder.md](../profiling/bottleneck-finder.md) - Identificar gargalos
- [../database-performance/query-optimizer.md](../database-performance/query-optimizer.md) - Otimizar queries
