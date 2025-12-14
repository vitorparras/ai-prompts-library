# Identificador de Gargalos

> **Categoria**: performance/profiling
> **Versão**: 1.0.0

---

## Quando Usar

- Identificar causa de lentidão
- Priorizar otimizações
- Análise de profiling
- Antes de otimizar (evitar otimização prematura)

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código ou fluxo a analisar | [código/descrição] | Sim |
| `{{SINTOMAS}}` | Sintomas observados | "Endpoint lento, timeout frequente" | Não |
| `{{METRICAS}}` | Métricas disponíveis | [output de profiler] | Não |

---

## Prompt

```text
Atue como um **Performance Engineer Senior** especializado em identificação de gargalos.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente.
</contexto>

<raciocinio>
Analise sistematicamente onde o tempo/recursos estão sendo gastos:

1. MAPEAMENTO DO FLUXO
   - Qual o caminho crítico?
   - Onde há operações sequenciais vs paralelas?

2. CATEGORIZAÇÃO
   - CPU-bound: Cálculos intensivos
   - I/O-bound: Disco, rede, banco
   - Memory-bound: Alocações, GC

3. ANÁLISE DE TEMPO
   - Onde o tempo está sendo gasto?
   - O que é síncrono que poderia ser async?
   - O que é sequencial que poderia ser paralelo?

4. ANÁLISE DE DEPENDÊNCIAS
   - Chamadas externas (APIs, banco)?
   - Latência de rede?
   - Contenção de recursos?

5. QUICK WINS
   - O que pode ser corrigido rapidamente?
   - Maior impacto com menor esforço?
</raciocinio>

<tarefa>
Identifique os gargalos de performance:

```
{{CODIGO}}
```

**Sintomas observados:** {{SINTOMAS}}

**Métricas/Profiling:**
```
{{METRICAS}}
```
</tarefa>

<formato-de-resposta>
# Análise de Gargalos

## Fluxo Mapeado
```
[Diagrama ASCII ou descrição do fluxo]
[Entrada] → [Passo 1: Xms] → [Passo 2: Yms] → [Saída]
```

---

## Gargalos Identificados

| # | Gargalo | Tipo | % do Tempo | Impacto |
|---|---------|------|------------|---------|
| 1 | [Nome] | CPU/IO/Mem | X% | Alto/Médio/Baixo |

---

### G1: [Nome do Gargalo Principal]

**Tipo**: [CPU-bound / I/O-bound / Memory-bound]
**Tempo Consumido**: ~Xms (Y% do total)
**Localização**: [arquivo:linha ou componente]

**Por que é gargalo**:
[Explicação técnica]

**Evidências**:
- [Evidência 1 das métricas]
- [Evidência 2 do código]

**Impacto no Sistema**:
- Latência: +Xms
- Throughput: -Y%
- Recursos: [descrição]

**Otimização Sugerida**:
```[linguagem]
[código ou abordagem]
```

**Ganho Esperado**: [X% / Xms]

---

### G2: [Segundo Gargalo]
...

---

## Distribuição de Tempo

```
[Diagrama ASCII de distribuição]
Operação A: ████████████████████ 40%
Operação B: ██████████ 20%
Operação C: ████████████████ 30%
Outros:     █████ 10%
```

---

## Priorização de Correções

| Prioridade | Gargalo | Impacto | Esforço | ROI |
|------------|---------|---------|---------|-----|
| 1 | [G1] | Alto | Baixo | Alto |
| 2 | [G2] | Médio | Médio | Médio |

---

## Quick Wins
[Correções rápidas de alto impacto]

1. [Quick win 1]: [ganho esperado]
2. [Quick win 2]: [ganho esperado]

---

## Investigações Adicionais Necessárias
[O que precisa de mais dados para diagnosticar]

1. [Área que precisa profiling mais detalhado]
2. [Métricas adicionais necessárias]
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../optimization/performance-analyzer.md](../optimization/performance-analyzer.md) - Otimizar código
- [../database-performance/query-optimizer.md](../database-performance/query-optimizer.md) - Otimizar queries
