# Critérios de Qualidade de Código

Este documento define os critérios de qualidade que todos os prompts de código desta biblioteca aplicam.

---

## SOLID Principles

### S - Single Responsibility Principle (SRP)

**Definição**: Uma classe/função deve ter apenas um motivo para mudar.

**Aplicação**:
- Cada classe deve representar um único conceito
- Cada função deve fazer apenas uma coisa
- Se você precisa usar "e" para descrever o que a classe faz, ela faz demais

**Exemplo de Violação**:
```csharp
// RUIM: Faz múltiplas coisas
public class UserService
{
    public void CreateUser(User user) { }
    public void SendEmail(string email, string message) { }
    public void GenerateReport(int userId) { }
}
```

**Exemplo Correto**:
```csharp
// BOM: Responsabilidade única
public class UserService
{
    public void CreateUser(User user) { }
}

public class EmailService
{
    public void SendEmail(string email, string message) { }
}

public class ReportService
{
    public void GenerateReport(int userId) { }
}
```

---

### O - Open/Closed Principle (OCP)

**Definição**: Entidades devem estar abertas para extensão, fechadas para modificação.

**Aplicação**:
- Use abstrações (interfaces, classes abstratas)
- Adicione comportamento via herança ou composição
- Evite modificar código existente que funciona

**Exemplo de Violação**:
```csharp
// RUIM: Precisa modificar para adicionar novo tipo
public decimal CalculateDiscount(string customerType)
{
    if (customerType == "Regular") return 0.1m;
    if (customerType == "Premium") return 0.2m;
    // Para adicionar "VIP", precisa modificar este código
    return 0;
}
```

**Exemplo Correto**:
```csharp
// BOM: Extensível sem modificação
public interface IDiscountStrategy
{
    decimal Calculate();
}

public class RegularDiscount : IDiscountStrategy
{
    public decimal Calculate() => 0.1m;
}

public class PremiumDiscount : IDiscountStrategy
{
    public decimal Calculate() => 0.2m;
}
// Para adicionar VIP, apenas crie nova classe
```

---

### L - Liskov Substitution Principle (LSP)

**Definição**: Subtipos devem ser substituíveis por seus tipos base sem alterar o comportamento correto do programa.

**Aplicação**:
- Subclasses não devem remover comportamentos da classe base
- Subclasses não devem lançar exceções que a base não lança
- Pré-condições não podem ser fortalecidas
- Pós-condições não podem ser enfraquecidas

**Exemplo de Violação**:
```csharp
// RUIM: Viola expectativas do tipo base
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }
    // Comportamento inesperado quando usado como Rectangle
}
```

---

### I - Interface Segregation Principle (ISP)

**Definição**: Clientes não devem ser forçados a depender de interfaces que não usam.

**Aplicação**:
- Prefira múltiplas interfaces pequenas a uma grande
- Interfaces devem ser coesas
- Clientes devem implementar apenas o que precisam

**Exemplo de Violação**:
```csharp
// RUIM: Interface muito grande
public interface IWorker
{
    void Work();
    void Eat();
    void Sleep();
}

// Robô não come nem dorme!
public class Robot : IWorker
{
    public void Work() { }
    public void Eat() => throw new NotImplementedException();
    public void Sleep() => throw new NotImplementedException();
}
```

**Exemplo Correto**:
```csharp
// BOM: Interfaces segregadas
public interface IWorkable
{
    void Work();
}

public interface IFeedable
{
    void Eat();
}

public class Robot : IWorkable
{
    public void Work() { }
}
```

---

### D - Dependency Inversion Principle (DIP)

**Definição**: Módulos de alto nível não devem depender de módulos de baixo nível. Ambos devem depender de abstrações.

**Aplicação**:
- Injete dependências via construtor
- Dependa de interfaces, não de implementações concretas
- Use IoC containers quando apropriado

**Exemplo de Violação**:
```csharp
// RUIM: Dependência direta de implementação
public class OrderService
{
    private readonly SqlDatabase _database = new SqlDatabase();

    public void SaveOrder(Order order)
    {
        _database.Save(order);
    }
}
```

**Exemplo Correto**:
```csharp
// BOM: Dependência de abstração
public class OrderService
{
    private readonly IDatabase _database;

    public OrderService(IDatabase database)
    {
        _database = database;
    }

    public void SaveOrder(Order order)
    {
        _database.Save(order);
    }
}
```

---

## Clean Code

### Nomenclatura

| Regra | Ruim | Bom |
|-------|------|-----|
| Nomes descritivos | `int d;` | `int elapsedTimeInDays;` |
| Evite abreviações | `GetCustAddr()` | `GetCustomerAddress()` |
| Verbos para funções | `UserData()` | `GetUserData()` |
| Substantivos para classes | `ManageUser` | `UserManager` |
| Booleanos como perguntas | `active` | `isActive` |
| Constantes descritivas | `86400` | `SECONDS_PER_DAY` |

### Funções

**Regras**:
1. **Pequenas**: Máximo 20-30 linhas
2. **Fazem uma coisa**: Uma responsabilidade clara
3. **Um nível de abstração**: Não misture níveis
4. **Poucos parâmetros**: Ideal 0-2, máximo 3
5. **Sem efeitos colaterais**: Faça apenas o que o nome diz

**Exemplo**:
```csharp
// RUIM
public void ProcessOrder(Order order)
{
    // Valida (nível alto)
    if (order == null) throw new ArgumentNullException();

    // Calcula (nível médio)
    decimal total = 0;
    foreach (var item in order.Items)
    {
        total += item.Price * item.Quantity;
    }

    // Persiste (nível baixo)
    using var conn = new SqlConnection(connString);
    conn.Open();
    // ... SQL aqui
}

// BOM
public void ProcessOrder(Order order)
{
    ValidateOrder(order);
    var total = CalculateTotal(order);
    SaveOrder(order, total);
}
```

### DRY (Don't Repeat Yourself)

- Extraia código duplicado para funções/classes
- Use herança ou composição para comportamento compartilhado
- Centralize constantes e configurações

### Comentários

**Quando usar**:
- Explicar "por que", não "o que"
- Documentar APIs públicas
- Avisos importantes (TODO, FIXME, HACK)

**Quando NÃO usar**:
- Código auto-explicativo
- Histórico de mudanças (use Git)
- Código comentado (delete!)

---

## Tratamento de Erros

### Princípios

1. **Use exceções, não códigos de erro**
2. **Forneça contexto na exceção**
3. **Defina exceções por necessidade do chamador**
4. **Não retorne null** - use Optional/Maybe ou lance exceção

### Padrão

```csharp
public async Task<User> GetUserAsync(int id)
{
    try
    {
        var user = await _repository.GetByIdAsync(id);

        if (user is null)
        {
            throw new NotFoundException($"User with ID {id} not found");
        }

        return user;
    }
    catch (DatabaseException ex)
    {
        _logger.LogError(ex, "Failed to retrieve user {UserId}", id);
        throw new ServiceException("Failed to retrieve user", ex);
    }
}
```

---

## Logging Estruturado

### Níveis

| Nível | Quando Usar |
|-------|-------------|
| `Trace` | Informações muito detalhadas (debug) |
| `Debug` | Informações úteis para debugging |
| `Information` | Fluxo normal da aplicação |
| `Warning` | Situações anormais mas recuperáveis |
| `Error` | Erros que impedem a operação |
| `Critical` | Falhas que requerem atenção imediata |

### Padrão

```csharp
// BOM: Estruturado com contexto
_logger.LogInformation(
    "Order {OrderId} created for customer {CustomerId} with total {Total}",
    order.Id,
    order.CustomerId,
    order.Total
);

// RUIM: Concatenação sem estrutura
_logger.LogInformation($"Order {order.Id} created");
```

---

## Testabilidade

### Princípios

1. **Injete dependências** - facilita mocking
2. **Evite estado global** - dificulta isolamento
3. **Funções puras** - mesma entrada, mesma saída
4. **Interfaces para dependências externas** - facilita substituição

### Estrutura de Teste (AAA)

```csharp
[Fact]
public async Task GetUser_WhenUserExists_ReturnsUser()
{
    // Arrange
    var userId = 1;
    var expectedUser = new User { Id = userId, Name = "John" };
    _mockRepository.Setup(r => r.GetByIdAsync(userId))
        .ReturnsAsync(expectedUser);

    // Act
    var result = await _service.GetUserAsync(userId);

    // Assert
    Assert.Equal(expectedUser.Id, result.Id);
    Assert.Equal(expectedUser.Name, result.Name);
}
```

---

## Design Patterns Comuns

| Pattern | Quando Usar |
|---------|-------------|
| **Repository** | Isolar acesso a dados |
| **Factory** | Criação complexa de objetos |
| **Strategy** | Algoritmos intercambiáveis |
| **Observer** | Notificação de eventos |
| **Decorator** | Adicionar comportamento |
| **Adapter** | Integrar interfaces incompatíveis |
| **Command** | Encapsular operações |
| **Mediator** | Desacoplar comunicação |

---

## Checklist de Qualidade

Antes de finalizar o código, verifique:

- [ ] Cada classe/função tem responsabilidade única (SRP)
- [ ] Código é extensível sem modificação (OCP)
- [ ] Subtipos são substituíveis (LSP)
- [ ] Interfaces são coesas e pequenas (ISP)
- [ ] Dependências são injetadas via abstração (DIP)
- [ ] Nomenclatura é clara e expressiva
- [ ] Funções são pequenas e fazem uma coisa
- [ ] Não há duplicação de código
- [ ] Erros são tratados apropriadamente
- [ ] Logging é estruturado e adequado
- [ ] Código é testável
- [ ] Decisões estão justificadas
