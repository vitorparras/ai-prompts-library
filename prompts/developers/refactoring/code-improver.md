# Melhorador de Código

> **Categoria**: developers/refactoring
> **Versão**: 1.0.0

---

## Quando Usar

- Melhorar código que funciona mas tem problemas de qualidade
- Refatorar código legado
- Reduzir complexidade ciclomática
- Melhorar legibilidade e manutenibilidade

## Quando NÃO Usar

- Código com bugs (corrija primeiro com error-analyzer)
- Adicionar features (use feature-implementation)
- Otimizar performance (use prompts de performance)

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Código crítico | Mudanças mínimas, máxima segurança |
| Refactoring grande | Divida em etapas incrementais |
| Sem testes | Crie testes antes de refatorar |
| Legado complexo | Foque nas partes mais problemáticas |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código a ser melhorado | [código fonte] | Sim |
| `{{PROBLEMAS}}` | Problemas específicos a resolver | "Muito complexo, difícil de testar" | Não |
| `{{RESTRICOES}}` | Limitações da refatoração | "Não mudar interface pública" | Não |

---

## Prompt

```text
Atue como um **Engenheiro de Software Senior** especialista em refatoração e Clean Code.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Mantenha consistência com os padrões existentes enquanto melhora o código.
</contexto>

<raciocinio>
Antes de refatorar, analise sistematicamente:

1. DIAGNÓSTICO
   - Quais são os code smells presentes?
   - Quais princípios SOLID estão sendo violados?
   - Qual a complexidade ciclomática atual?
   - O que torna o código difícil de manter/testar?

2. PRIORIZAÇÃO
   Classifique os problemas por impacto:
   - Crítico: Afeta funcionamento ou segurança
   - Alto: Dificulta manutenção significativamente
   - Médio: Melhoria de legibilidade
   - Baixo: Preferências de estilo

3. ESTRATÉGIA DE REFATORAÇÃO
   Para cada problema, identifique:
   - Técnica de refatoração apropriada (Extract Method, etc.)
   - Risco da mudança
   - Testes necessários para garantir comportamento

4. PLANO DE EXECUÇÃO
   - Ordem das refatorações (menor risco primeiro)
   - Checkpoints de validação
   - Rollback se necessário
</raciocinio>

<tarefa>
Melhore o seguinte código mantendo seu comportamento:

```
{{CODIGO}}
```

Problemas específicos a resolver:
{{PROBLEMAS}}

Restrições:
{{RESTRICOES}}
</tarefa>

<tecnicas-de-refatoracao>
Use técnicas apropriadas:

EXTRAÇÃO
- Extract Method: Funções longas → funções pequenas
- Extract Class: Classe com muitas responsabilidades
- Extract Interface: Para inverter dependências

SIMPLIFICAÇÃO
- Replace Conditional with Polymorphism
- Replace Nested Conditional with Guard Clauses
- Decompose Conditional

ORGANIZAÇÃO
- Move Method: Mover para classe mais apropriada
- Rename: Nomes mais expressivos
- Introduce Parameter Object: Muitos parâmetros

LIMPEZA
- Remove Dead Code
- Remove Duplication
- Replace Magic Numbers with Constants
</tecnicas-de-refatoracao>

<criterios-de-qualidade>
O código refatorado DEVE:

MANTER
- Mesmo comportamento observável
- Mesma interface pública (se não permitido mudar)
- Compatibilidade com código existente

MELHORAR
- Legibilidade (código auto-explicativo)
- Testabilidade (dependências injetáveis)
- Manutenibilidade (fácil de modificar)
- Aderência a SOLID
- Aderência a Clean Code
</criterios-de-qualidade>

<formato-de-resposta>
Estruture sua resposta assim:

## Diagnóstico

### Code Smells Identificados
| Smell | Localização | Impacto | Técnica de Refatoração |
|-------|-------------|---------|----------------------|
| | | | |

### Violações de Princípios
| Princípio | Violação | Consequência |
|-----------|----------|--------------|
| | | |

### Métricas Atuais vs. Esperadas
| Métrica | Antes | Depois |
|---------|-------|--------|
| Linhas por função (máx) | | |
| Complexidade ciclomática | | |
| Dependências | | |

## Plano de Refatoração

### Etapa 1: [Nome]
- Técnica: [técnica de refatoração]
- Risco: [baixo/médio/alto]
- Validação: [como verificar que não quebrou]

### Etapa 2: [Nome]
...

## Código Refatorado

### Antes vs. Depois (Resumo Visual)
[Diagrama ou descrição das mudanças estruturais]

### Código Completo

```[linguagem]
[código refatorado completo]
```

### Explicação das Mudanças

#### [Mudança 1]
- **O que era**: [código original]
- **O que é agora**: [código novo]
- **Técnica usada**: [nome da técnica]
- **Justificativa**: [por que, citando princípio]

#### [Mudança 2]
...

## Testes Necessários
[Testes que devem passar para garantir que a refatoração não quebrou nada]

## Riscos e Mitigações
| Risco | Probabilidade | Mitigação |
|-------|--------------|-----------|
| | | |

## Melhorias Futuras (fora do escopo atual)
[O que mais poderia ser melhorado em iterações futuras]
</formato-de-resposta>

<justificativas>
Para CADA mudança significativa:
1. Cite o code smell eliminado ou princípio aplicado
2. Explique o BENEFÍCIO concreto (testabilidade, legibilidade, etc.)
3. Explique por que a nova forma é melhor

Exemplo: "Extraindo validateOrder() seguindo SRP - a função processOrder agora
faz apenas uma coisa (processar), delegando validação. Isso facilita testar
validação isoladamente e reutilizá-la em outros contextos."
</justificativas>
```

---

## Exemplo de Uso

### Input

```csharp
{{CODIGO}}:
public void ProcessOrder(Order order)
{
    // Validation
    if (order == null) throw new Exception("Order is null");
    if (order.Items == null || order.Items.Count == 0) throw new Exception("No items");
    if (order.CustomerId <= 0) throw new Exception("Invalid customer");

    // Get customer
    var conn = new SqlConnection("...");
    conn.Open();
    var cmd = new SqlCommand("SELECT * FROM Customers WHERE Id = " + order.CustomerId, conn);
    var reader = cmd.ExecuteReader();
    Customer customer = null;
    if (reader.Read())
    {
        customer = new Customer();
        customer.Id = (int)reader["Id"];
        customer.Name = (string)reader["Name"];
        customer.Email = (string)reader["Email"];
    }
    conn.Close();

    // Calculate
    decimal total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }

    // Apply discount
    if (customer.Type == "VIP") total *= 0.9m;
    else if (customer.Type == "Regular") total *= 0.95m;

    // Save
    conn.Open();
    var saveCmd = new SqlCommand($"INSERT INTO Orders VALUES ({order.Id}, {total})", conn);
    saveCmd.ExecuteNonQuery();
    conn.Close();

    // Notify
    var smtp = new SmtpClient();
    smtp.Send("system@company.com", customer.Email, "Order", "Your order total: " + total);
}

{{PROBLEMAS}}: Código muito longo, difícil de testar, SQL direto
{{RESTRICOES}}: Manter método público ProcessOrder
```

### Output Esperado

```markdown
## Diagnóstico

### Code Smells Identificados
| Smell | Localização | Impacto | Técnica |
|-------|-------------|---------|---------|
| Long Method | ProcessOrder (50+ linhas) | Alto | Extract Method |
| Feature Envy | Acesso direto ao banco | Alto | Extract Class |
| Magic Strings | "VIP", "Regular" | Médio | Replace with Enum/Strategy |

## Código Refatorado

```csharp
public class OrderProcessor
{
    private readonly ICustomerRepository _customerRepository;
    private readonly IOrderRepository _orderRepository;
    private readonly IDiscountStrategy _discountStrategy;
    private readonly INotificationService _notificationService;

    public OrderProcessor(
        ICustomerRepository customerRepository,
        IOrderRepository orderRepository,
        IDiscountStrategy discountStrategy,
        INotificationService notificationService)
    {
        _customerRepository = customerRepository;
        _orderRepository = orderRepository;
        _discountStrategy = discountStrategy;
        _notificationService = notificationService;
    }

    public void ProcessOrder(Order order)
    {
        ValidateOrder(order);

        var customer = _customerRepository.GetById(order.CustomerId)
            ?? throw new NotFoundException($"Customer {order.CustomerId} not found");

        var total = CalculateTotal(order.Items);
        var discountedTotal = _discountStrategy.Apply(total, customer);

        _orderRepository.Save(order, discountedTotal);
        _notificationService.NotifyOrderCreated(customer, discountedTotal);
    }
    // ...
}
```
**Justificativa**: Aplicando SRP - cada dependência cuida de uma responsabilidade...
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Mudou comportamento | Refatoração arriscada | Peça mudanças menores |
| Muito conservador | Restrições muito rígidas | Relaxe restrições |
| Muito radical | Escopo muito amplo | Foque em um problema |

---

## Prompts Relacionados

- [solid-applier.md](solid-applier.md) - Foco específico em SOLID
- [../testing/unit-test-generator.md](../testing/unit-test-generator.md) - Criar testes antes de refatorar
- [../code-review/quality-reviewer.md](../code-review/quality-reviewer.md) - Revisar refatoração
