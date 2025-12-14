# Gerador de Testes Unitários

> **Categoria**: developers/testing
> **Versão**: 1.0.0

---

## Quando Usar

- Criar testes para código novo ou existente
- Aumentar cobertura de testes
- Implementar TDD (Test-Driven Development)
- Criar testes de regressão após fix de bug

## Quando NÃO Usar

- Testes de integração (use test-scenarios)
- Testes E2E
- Testes de performance/carga

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| TDD | Gere testes ANTES do código |
| Código legado | Foque em comportamento observável |
| Código crítico | Inclua mais edge cases |
| Alta cobertura | Foque em branch coverage |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código a ser testado | [classe/função] | Sim |
| `{{FRAMEWORK}}` | Framework de testes | "xUnit", "Jest", "pytest" | Não |
| `{{FOCO}}` | Área específica para focar | "edge cases", "happy path" | Não |

---

## Prompt

```text
Atue como um **QA Engineer Senior** especialista em criar testes unitários abrangentes e eficazes.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Use os padrões de teste já existentes no projeto.
</contexto>

<raciocinio>
Antes de escrever os testes, analise:

1. ANÁLISE DO CÓDIGO
   - Qual o propósito do código?
   - Quais são os inputs e outputs?
   - Quais são as dependências?
   - Quais são os caminhos de execução (branches)?

2. IDENTIFICAÇÃO DE CENÁRIOS
   Categorize os cenários:

   a) Happy Path
      - Fluxo normal esperado
      - Inputs válidos típicos

   b) Edge Cases
      - Valores limite (0, 1, max, min)
      - Strings vazias, null, whitespace
      - Coleções vazias, um elemento, muitos elementos

   c) Error Cases
      - Inputs inválidos
      - Exceções esperadas
      - Estados inválidos

   d) Boundary Cases
      - Limites exatos
      - Off-by-one (n-1, n, n+1)

3. DECISÕES DE DESIGN
   - O que mockar vs. usar real?
   - Como isolar o código sendo testado?
   - Como tornar os testes determinísticos?

4. QUALIDADE DOS TESTES
   - Cada teste testa UMA coisa?
   - Os testes são independentes?
   - Os nomes são descritivos?
   - Os testes são fáceis de manter?
</raciocinio>

<tarefa>
Crie testes unitários abrangentes para o seguinte código:

```
{{CODIGO}}
```

Framework de testes: {{FRAMEWORK}}

Foco específico: {{FOCO}}
</tarefa>

<criterios-de-qualidade>
Os testes DEVEM seguir:

ESTRUTURA
- Padrão AAA (Arrange-Act-Assert)
- Um assert principal por teste (quando possível)
- Nomenclatura: [Método]_[Cenário]_[ResultadoEsperado]

COBERTURA
- Todos os happy paths
- Edge cases relevantes
- Casos de erro/exceção
- Branches condicionais

QUALIDADE
- Testes independentes (sem ordem de execução)
- Sem lógica condicional em testes
- Dados de teste claros e significativos
- Mocks apenas para dependências externas

MANUTENIBILIDADE
- Setup compartilhado em fixture/beforeEach
- Helpers para criação de dados de teste
- Evitar duplicação entre testes
</criterios-de-qualidade>

<formato-de-resposta>
Estruture sua resposta assim:

## Análise do Código
[Resumo do que o código faz e pontos críticos para testar]

## Cenários Identificados

### Happy Path
| Cenário | Input | Output Esperado |
|---------|-------|-----------------|
| | | |

### Edge Cases
| Cenário | Input | Output Esperado |
|---------|-------|-----------------|
| | | |

### Error Cases
| Cenário | Input | Exceção/Erro Esperado |
|---------|-------|----------------------|
| | | |

## Decisões de Mocking
| Dependência | Mock/Real | Justificativa |
|-------------|-----------|---------------|
| | | |

## Implementação dos Testes

```[linguagem]
// Imports e setup

public class [NomeDaClasse]Tests
{
    // Setup/Fixtures

    // Happy Path Tests
    [Fact/Test]
    public void [Método]_[Cenário]_[Resultado]()
    {
        // Arrange
        // Act
        // Assert
    }

    // Edge Case Tests
    ...

    // Error Case Tests
    ...
}
```

## Cobertura Alcançada
| Métrica | Valor |
|---------|-------|
| Line Coverage | ~X% |
| Branch Coverage | ~X% |
| Cenários cobertos | X/Y |

## Cenários NÃO Cobertos (e por quê)
[Se algum cenário foi intencionalmente deixado de fora, explique]
</formato-de-resposta>

<justificativas>
Para cada grupo de testes, explique:
1. Por que esses cenários são importantes
2. Que bug/problema cada teste previne
3. Decisões de mock e por que

Exemplo: "Testando com quantidade = 0 porque esse edge case frequentemente
causa divisão por zero em cálculos de média."
</justificativas>
```

---

## Exemplo de Uso

### Input

```csharp
{{CODIGO}}:
public class PriceCalculator
{
    private readonly IDiscountService _discountService;

    public PriceCalculator(IDiscountService discountService)
    {
        _discountService = discountService;
    }

    public decimal CalculateTotal(List<OrderItem> items, string couponCode)
    {
        if (items == null || !items.Any())
            throw new ArgumentException("Items cannot be empty");

        decimal subtotal = items.Sum(i => i.Price * i.Quantity);

        decimal discount = 0;
        if (!string.IsNullOrEmpty(couponCode))
        {
            discount = _discountService.GetDiscount(couponCode);
        }

        return subtotal - (subtotal * discount);
    }
}

{{FRAMEWORK}}: xUnit com Moq
```

### Output Esperado

```csharp
public class PriceCalculatorTests
{
    private readonly Mock<IDiscountService> _discountServiceMock;
    private readonly PriceCalculator _calculator;

    public PriceCalculatorTests()
    {
        _discountServiceMock = new Mock<IDiscountService>();
        _calculator = new PriceCalculator(_discountServiceMock.Object);
    }

    // Happy Path
    [Fact]
    public void CalculateTotal_WithValidItemsNoCoupon_ReturnsSubtotal()
    {
        // Arrange
        var items = new List<OrderItem>
        {
            new OrderItem { Price = 100m, Quantity = 2 }
        };

        // Act
        var result = _calculator.CalculateTotal(items, null);

        // Assert
        Assert.Equal(200m, result);
    }

    [Fact]
    public void CalculateTotal_WithValidCoupon_AppliesDiscount()
    {
        // Arrange
        var items = new List<OrderItem>
        {
            new OrderItem { Price = 100m, Quantity = 1 }
        };
        _discountServiceMock
            .Setup(s => s.GetDiscount("SAVE10"))
            .Returns(0.1m);

        // Act
        var result = _calculator.CalculateTotal(items, "SAVE10");

        // Assert
        Assert.Equal(90m, result);
    }

    // Edge Cases
    [Fact]
    public void CalculateTotal_WithNullItems_ThrowsArgumentException()
    {
        // Act & Assert
        Assert.Throws<ArgumentException>(
            () => _calculator.CalculateTotal(null, null)
        );
    }

    // ... mais testes
}
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Testes muito simples | Código simples | Peça foco em edge cases |
| Muitos mocks | Código muito acoplado | Pode indicar problema no código |
| Testes frágeis | Testando implementação | Foque em comportamento |

---

## Prompts Relacionados

- [test-scenarios.md](test-scenarios.md) - Identificar todos os cenários
- [../code-review/quality-reviewer.md](../code-review/quality-reviewer.md) - Revisar os testes
