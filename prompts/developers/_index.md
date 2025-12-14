# Developers - Prompts para Desenvolvimento

> Prompts para desenvolvimento de software com qualidade de nível senior.

---

## Categorias

| Categoria | Descrição | Prompts |
|-----------|-----------|---------|
| [code-generation/](code-generation/) | Criar código novo | 2 |
| [debugging/](debugging/) | Analisar e resolver erros | 1 |
| [refactoring/](refactoring/) | Melhorar código existente | 2 |
| [code-review/](code-review/) | Revisar código | 3 |
| [testing/](testing/) | Criar testes | 2 |
| [documentation/](documentation/) | Documentar código | - |

---

## Prompts Mais Usados

| Prompt | Caso de Uso | Link |
|--------|-------------|------|
| Quality Reviewer | Revisar PR/código com foco em qualidade | [code-review/quality-reviewer.md](code-review/quality-reviewer.md) |
| Unit Test Generator | Gerar testes unitários | [testing/unit-test-generator.md](testing/unit-test-generator.md) |
| Error Analyzer | Diagnosticar e resolver erros | [debugging/error-analyzer.md](debugging/error-analyzer.md) |
| Feature Implementation | Implementar nova feature | [code-generation/feature-implementation.md](code-generation/feature-implementation.md) |

---

## Fluxo Típico de Desenvolvimento

```
┌──────────────────────┐
│ Feature Implementation │ ─────> Criar código novo
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Unit Test Generator   │ ─────> Criar testes
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Quality Reviewer      │ ─────> Revisar qualidade
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Security Reviewer     │ ─────> Revisar segurança
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Performance Reviewer  │ ─────> Revisar performance
└──────────────────────┘
```

---

## Pré-requisito

Sempre execute o [context-extractor](../foundation/context-extractor.md) antes de usar estes prompts para garantir que a IA entenda o contexto do seu projeto.

---

## Lista Completa de Prompts

### Code Generation
- [feature-implementation.md](code-generation/feature-implementation.md) - Implementar features completas
- [function-creator.md](code-generation/function-creator.md) - Criar funções/métodos específicos

### Debugging
- [error-analyzer.md](debugging/error-analyzer.md) - Analisar e resolver erros

### Refactoring
- [code-improver.md](refactoring/code-improver.md) - Melhorar código existente
- [solid-applier.md](refactoring/solid-applier.md) - Aplicar princípios SOLID

### Code Review
- [quality-reviewer.md](code-review/quality-reviewer.md) - Review com foco em qualidade
- [security-reviewer.md](code-review/security-reviewer.md) - Review com foco em segurança
- [performance-reviewer.md](code-review/performance-reviewer.md) - Review com foco em performance

### Testing
- [unit-test-generator.md](testing/unit-test-generator.md) - Gerar testes unitários
- [test-scenarios.md](testing/test-scenarios.md) - Identificar cenários de teste
