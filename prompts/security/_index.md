# Security - Prompts para Segurança

> Prompts para análise e melhoria de segurança de aplicações.

---

## Categorias

| Categoria | Descrição | Prompts |
|-----------|-----------|---------|
| [owasp/](owasp/) | Análise OWASP Top 10 | 1 |
| [code-security-review/](code-security-review/) | Auditoria de código | 1 |
| [vulnerability-assessment/](vulnerability-assessment/) | Avaliação de vulnerabilidades | 1 |
| [authentication/](authentication/) | Auth e AuthZ | - |
| [encryption/](encryption/) | Criptografia | - |

---

## Prompts Disponíveis

| Prompt | Caso de Uso | Link |
|--------|-------------|------|
| OWASP Checker | Verificar contra OWASP Top 10 | [owasp/owasp-checker.md](owasp/owasp-checker.md) |
| Security Audit | Auditoria completa de segurança | [code-security-review/security-audit.md](code-security-review/security-audit.md) |
| Vulnerability Finder | Encontrar vulnerabilidades específicas | [vulnerability-assessment/vulnerability-finder.md](vulnerability-assessment/vulnerability-finder.md) |

---

## Fluxo de Análise de Segurança

```
┌───────────────────────┐
│ Código a ser analisado │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ OWASP Checker          │ ─────> Verificação rápida Top 10
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Vulnerability Finder   │ ─────> Análise específica
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Security Audit         │ ─────> Auditoria completa
└───────────────────────┘
```

---

## Referência Rápida OWASP Top 10

| # | Vulnerabilidade | Exemplo |
|---|----------------|---------|
| A01 | Broken Access Control | IDOR, privilege escalation |
| A02 | Cryptographic Failures | Weak crypto, exposed data |
| A03 | Injection | SQL, NoSQL, Command injection |
| A04 | Insecure Design | Business logic flaws |
| A05 | Security Misconfiguration | Default configs, verbose errors |
| A06 | Vulnerable Components | Outdated dependencies |
| A07 | Auth Failures | Weak passwords, session issues |
| A08 | Data Integrity Failures | Insecure deserialization |
| A09 | Logging Failures | Missing audit logs |
| A10 | SSRF | Internal resource access |
