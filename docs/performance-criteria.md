# Critérios de Performance

Este documento define os critérios de performance que todos os prompts de código desta biblioteca aplicam.

---

## Complexidade Algorítmica (Big O)

### Hierarquia de Complexidade

| Notação | Nome | Exemplo | 1000 itens | 1M itens |
|---------|------|---------|------------|----------|
| O(1) | Constante | Hash lookup | 1 op | 1 op |
| O(log n) | Logarítmica | Binary search | 10 ops | 20 ops |
| O(n) | Linear | Loop simples | 1K ops | 1M ops |
| O(n log n) | Linearítmica | Merge sort | 10K ops | 20M ops |
| O(n²) | Quadrática | Nested loops | 1M ops | 1T ops |
| O(2ⁿ) | Exponencial | Subset enumeration | - | - |

### Decisões de Design

```csharp
// RUIM: O(n) para busca frequente
var user = users.FirstOrDefault(u => u.Id == id);

// BOM: O(1) com Dictionary
var userDict = users.ToDictionary(u => u.Id);
var user = userDict[id];

// RUIM: O(n²) com Contains em loop
foreach (var item in items)
{
    if (excludeList.Contains(item.Id)) // O(n) cada vez
        continue;
}

// BOM: O(n) com HashSet
var excludeSet = new HashSet<int>(excludeList);
foreach (var item in items)
{
    if (excludeSet.Contains(item.Id)) // O(1) cada vez
        continue;
}
```

---

## Queries N+1

### O Problema

```csharp
// RUIM: N+1 queries
var orders = await _context.Orders.ToListAsync(); // 1 query
foreach (var order in orders)
{
    var customer = await _context.Customers
        .FindAsync(order.CustomerId); // N queries
}
```

### Soluções

**1. Eager Loading**:
```csharp
// BOM: 1 query com JOIN
var orders = await _context.Orders
    .Include(o => o.Customer)
    .ToListAsync();
```

**2. Explicit Loading (quando condicional)**:
```csharp
// BOM: 2 queries
var orders = await _context.Orders.ToListAsync();
var customerIds = orders.Select(o => o.CustomerId).Distinct();
var customers = await _context.Customers
    .Where(c => customerIds.Contains(c.Id))
    .ToDictionaryAsync(c => c.Id);
```

**3. Projection**:
```csharp
// BOM: 1 query, apenas dados necessários
var orderDtos = await _context.Orders
    .Select(o => new OrderDto
    {
        Id = o.Id,
        CustomerName = o.Customer.Name,
        Total = o.Total
    })
    .ToListAsync();
```

---

## Memory Management

### Prevenção de Memory Leaks

**1. Dispose de recursos**:
```csharp
// RUIM: Sem dispose
public void ProcessFile(string path)
{
    var stream = new FileStream(path, FileMode.Open);
    // Leak: stream nunca é fechado
}

// BOM: Using statement
public void ProcessFile(string path)
{
    using var stream = new FileStream(path, FileMode.Open);
    // Automaticamente disposed
}
```

**2. Event handlers**:
```csharp
// RUIM: Leak por event handler
public class MyComponent
{
    public MyComponent()
    {
        SomeService.DataChanged += OnDataChanged;
        // Leak: nunca remove o handler
    }
}

// BOM: Implementar IDisposable
public class MyComponent : IDisposable
{
    public MyComponent()
    {
        SomeService.DataChanged += OnDataChanged;
    }

    public void Dispose()
    {
        SomeService.DataChanged -= OnDataChanged;
    }
}
```

**3. Caching ilimitado**:
```csharp
// RUIM: Cache cresce infinitamente
private static Dictionary<string, Data> _cache = new();

// BOM: Cache com limite
private static MemoryCache _cache = new(new MemoryCacheOptions
{
    SizeLimit = 1000
});
```

---

## Async/Await

### Uso Correto

**1. Não bloqueie threads**:
```csharp
// RUIM: Bloqueia thread
var result = GetDataAsync().Result;
var result2 = GetDataAsync().GetAwaiter().GetResult();

// BOM: Await assíncrono
var result = await GetDataAsync();
```

**2. Use ConfigureAwait quando apropriado**:
```csharp
// Em bibliotecas (não precisa do contexto de sincronização)
var data = await _httpClient.GetStringAsync(url)
    .ConfigureAwait(false);
```

**3. Paralelização correta**:
```csharp
// RUIM: Sequencial
foreach (var url in urls)
{
    var data = await FetchAsync(url);
}

// BOM: Paralelo (quando independentes)
var tasks = urls.Select(url => FetchAsync(url));
var results = await Task.WhenAll(tasks);

// BOM: Paralelo com limite
var semaphore = new SemaphoreSlim(10);
var tasks = urls.Select(async url =>
{
    await semaphore.WaitAsync();
    try { return await FetchAsync(url); }
    finally { semaphore.Release(); }
});
```

**4. Async all the way**:
```csharp
// RUIM: Sync sobre async
public void Process()
{
    ProcessAsync().Wait();
}

// BOM: Async completo
public async Task ProcessAsync()
{
    await ProcessDataAsync();
}
```

---

## Caching Strategies

### Quando usar cache

| Cenário | Cache Recomendado |
|---------|-------------------|
| Dados lidos frequentemente, mudam raramente | Sim |
| Computações caras | Sim |
| Chamadas externas lentas | Sim |
| Dados que mudam frequentemente | Avaliar TTL curto |
| Dados específicos por usuário | Cache distribuído |

### Implementação

**1. Cache em memória (single instance)**:
```csharp
public class CachedService
{
    private readonly IMemoryCache _cache;

    public async Task<Data> GetDataAsync(string key)
    {
        return await _cache.GetOrCreateAsync(key, async entry =>
        {
            entry.AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10);
            entry.SlidingExpiration = TimeSpan.FromMinutes(2);

            return await _repository.GetDataAsync(key);
        });
    }
}
```

**2. Cache distribuído (múltiplas instâncias)**:
```csharp
public class CachedService
{
    private readonly IDistributedCache _cache;

    public async Task<Data> GetDataAsync(string key)
    {
        var cached = await _cache.GetStringAsync(key);
        if (cached != null)
        {
            return JsonSerializer.Deserialize<Data>(cached);
        }

        var data = await _repository.GetDataAsync(key);

        await _cache.SetStringAsync(key,
            JsonSerializer.Serialize(data),
            new DistributedCacheEntryOptions
            {
                AbsoluteExpirationRelativeToNow = TimeSpan.FromMinutes(10)
            });

        return data;
    }
}
```

**3. Cache-aside com invalidação**:
```csharp
public async Task UpdateDataAsync(string key, Data data)
{
    await _repository.UpdateAsync(data);
    await _cache.RemoveAsync(key); // Invalida cache
}
```

---

## Connection Pooling

### Database

```csharp
// Connection string com pooling
var connectionString = "Server=...;Pooling=true;Min Pool Size=5;Max Pool Size=100;";

// O pool é gerenciado automaticamente pelo ADO.NET
// Sempre use 'using' para retornar conexão ao pool
using var connection = new SqlConnection(connectionString);
await connection.OpenAsync();
```

### HTTP

```csharp
// RUIM: Nova instância a cada request
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    return await client.GetStringAsync(url);
}

// BOM: Reusar HttpClient via DI
public class MyService
{
    private readonly HttpClient _httpClient;

    public MyService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }

    public async Task<string> FetchDataAsync(string url)
    {
        return await _httpClient.GetStringAsync(url);
    }
}

// Registro no Startup
services.AddHttpClient<MyService>();
```

---

## Otimizações de Banco de Dados

### Índices

```sql
-- Criar índice para queries frequentes
CREATE INDEX IX_Orders_CustomerId ON Orders(CustomerId);

-- Índice composto para queries com múltiplos filtros
CREATE INDEX IX_Orders_Status_Date ON Orders(Status, OrderDate);

-- Índice covering para evitar lookup
CREATE INDEX IX_Orders_CustomerId_Include
ON Orders(CustomerId) INCLUDE (Status, Total);
```

### Queries Eficientes

```csharp
// RUIM: Traz todos os dados
var allOrders = await _context.Orders.ToListAsync();
var filtered = allOrders.Where(o => o.Status == "Pending");

// BOM: Filtra no banco
var filtered = await _context.Orders
    .Where(o => o.Status == "Pending")
    .ToListAsync();

// RUIM: Select *
var orders = await _context.Orders.ToListAsync();

// BOM: Apenas campos necessários
var orders = await _context.Orders
    .Select(o => new { o.Id, o.Total })
    .ToListAsync();

// RUIM: Sem paginação
var orders = await _context.Orders.ToListAsync();

// BOM: Com paginação
var orders = await _context.Orders
    .Skip(page * pageSize)
    .Take(pageSize)
    .ToListAsync();
```

---

## Profiling e Métricas

### Métricas Essenciais

| Métrica | O que mede | Threshold típico |
|---------|------------|------------------|
| Response Time (P50) | Latência mediana | < 100ms |
| Response Time (P99) | Latência worst case | < 500ms |
| Throughput | Requests/segundo | Depende do SLA |
| Error Rate | % de erros | < 0.1% |
| CPU Usage | Uso de CPU | < 70% |
| Memory Usage | Uso de memória | < 80% |

### Instrumentação

```csharp
// Medição de tempo
public async Task<Result> ProcessAsync()
{
    var stopwatch = Stopwatch.StartNew();

    try
    {
        var result = await DoWorkAsync();

        _metrics.RecordDuration("process_duration", stopwatch.ElapsedMilliseconds);

        return result;
    }
    catch (Exception ex)
    {
        _metrics.IncrementCounter("process_errors");
        throw;
    }
}
```

---

## Checklist de Performance

Antes de finalizar o código, verifique:

- [ ] Complexidade algorítmica é apropriada
- [ ] Não há queries N+1
- [ ] Recursos são disposed corretamente
- [ ] Async/await é usado corretamente
- [ ] Cache é usado onde apropriado
- [ ] Connection pooling está habilitado
- [ ] Queries de banco são otimizadas
- [ ] Índices necessários existem
- [ ] Paginação está implementada para listas grandes
- [ ] Apenas dados necessários são carregados
- [ ] Operações paralelas têm limite de concorrência
- [ ] Memory leaks potenciais foram verificados
