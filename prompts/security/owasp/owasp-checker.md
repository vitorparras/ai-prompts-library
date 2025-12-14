# Verificador OWASP Top 10

> **Categoria**: security/owasp
> **Versão**: 1.0.0

---

## Quando Usar

- Verificação rápida de segurança antes de deploy
- Auditoria periódica de código
- Revisão de PRs com impacto em segurança
- Checklist de segurança para novas features

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código a ser verificado | [código fonte] | Sim |
| `{{TIPO}}` | Tipo de aplicação | "API REST", "Web App", "Backend" | Não |

---

## Prompt

```text
Atue como um **Security Analyst Senior** certificado em OWASP e Application Security.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente.
</contexto>

<raciocinio>
Verifique o código contra cada categoria do OWASP Top 10 (2021):

1. A01: BROKEN ACCESS CONTROL
   - Há verificação de autorização em cada operação?
   - IDOR é possível?
   - Elevação de privilégio é prevenida?

2. A02: CRYPTOGRAPHIC FAILURES
   - Dados sensíveis são criptografados?
   - Algoritmos modernos são usados?
   - Chaves são gerenciadas corretamente?

3. A03: INJECTION
   - Inputs são validados/sanitizados?
   - Queries são parametrizadas?
   - Comandos são escapados?

4. A04: INSECURE DESIGN
   - Fluxos de negócio são seguros?
   - Rate limiting existe?
   - Fail securely?

5. A05: SECURITY MISCONFIGURATION
   - Configs de segurança adequadas?
   - Headers de segurança?
   - Verbose errors desabilitados em prod?

6. A06: VULNERABLE COMPONENTS
   - Dependências atualizadas?
   - Vulnerabilidades conhecidas?

7. A07: AUTH FAILURES
   - Autenticação robusta?
   - Sessões seguras?
   - MFA disponível?

8. A08: DATA INTEGRITY FAILURES
   - Deserialização segura?
   - Integridade verificada?

9. A09: LOGGING FAILURES
   - Eventos de segurança logados?
   - Logs protegidos?

10. A10: SSRF
    - URLs externas validadas?
    - Acesso interno bloqueado?
</raciocinio>

<tarefa>
Verifique o seguinte código contra OWASP Top 10:

```
{{CODIGO}}
```

Tipo de aplicação: {{TIPO}}
</tarefa>

<formato-de-resposta>
# Relatório OWASP Top 10

## Resumo
| Total de Verificações | Passou | Falhou | N/A |
|----------------------|--------|--------|-----|
| 10 | X | Y | Z |

**Status Geral**: [SEGURO / REQUER ATENÇÃO / CRÍTICO]

---

## Verificação Detalhada

### A01: Broken Access Control
**Status**: ✅ Passou / ⚠️ Alerta / ❌ Falhou / ➖ N/A

**Verificações:**
- [ ] Autorização verificada em cada endpoint
- [ ] IDOR prevenido
- [ ] Elevação de privilégio bloqueada

**Achados:**
[Descrição do que foi encontrado]

**Correção (se necessário):**
```[linguagem]
[código corrigido]
```

---

### A02: Cryptographic Failures
**Status**: ✅/⚠️/❌/➖
...

[Repetir para A03-A10]

---

## Priorização de Correções

| Prioridade | Vulnerabilidade | Severidade | Esforço |
|------------|-----------------|------------|---------|
| 1 | [A0X] | Crítica | Baixo |
| 2 | [A0Y] | Alta | Médio |

---

## Recomendações Gerais
1. [Recomendação 1]
2. [Recomendação 2]

---

## Próximos Passos
1. [Ação imediata]
2. [Ação de médio prazo]
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../code-security-review/security-audit.md](../code-security-review/security-audit.md) - Auditoria completa
- [../vulnerability-assessment/vulnerability-finder.md](../vulnerability-assessment/vulnerability-finder.md) - Análise específica
