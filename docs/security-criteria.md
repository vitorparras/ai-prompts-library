# Critérios de Segurança

Este documento define os critérios de segurança baseados no OWASP que todos os prompts de código desta biblioteca aplicam.

---

## OWASP Top 10 (2021)

### A01: Broken Access Control

**Risco**: Usuários agindo fora de suas permissões.

**Prevenção**:
- Negar acesso por padrão (deny by default)
- Implementar controle de acesso em cada endpoint
- Validar permissões no servidor, nunca apenas no cliente
- Usar tokens de sessão com escopo limitado

**Exemplo**:
```csharp
// RUIM: Sem verificação de propriedade
[HttpGet("orders/{id}")]
public async Task<Order> GetOrder(int id)
{
    return await _orderService.GetByIdAsync(id);
}

// BOM: Verifica se o usuário é dono do recurso
[HttpGet("orders/{id}")]
[Authorize]
public async Task<Order> GetOrder(int id)
{
    var userId = User.GetUserId();
    var order = await _orderService.GetByIdAsync(id);

    if (order.UserId != userId)
    {
        throw new ForbiddenException("Access denied");
    }

    return order;
}
```

---

### A02: Cryptographic Failures

**Risco**: Exposição de dados sensíveis por criptografia fraca ou ausente.

**Prevenção**:
- Use HTTPS sempre (TLS 1.2+)
- Criptografe dados sensíveis em repouso
- Use algoritmos modernos (AES-256, RSA-2048+)
- Nunca crie seus próprios algoritmos de criptografia
- Hash senhas com bcrypt, Argon2 ou PBKDF2

**Exemplo**:
```csharp
// RUIM: Hash fraco
var hashedPassword = MD5.Create().ComputeHash(passwordBytes);

// BOM: Hash seguro com salt
var hashedPassword = BCrypt.Net.BCrypt.HashPassword(password, workFactor: 12);

// Verificação
bool isValid = BCrypt.Net.BCrypt.Verify(inputPassword, hashedPassword);
```

---

### A03: Injection

**Risco**: Dados não confiáveis são enviados como parte de um comando ou query.

**Tipos**:
- SQL Injection
- NoSQL Injection
- Command Injection
- LDAP Injection

**Prevenção**:
- Use queries parametrizadas SEMPRE
- Valide e sanitize inputs
- Use ORMs corretamente
- Escape outputs

**Exemplo SQL Injection**:
```csharp
// RUIM: Concatenação direta
var query = $"SELECT * FROM Users WHERE Name = '{userName}'";

// BOM: Query parametrizada
var query = "SELECT * FROM Users WHERE Name = @Name";
command.Parameters.AddWithValue("@Name", userName);

// BOM: Com Entity Framework
var user = await _context.Users
    .Where(u => u.Name == userName)
    .FirstOrDefaultAsync();
```

---

### A04: Insecure Design

**Risco**: Falhas de design que não podem ser corrigidas com implementação perfeita.

**Prevenção**:
- Modelagem de ameaças desde o início
- Princípio do menor privilégio
- Defense in depth (múltiplas camadas)
- Fail securely (falhar de forma segura)

**Exemplo**:
```csharp
// Design seguro: Rate limiting + Account lockout
public class LoginService
{
    private const int MaxFailedAttempts = 5;
    private const int LockoutMinutes = 15;

    public async Task<LoginResult> LoginAsync(LoginRequest request)
    {
        var user = await _userRepository.GetByEmailAsync(request.Email);

        if (user?.IsLockedOut == true)
        {
            return LoginResult.AccountLocked;
        }

        if (!await ValidatePasswordAsync(user, request.Password))
        {
            await IncrementFailedAttemptsAsync(user);
            return LoginResult.InvalidCredentials;
        }

        await ResetFailedAttemptsAsync(user);
        return LoginResult.Success(GenerateToken(user));
    }
}
```

---

### A05: Security Misconfiguration

**Risco**: Configurações de segurança incorretas ou ausentes.

**Prevenção**:
- Desabilite features não utilizadas
- Configure headers de segurança
- Atualize dependências regularmente
- Não exponha stack traces em produção

**Exemplo**:
```csharp
// Configuração de headers de segurança
app.Use(async (context, next) =>
{
    context.Response.Headers.Add("X-Content-Type-Options", "nosniff");
    context.Response.Headers.Add("X-Frame-Options", "DENY");
    context.Response.Headers.Add("X-XSS-Protection", "1; mode=block");
    context.Response.Headers.Add("Strict-Transport-Security", "max-age=31536000");
    context.Response.Headers.Add("Content-Security-Policy", "default-src 'self'");

    await next();
});

// Não exponha detalhes em produção
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
else
{
    app.UseExceptionHandler("/error");
}
```

---

### A06: Vulnerable and Outdated Components

**Risco**: Uso de componentes com vulnerabilidades conhecidas.

**Prevenção**:
- Monitore CVEs das dependências
- Atualize regularmente
- Remova dependências não utilizadas
- Use apenas fontes confiáveis

**Ferramentas**:
- `dotnet list package --vulnerable`
- `npm audit`
- Dependabot / Snyk / OWASP Dependency-Check

---

### A07: Identification and Authentication Failures

**Risco**: Falhas que permitem comprometer identidades.

**Prevenção**:
- Implemente MFA quando possível
- Não exponha session IDs na URL
- Regenere session ID após login
- Use políticas de senha fortes
- Implemente rate limiting no login

**Exemplo**:
```csharp
public class PasswordPolicy
{
    public const int MinLength = 12;
    public const bool RequireUppercase = true;
    public const bool RequireLowercase = true;
    public const bool RequireDigit = true;
    public const bool RequireSpecialChar = true;

    public static bool Validate(string password)
    {
        if (password.Length < MinLength) return false;
        if (RequireUppercase && !password.Any(char.IsUpper)) return false;
        if (RequireLowercase && !password.Any(char.IsLower)) return false;
        if (RequireDigit && !password.Any(char.IsDigit)) return false;
        if (RequireSpecialChar && password.All(char.IsLetterOrDigit)) return false;

        return true;
    }
}
```

---

### A08: Software and Data Integrity Failures

**Risco**: Código e dados modificados por atacantes.

**Prevenção**:
- Verifique integridade de downloads
- Use assinaturas digitais
- Proteja pipelines CI/CD
- Valide dados deserializados

**Exemplo**:
```csharp
// RUIM: Deserialização insegura
var obj = JsonConvert.DeserializeObject<dynamic>(json);

// BOM: Deserialização tipada
var settings = new JsonSerializerSettings
{
    TypeNameHandling = TypeNameHandling.None
};
var user = JsonConvert.DeserializeObject<User>(json, settings);
```

---

### A09: Security Logging and Monitoring Failures

**Risco**: Ataques não detectados por falta de logging.

**Prevenção**:
- Log de eventos de segurança
- Alertas para anomalias
- Retenção adequada de logs
- Proteja logs contra tampering

**O que logar**:
```csharp
// Eventos de segurança que devem ser logados
_logger.LogWarning("Failed login attempt for user {Email} from IP {IP}",
    email, ipAddress);

_logger.LogInformation("User {UserId} granted role {Role}",
    userId, roleName);

_logger.LogWarning("Access denied for user {UserId} to resource {Resource}",
    userId, resourceId);

_logger.LogCritical("Possible SQL injection attempt detected: {Input}",
    SanitizeForLog(input));
```

---

### A10: Server-Side Request Forgery (SSRF)

**Risco**: Aplicação faz requisições para URLs controladas pelo atacante.

**Prevenção**:
- Valide e sanitize URLs fornecidas pelo usuário
- Use allowlists de domínios permitidos
- Bloqueie acesso a IPs internos
- Desabilite redirecionamentos automáticos

**Exemplo**:
```csharp
// BOM: Validação de URL
private readonly HashSet<string> _allowedHosts = new()
{
    "api.trusted-service.com",
    "cdn.our-company.com"
};

public async Task<string> FetchExternalData(string url)
{
    var uri = new Uri(url);

    if (!_allowedHosts.Contains(uri.Host))
    {
        throw new SecurityException($"Host not allowed: {uri.Host}");
    }

    // Verificar se não é IP interno
    var ip = await Dns.GetHostAddressesAsync(uri.Host);
    if (IsPrivateIp(ip.First()))
    {
        throw new SecurityException("Access to internal resources denied");
    }

    return await _httpClient.GetStringAsync(uri);
}
```

---

## Cross-Site Scripting (XSS)

**Tipos**:
- Reflected XSS
- Stored XSS
- DOM-based XSS

**Prevenção**:
- Escape output por contexto (HTML, JS, CSS, URL)
- Use Content Security Policy
- Sanitize HTML se necessário
- HttpOnly cookies

**Exemplo**:
```csharp
// RUIM: Output direto
<p>@Html.Raw(userInput)</p>

// BOM: Encoding automático
<p>@userInput</p>

// BOM: Sanitização quando HTML é necessário
<p>@Html.Raw(Sanitizer.Sanitize(userInput))</p>
```

---

## Cross-Site Request Forgery (CSRF)

**Prevenção**:
- Use tokens anti-CSRF
- Verifique header Origin/Referer
- Use SameSite cookies

**Exemplo**:
```csharp
// No Startup.cs
services.AddAntiforgery(options =>
{
    options.HeaderName = "X-CSRF-TOKEN";
});

// No Controller
[ValidateAntiForgeryToken]
[HttpPost]
public async Task<IActionResult> UpdateProfile(ProfileDto dto)
{
    // ...
}

// No formulário
<form method="post">
    @Html.AntiForgeryToken()
    ...
</form>
```

---

## Secrets Management

**Regras**:
- NUNCA hardcode secrets no código
- Use variáveis de ambiente ou vault
- Rotacione secrets regularmente
- Diferentes secrets por ambiente

**Exemplo**:
```csharp
// RUIM: Hardcoded
var connectionString = "Server=prod;Password=MyP@ssw0rd!";

// BOM: Environment variable
var connectionString = Environment.GetEnvironmentVariable("DATABASE_CONNECTION");

// BOM: User Secrets (desenvolvimento)
var connectionString = Configuration.GetConnectionString("Database");

// BOM: Azure Key Vault (produção)
var connectionString = await _keyVaultClient.GetSecretAsync("DatabaseConnection");
```

---

## Checklist de Segurança

Antes de finalizar o código, verifique:

- [ ] Inputs são validados e sanitizados
- [ ] Queries são parametrizadas (sem SQL Injection)
- [ ] Outputs são escapados (sem XSS)
- [ ] Proteção CSRF está implementada
- [ ] Autenticação e autorização estão corretas
- [ ] Secrets não estão no código
- [ ] Criptografia usa algoritmos modernos
- [ ] Headers de segurança estão configurados
- [ ] Logging de segurança está implementado
- [ ] Dependências estão atualizadas
- [ ] Rate limiting está implementado onde necessário
