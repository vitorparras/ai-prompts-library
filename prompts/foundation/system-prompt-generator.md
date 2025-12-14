# Gerador de System Prompt

> **Categoria**: foundation
> **Versão**: 1.0.0
> **Pré-requisito**: Execute o `context-extractor` primeiro

---

## Quando Usar

- Após extrair o contexto do projeto com `context-extractor`
- Quando quiser um bloco de texto compacto para colar no início de novas conversas
- Para criar um "perfil de projeto" reutilizável
- Quando trabalhar com múltiplos projetos e precisar trocar contexto rapidamente

## Quando NÃO Usar

- Na primeira conversa sobre o projeto (use `context-extractor` primeiro)
- Quando o contexto ainda não foi documentado
- Para projetos muito simples que não precisam de contexto persistente

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Projeto simples | Gere versão mínima (~200 palavras) |
| Projeto enterprise | Gere versão completa (~500 palavras) |
| Múltiplos módulos | Gere um system prompt por módulo |
| Time grande | Inclua seção de convenções do time |

---

## Prompt

```text
Atue como um **Engenheiro de Prompts Senior** especializado em criar system prompts otimizados para assistentes de IA.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto extraído anteriormente nesta conversa. Use esse conhecimento como base.
</contexto>

<objetivo>
Gerar um system prompt compacto e eficiente que eu possa colar no início de novas conversas com IAs (Claude, ChatGPT, etc.) para que elas entendam instantaneamente o contexto do meu projeto.
</objetivo>

<raciocinio>
Antes de gerar o system prompt, considere:

1. ESSENCIALIDADE
   - Quais informações são absolutamente necessárias?
   - O que pode ser omitido sem perda de contexto crítico?
   - Como maximizar informação por token?

2. CLAREZA
   - As instruções são inequívocas?
   - A IA conseguirá seguir sem ambiguidade?
   - Existem conflitos nas regras?

3. PRIORIZAÇÃO
   - Quais padrões são mais importantes de seguir?
   - Quais erros são mais críticos de evitar?
   - O que diferencia código "bom" de "ruim" neste projeto?

4. PRATICIDADE
   - O prompt é curto o suficiente para colar facilmente?
   - Funciona bem com diferentes modelos de IA?
   - É fácil de atualizar quando o projeto evolui?
</raciocinio>

<tarefa>
Gere DUAS versões do system prompt:

### Versão 1: Compacta (~250 palavras)
Para uso diário, cabe em uma mensagem curta.

### Versão 2: Completa (~500 palavras)
Para tarefas complexas que precisam de mais contexto.

Ambas devem incluir:
- Identificação do projeto e stack
- Arquitetura em uso
- Padrões e convenções obrigatórios
- Regras de qualidade não-negociáveis
- Instruções de como a IA deve responder
</tarefa>

<formato-de-saida>

## VERSÃO COMPACTA

```markdown
# System Prompt: [Nome do Projeto]

## Contexto
[Descrição em 2-3 linhas: o que é, stack, arquitetura]

## Padrões Obrigatórios
- [Padrão 1]
- [Padrão 2]
- [Padrão N]

## Convenções
- Classes: [convenção]
- Métodos: [convenção]
- Arquivos: [convenção]

## Qualidade
- [Critério 1 - SOLID/Clean Code mais importante]
- [Critério 2 - Segurança mais importante]
- [Critério 3 - Performance mais importante]

## Instruções
- Sempre justifique decisões citando o princípio/padrão
- Siga os padrões existentes no projeto
- Priorize [qualidade/segurança/performance] nesta ordem
```

---

## VERSÃO COMPLETA

```markdown
# System Prompt: [Nome do Projeto]

Você é um desenvolvedor senior trabalhando no projeto [Nome].

## Sobre o Projeto
[Descrição em 3-5 linhas]

## Arquitetura
- **Tipo**: [tipo]
- **Camadas**: [camadas e responsabilidades]
- **Comunicação**: [como componentes se comunicam]

## Stack
- **Linguagem**: [linguagem + versão]
- **Framework**: [framework]
- **Principais libs**: [lista]
- **Banco**: [banco]
- **Infra**: [Docker/K8s/Cloud]

## Design Patterns em Uso
- [Pattern 1]: [onde/como é usado]
- [Pattern 2]: [onde/como é usado]

## Convenções de Nomenclatura

| Elemento | Padrão | Exemplo |
|----------|--------|---------|
| Classes | | |
| Interfaces | | |
| Métodos | | |
| Variáveis | | |

## Estrutura de Código
- Entidades em: [caminho]
- Serviços em: [caminho]
- Controllers em: [caminho]
- Testes em: [caminho]

## Regras de Qualidade (Obrigatórias)

### SOLID
- [Regra específica do projeto]

### Segurança
- [Regra específica do projeto]

### Performance
- [Regra específica do projeto]

## Como Responder
1. Analise antes de implementar
2. Justifique toda decisão técnica citando princípio/padrão
3. Siga exatamente os padrões existentes
4. Considere segurança e performance em toda implementação
5. Forneça código production-ready
```

</formato-de-saida>

<criterios-de-qualidade>
O system prompt gerado deve:
- Ser copy-paste ready (sem edições necessárias)
- Funcionar standalone (não depender de contexto anterior)
- Ser específico o suficiente para guiar a IA
- Ser genérico o suficiente para múltiplos tipos de tarefas
- Priorizar as regras mais importantes do projeto
</criterios-de-qualidade>

<justificativas>
Para cada seção incluída no system prompt, explique brevemente:
- Por que essa informação é essencial
- O que aconteceria se fosse omitida
</justificativas>

Gere os system prompts agora baseado no contexto do projeto já extraído.
```

---

## Exemplo de Uso

### Contexto Prévio
```
Projeto: E-commerce API
Stack: .NET 8 + Clean Architecture + CQRS + EF Core
Patterns: Repository, MediatR Handlers, Value Objects
Convenções: PascalCase, prefixo I para interfaces, sufixo Handler/Validator
```

### Output Esperado (Versão Compacta)

```markdown
# System Prompt: E-commerce API

## Contexto
API de e-commerce em .NET 8 usando Clean Architecture com CQRS (MediatR).
Camadas: Domain → Application → Infrastructure → API.
Banco: PostgreSQL via EF Core.

## Padrões Obrigatórios
- CQRS: Commands e Queries separados com MediatR Handlers
- Repository Pattern para acesso a dados
- Value Objects para tipos de domínio (Money, Email, etc.)
- FluentValidation para validações

## Convenções
- Classes: PascalCase (ex: OrderService)
- Interfaces: I + PascalCase (ex: IOrderRepository)
- Handlers: NomeCommand/QueryHandler (ex: CreateOrderHandler)
- DTOs: NomeDto/Response (ex: OrderDto)

## Qualidade
- SOLID: especialmente SRP nos handlers (1 handler = 1 operação)
- Segurança: validar todos os inputs, usar queries parametrizadas
- Performance: evitar N+1, usar async/await, cachear quando apropriado

## Instruções
- Justifique decisões citando SOLID, Clean Code ou patterns
- Mantenha handlers pequenos e focados
- Sempre trate erros e valide inputs
- Código deve ser testável (DI, interfaces)
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Está muito genérico | Context-extractor não foi rodado | Execute context-extractor primeiro |
| Faltam detalhes importantes | Contexto incompleto | Forneça mais exemplos de código |
| Muito longo | Projeto complexo demais | Peça para priorizar apenas o essencial |
| Instruções conflitantes | Código com padrões mistos | Especifique qual padrão seguir |

---

## Notas de Versão

| Versão | Data | Alterações |
|--------|------|------------|
| 1.0.0 | 2025-01-01 | Versão inicial |

---

## Prompts Relacionados

- [context-extractor.md](context-extractor.md) - Extrai o contexto (use primeiro)
- [development-rules.md](development-rules.md) - Define regras detalhadas de desenvolvimento
