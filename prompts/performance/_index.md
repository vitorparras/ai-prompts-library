# Performance - Prompts para Otimização

> Prompts para análise e otimização de performance de aplicações.

---

## Categorias

| Categoria | Descrição | Prompts |
|-----------|-----------|---------|
| [profiling/](profiling/) | Identificar gargalos | 1 |
| [optimization/](optimization/) | Otimizar código | 1 |
| [database-performance/](database-performance/) | Otimizar queries | 1 |
| [caching/](caching/) | Estratégias de cache | - |
| [memory-management/](memory-management/) | Gestão de memória | - |

---

## Prompts Disponíveis

| Prompt | Caso de Uso | Link |
|--------|-------------|------|
| Performance Analyzer | Análise completa de performance | [optimization/performance-analyzer.md](optimization/performance-analyzer.md) |
| Query Optimizer | Otimizar queries de banco | [database-performance/query-optimizer.md](database-performance/query-optimizer.md) |
| Bottleneck Finder | Identificar gargalos | [profiling/bottleneck-finder.md](profiling/bottleneck-finder.md) |

---

## Fluxo de Otimização

```
┌───────────────────────┐
│ Problema de Performance│
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Bottleneck Finder      │ ─────> Identificar onde está o problema
└──────────┬────────────┘
           │
     ┌─────┴─────┐
     │           │
     ▼           ▼
┌──────────┐ ┌──────────────┐
│ Query    │ │ Performance  │
│ Optimizer│ │ Analyzer     │
└──────────┘ └──────────────┘
     │           │
     └─────┬─────┘
           ▼
    [Código Otimizado]
```

---

## Métricas Chave

| Métrica | Descrição | Meta Típica |
|---------|-----------|-------------|
| Response Time P50 | Latência mediana | < 100ms |
| Response Time P99 | Latência worst case | < 500ms |
| Throughput | Requests/segundo | Varia por SLA |
| Error Rate | Taxa de erros | < 0.1% |
| CPU Usage | Uso de CPU | < 70% |
| Memory Usage | Uso de memória | < 80% |
