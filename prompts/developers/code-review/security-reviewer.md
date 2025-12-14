# Revisor de Segurança

> **Categoria**: developers/code-review
> **Versão**: 1.0.0

---

## Quando Usar

- Revisar código que lida com autenticação/autorização
- Revisar código que processa dados de usuário
- Revisar código que interage com banco de dados
- Antes de deploy de código crítico
- Auditoria de segurança periódica

## Quando NÃO Usar

- Análise de qualidade geral (use quality-reviewer)
- Análise de performance (use performance-reviewer)
- Código de teste ou protótipos

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Código financeiro | Máximo rigor, compliance |
| API pública | Foco em input validation, rate limiting |
| Código interno | Foco em autorização |
| Integração externa | Foco em comunicação segura |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código a ser revisado | [código fonte] | Sim |
| `{{TIPO_DADOS}}` | Tipos de dados sensíveis envolvidos | "PII, cartões de crédito" | Não |
| `{{CONTEXTO}}` | Contexto de exposição | "API pública, acesso anônimo" | Não |

---

## Prompt

```text
Atue como um **Security Engineer Senior** especializado em Application Security e OWASP.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Considere os padrões de segurança já implementados e identifique gaps.
</contexto>

<raciocinio>
Conduza a análise de segurança sistematicamente:

1. SUPERFÍCIE DE ATAQUE
   - Quais são os pontos de entrada de dados?
   - Quais dados externos são processados?
   - Quem pode acessar este código?
   - Qual é o nível de privilégio necessário?

2. ANÁLISE OWASP TOP 10
   Para cada categoria aplicável:
   - A01: Broken Access Control
   - A02: Cryptographic Failures
   - A03: Injection
   - A04: Insecure Design
   - A05: Security Misconfiguration
   - A06: Vulnerable Components
   - A07: Auth Failures
   - A08: Data Integrity Failures
   - A09: Logging Failures
   - A10: SSRF

3. ANÁLISE DE DADOS SENSÍVEIS
   - Quais dados sensíveis são manipulados?
   - Como são armazenados?
   - Como são transmitidos?
   - Como são logados?

4. VALIDAÇÃO E SANITIZAÇÃO
   - Inputs são validados?
   - Outputs são escapados?
   - Dados são sanitizados antes do uso?

5. AUTENTICAÇÃO E AUTORIZAÇÃO
   - Quem pode executar esta ação?
   - A autorização é verificada em cada ponto?
   - Tokens/sessões são gerenciados corretamente?
</raciocinio>

<tarefa>
Analise o seguinte código do ponto de vista de segurança:

```
{{CODIGO}}
```

Tipos de dados sensíveis envolvidos:
{{TIPO_DADOS}}

Contexto de exposição:
{{CONTEXTO}}
</tarefa>

<criterios-de-avaliacao>
Classifique cada vulnerabilidade:

🔴 CRÍTICO: Exploração remota fácil, alto impacto
🟠 ALTO: Exploração possível, impacto significativo
🟡 MÉDIO: Exploração requer condições, impacto moderado
🟢 BAIXO: Difícil exploração, baixo impacto

Para cada vulnerabilidade, forneça:
- CWE ID quando aplicável
- CVSS aproximado
- Vetor de ataque
- Impacto potencial
</criterios-de-avaliacao>

<formato-de-resposta>
## Resumo de Segurança
[Avaliação geral: CRÍTICO / ALTO / MÉDIO / BAIXO]
[Quantidade de vulnerabilidades por severidade]

## Superfície de Ataque
| Ponto de Entrada | Tipo de Dado | Nível de Risco |
|------------------|--------------|----------------|
| | | |

## Vulnerabilidades Encontradas

### 🔴 [CRÍTICO] Título da Vulnerabilidade
- **CWE**: CWE-XXX
- **OWASP**: A0X
- **Localização**: [arquivo:linha]
- **Descrição**: [O que é a vulnerabilidade]
- **Vetor de Ataque**: [Como um atacante exploraria]
- **Impacto**: [Consequências da exploração]
- **Código Vulnerável**:
```[linguagem]
[código problemático]
```
- **Correção**:
```[linguagem]
[código corrigido]
```
- **Justificativa**: [Por que essa correção resolve o problema]

### 🟠 [ALTO] Título
...

### 🟡 [MÉDIO] Título
...

## Análise OWASP

| Categoria | Status | Observação |
|-----------|--------|------------|
| A01: Access Control | ✅/⚠️/❌ | |
| A02: Crypto | ✅/⚠️/❌ | |
| A03: Injection | ✅/⚠️/❌ | |
| ... | | |

## Dados Sensíveis

| Dado | Classificação | Tratamento Atual | Recomendação |
|------|--------------|------------------|--------------|
| | | | |

## Boas Práticas Identificadas
[O que já está sendo feito corretamente]

## Recomendações Gerais
[Melhorias de segurança além das vulnerabilidades específicas]

## Checklist de Correção
- [ ] [Correção 1 - Crítica]
- [ ] [Correção 2 - Alta]
- [ ] [Correção 3 - Média]

## Testes de Segurança Sugeridos
[Testes que devem ser implementados]
</formato-de-resposta>

<justificativas>
Para cada vulnerabilidade:
1. Cite a categoria OWASP/CWE específica
2. Explique como um atacante poderia explorar
3. Explique o impacto real no negócio
4. Justifique por que a correção é adequada
</justificativas>
```

---

## Exemplo de Uso

### Input

```csharp
{{CODIGO}}:
[HttpPost("login")]
public IActionResult Login(LoginRequest request)
{
    var user = _db.Users.FirstOrDefault(u =>
        u.Email == request.Email &&
        u.Password == request.Password);

    if (user == null)
        return Unauthorized("Invalid email or password");

    var token = Convert.ToBase64String(
        Encoding.UTF8.GetBytes($"{user.Id}:{user.Email}"));

    _logger.LogInformation($"User {request.Email} logged in with password {request.Password}");

    return Ok(new { Token = token });
}

{{TIPO_DADOS}}: Credenciais de usuário
{{CONTEXTO}}: API pública, endpoint de autenticação
```

### Output Esperado

```markdown
## Resumo de Segurança
**CRÍTICO** - 4 vulnerabilidades críticas identificadas

## Vulnerabilidades Encontradas

### 🔴 [CRÍTICO] Senhas Armazenadas em Texto Plano
- **CWE**: CWE-256
- **OWASP**: A02 - Cryptographic Failures
- **Descrição**: Senhas comparadas diretamente sem hash
- **Correção**:
```csharp
var user = _db.Users.FirstOrDefault(u => u.Email == request.Email);
if (user == null || !BCrypt.Verify(request.Password, user.PasswordHash))
    return Unauthorized();
```
...
```

---

## Prompts Relacionados

- [quality-reviewer.md](quality-reviewer.md) - Revisar qualidade geral
- [performance-reviewer.md](performance-reviewer.md) - Revisar performance
- [../../security/owasp/owasp-checker.md](../../security/owasp/owasp-checker.md) - Análise OWASP completa
