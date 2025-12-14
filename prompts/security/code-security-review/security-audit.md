# Auditoria de Segurança de Código

> **Categoria**: security/code-security-review
> **Versão**: 1.0.0

---

## Quando Usar

- Auditoria completa antes de lançamento
- Revisão de módulos críticos
- Avaliação de código de terceiros
- Compliance e certificações

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | Código para auditoria | [código fonte] | Sim |
| `{{ESCOPO}}` | Escopo da auditoria | "Autenticação", "Pagamentos" | Não |
| `{{COMPLIANCE}}` | Requisitos de compliance | "PCI-DSS", "LGPD", "SOC2" | Não |

---

## Prompt

```text
Atue como um **Security Auditor Senior** realizando uma auditoria completa de segurança.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente.
</contexto>

<tarefa>
Realize uma auditoria de segurança completa:

```
{{CODIGO}}
```

**Escopo:** {{ESCOPO}}
**Compliance:** {{COMPLIANCE}}
</tarefa>

<areas-de-analise>
1. Input Validation
2. Output Encoding
3. Authentication
4. Authorization
5. Session Management
6. Cryptography
7. Error Handling
8. Logging & Monitoring
9. Data Protection
10. API Security
11. File Operations
12. Third-party Integrations
</areas-de-analise>

<formato-de-resposta>
# Relatório de Auditoria de Segurança

## Informações Gerais
- **Data**: [data]
- **Escopo**: {{ESCOPO}}
- **Compliance**: {{COMPLIANCE}}

## Sumário Executivo
[Resumo em 3-5 linhas]

**Classificação de Risco**: [Crítico/Alto/Médio/Baixo]

## Estatísticas

| Severidade | Quantidade |
|------------|------------|
| Crítica | X |
| Alta | Y |
| Média | Z |
| Baixa | W |
| Informativa | V |

---

## Vulnerabilidades Encontradas

### [SEV-001] [Título] - Severidade: CRÍTICA

**CWE**: CWE-XXX
**CVSS**: X.X
**Localização**: [arquivo:linha]

**Descrição**:
[Descrição técnica da vulnerabilidade]

**Prova de Conceito**:
```
[Como explorar - de forma ética]
```

**Impacto**:
- Confidencialidade: [Alto/Médio/Baixo]
- Integridade: [Alto/Médio/Baixo]
- Disponibilidade: [Alto/Médio/Baixo]

**Remediação**:
```[linguagem]
[código corrigido]
```

**Referências**:
- [Link para documentação]

---

## Conformidade com {{COMPLIANCE}}

| Requisito | Status | Observação |
|-----------|--------|------------|
| [Req 1] | ✅/❌ | [Nota] |

---

## Recomendações

### Imediatas (0-7 dias)
1. [Ação crítica 1]

### Curto Prazo (7-30 dias)
1. [Ação importante 1]

### Médio Prazo (30-90 dias)
1. [Melhoria 1]

---

## Conclusão
[Parecer final da auditoria]
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../owasp/owasp-checker.md](../owasp/owasp-checker.md) - Verificação OWASP
- [../vulnerability-assessment/vulnerability-finder.md](../vulnerability-assessment/vulnerability-finder.md) - Análise específica
