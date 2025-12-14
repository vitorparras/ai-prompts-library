# Revisor de Performance

> **Categoria**: developers/code-review
> **Versão**: 1.0.0

---

## Quando Usar

- Revisar código que processa grandes volumes de dados
- Revisar queries de banco de dados
- Revisar código de hotspots identificados
- Antes de deploy de código crítico para performance
- Após identificar gargalos via profiling

## Quando NÃO Usar

- Análise de qualidade geral (use quality-reviewer)
- Código que não é performance-critical
- Otimização prematura sem evidência de problema

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| API de alta latência | Foco em async, caching |
| Processamento de dados | Foco em algoritmos, memória |
| Banco de dados | Foco em queries, índices |
| Real-time | Foco em latência P99 |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código a ser analisado | [código fonte] | Sim |
| `{{VOLUME}}` | Volume de dados esperado | "100k registros, 1k req/s" | Não |
| `{{SLA}}` | Requisitos de performance | "< 100ms P95" | Não |

---

## Prompt

```text
Atue como um **Performance Engineer Senior** especializado em otimização de código e sistemas.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Considere a infraestrutura e padrões de performance já estabelecidos.
</contexto>

<raciocinio>
Conduza a análise de performance sistematicamente:

1. ANÁLISE DE COMPLEXIDADE
   - Qual a complexidade algorítmica (Big O)?
   - Há loops aninhados?
   - Há operações O(n²) ou piores?
   - Pode ser reduzido?

2. ANÁLISE DE MEMÓRIA
   - Há alocações desnecessárias?
   - Objetos grandes em loops?
   - Possíveis memory leaks?
   - Uso adequado de pooling?

3. ANÁLISE DE I/O
   - Há operações de I/O em loops?
   - Async/await usado corretamente?
   - Batching aplicado onde possível?
   - Connection pooling?

4. ANÁLISE DE BANCO
   - Há queries N+1?
   - Índices necessários existem?
   - Dados desnecessários carregados?
   - Paginação implementada?

5. ANÁLISE DE CACHE
   - O que poderia ser cacheado?
   - Cache já existe e está correto?
   - TTL apropriado?
   - Invalidação correta?
</raciocinio>

<tarefa>
Analise o seguinte código do ponto de vista de performance:

```
{{CODIGO}}
```

Volume de dados esperado:
{{VOLUME}}

Requisitos de SLA:
{{SLA}}
</tarefa>

<criterios-de-avaliacao>
Classifique cada problema:

🔴 CRÍTICO: Causa degradação severa ou timeout
🟠 ALTO: Impacto significativo em latência/throughput
🟡 MÉDIO: Impacto moderado, otimização recomendada
🟢 BAIXO: Melhoria marginal, nice-to-have

Para cada problema:
- Impacto estimado em ms ou %
- Condições para manifestação
- Custo-benefício da correção
</criterios-de-avaliacao>

<formato-de-resposta>
## Resumo de Performance
[Avaliação geral do código]
[Principais gargalos identificados]

## Análise de Complexidade

| Operação | Complexidade Atual | Ideal | Impacto |
|----------|-------------------|-------|---------|
| | O(?) | O(?) | |

## Problemas Identificados

### 🔴 [CRÍTICO] Título do Problema
- **Tipo**: [algoritmo/memória/I-O/banco/cache]
- **Localização**: [arquivo:linha]
- **Complexidade**: O(?) → Deveria ser O(?)
- **Impacto Estimado**: [latência, memória, CPU]
- **Condição**: [Quando o problema se manifesta]
- **Código Problemático**:
```[linguagem]
[código atual]
```
- **Código Otimizado**:
```[linguagem]
[código melhorado]
```
- **Justificativa**: [Por que é mais eficiente, com números]
- **Benchmark Estimado**: [antes vs depois]

### 🟠 [ALTO] Título
...

## Análise de Queries

| Query/Operação | Problema | N para impacto | Otimização |
|----------------|----------|----------------|------------|
| | | | |

## Análise de Memória

| Alocação | Frequência | Tamanho | Problema | Solução |
|----------|------------|---------|----------|---------|
| | | | | |

## Oportunidades de Cache

| Dado | Frequência de Acesso | TTL Sugerido | Estratégia |
|------|---------------------|--------------|------------|
| | | | |

## Recomendações por Prioridade

### Imediato (Crítico)
1. [Correção 1]
2. [Correção 2]

### Curto Prazo (Alto)
1. [Melhoria 1]
2. [Melhoria 2]

### Médio Prazo (Médio/Baixo)
1. [Otimização 1]
2. [Otimização 2]

## Métricas para Monitorar
| Métrica | Valor Atual Estimado | Meta | Como Medir |
|---------|---------------------|------|------------|
| | | | |

## Próximos Passos
1. [Ação 1]
2. [Ação 2]
</formato-de-resposta>

<justificativas>
Para cada otimização:
1. Explique por que a solução atual é ineficiente
2. Cite a técnica de otimização aplicada
3. Forneça estimativas numéricas quando possível
4. Explique trade-offs (se houver)

Exemplo: "Substituindo List.Contains() por HashSet.Contains() - reduz complexidade
de O(n) para O(1) por lookup. Para 10k itens, redução de ~5ms para <0.1ms por busca."
</justificativas>
```

---

## Exemplo de Uso

### Input

```csharp
{{CODIGO}}:
public List<OrderDto> GetOrdersWithDetails(List<int> orderIds)
{
    var result = new List<OrderDto>();

    foreach (var orderId in orderIds)
    {
        var order = _context.Orders.Find(orderId);
        var items = _context.OrderItems.Where(i => i.OrderId == orderId).ToList();
        var customer = _context.Customers.Find(order.CustomerId);

        result.Add(new OrderDto
        {
            Id = order.Id,
            CustomerName = customer.Name,
            Items = items.Select(i => new ItemDto { Name = i.Name, Price = i.Price }).ToList(),
            Total = items.Sum(i => i.Price)
        });
    }

    return result;
}

{{VOLUME}}: 100-500 orderIds por chamada
{{SLA}}: < 200ms
```

### Output Esperado

```markdown
## Resumo de Performance
**CRÍTICO** - Query N+1 severa causará timeout com volume esperado.
Estimativa: 100 orders = ~300 queries = ~3000ms (15x acima do SLA)

## Problemas Identificados

### 🔴 [CRÍTICO] Query N+1 em Loop
- **Tipo**: banco
- **Complexidade**: O(3n) queries → Deveria ser O(1)
- **Impacto**: ~30ms por order = 3000ms para 100 orders
- **Código Otimizado**:
```csharp
public List<OrderDto> GetOrdersWithDetails(List<int> orderIds)
{
    return _context.Orders
        .Where(o => orderIds.Contains(o.Id))
        .Include(o => o.Items)
        .Include(o => o.Customer)
        .Select(o => new OrderDto
        {
            Id = o.Id,
            CustomerName = o.Customer.Name,
            Items = o.Items.Select(i => new ItemDto
            {
                Name = i.Name,
                Price = i.Price
            }).ToList(),
            Total = o.Items.Sum(i => i.Price)
        })
        .ToList();
}
```
- **Justificativa**: Uma única query com JOINs em vez de 3N queries...
```

---

## Prompts Relacionados

- [quality-reviewer.md](quality-reviewer.md) - Revisar qualidade
- [security-reviewer.md](security-reviewer.md) - Revisar segurança
- [../../performance/optimization/performance-analyzer.md](../../performance/optimization/performance-analyzer.md) - Análise profunda
