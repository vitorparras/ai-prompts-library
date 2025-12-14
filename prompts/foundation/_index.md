# Foundation - Prompts Fundamentais

> Use estes prompts **PRIMEIRO** antes de qualquer outro prompt da biblioteca.

---

## Visão Geral

Os prompts de Foundation são a base de toda a biblioteca. Eles estabelecem o contexto do projeto e garantem que a IA entenda profundamente seu código antes de ajudar.

## Fluxo Recomendado

```
┌─────────────────────┐
│ 1. Context Extractor │ ─────> Extrai contexto do projeto
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ 2. Development Rules │ ─────> Define regras de qualidade (opcional)
└──────────┬──────────┘
           │
           ▼
┌─────────────────────────┐
│ 3. System Prompt Generator│ ─────> Gera prompt reutilizável (opcional)
└─────────────────────────┘
           │
           ▼
     [Use outros prompts]
```

---

## Prompts Disponíveis

| Prompt | Descrição | Prioridade |
|--------|-----------|------------|
| [context-extractor.md](context-extractor.md) | Extrai e documenta o contexto completo do projeto | P0 - Use primeiro! |
| [development-rules.md](development-rules.md) | Define regras rigorosas de qualidade para o projeto | P1 - Para projetos sérios |
| [system-prompt-generator.md](system-prompt-generator.md) | Gera um system prompt compacto para reutilizar | P2 - Para produtividade |

---

## Quando Usar

### Context Extractor
- **Sempre** no início de uma nova conversa sobre um projeto
- Quando trocar de projeto
- Quando o projeto sofreu mudanças arquiteturais significativas

### Development Rules
- Projetos de produção/enterprise
- Quando precisar de critérios rigorosos de qualidade
- Para alinhar a IA com padrões específicos do time

### System Prompt Generator
- Quando trabalhar frequentemente com o mesmo projeto
- Para criar "atalhos" de contexto
- Para compartilhar contexto com colegas

---

## Output Esperado

Após executar os prompts de Foundation, você terá:

1. **Contexto na conversa** - A IA entende seu projeto
2. **PROJECT_CONTEXT.md** - Documento para reutilizar
3. **Regras definidas** - Padrões de qualidade claros
4. **System prompt** - Bloco para colar em novas conversas

---

## Próximos Passos

Após estabelecer o contexto, use os prompts das outras categorias:

- [developers/](../developers/_index.md) - Para desenvolvimento de código
- [architects/](../architects/_index.md) - Para decisões arquiteturais
- [security/](../security/_index.md) - Para análises de segurança
- [performance/](../performance/_index.md) - Para otimizações
