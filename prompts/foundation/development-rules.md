# Definidor de Regras de Desenvolvimento

> **Categoria**: foundation
> **Versão**: 1.0.0
> **Pré-requisito**: Execute o `context-extractor` primeiro

---

## Quando Usar

- Quando precisar definir padrões rigorosos de qualidade para um projeto
- Antes de iniciar uma sessão de desenvolvimento intensivo
- Para alinhar a IA com os padrões de código do time
- Quando quiser um "contrato de qualidade" explícito

## Quando NÃO Usar

- Para tarefas simples e pontuais
- Quando os padrões já estão claros no contexto
- Em projetos de prototipagem rápida

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Projeto crítico (financeiro, saúde) | Enfatize segurança e auditoria |
| Projeto de alta performance | Enfatize otimizações e benchmarks |
| Projeto com equipe iniciante | Adicione mais exemplos de código |
| Projeto legado em modernização | Defina padrão "antigo" vs "novo" |

---

## Prompt

```text
Atue como um **Tech Lead Senior** responsável por definir e documentar os padrões de desenvolvimento de um projeto.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto extraído anteriormente nesta conversa. Use esse conhecimento como base para criar regras específicas e aplicáveis.
</contexto>

<objetivo>
Criar um documento de regras de desenvolvimento que servirá como "contrato de qualidade" para todo código gerado neste projeto. Estas regras garantirão consistência, qualidade, segurança e performance.
</objetivo>

<raciocinio>
Antes de definir as regras, analise:

1. CONTEXTO DO PROJETO
   - Qual o nível de criticidade? (MVP, produção, enterprise)
   - Qual a maturidade do código existente?
   - Quais são os maiores riscos técnicos?

2. PRIORIDADES
   - O que é mais importante: velocidade, qualidade ou segurança?
   - Quais trade-offs são aceitáveis?
   - Onde não pode haver exceções?

3. APLICABILIDADE
   - As regras são verificáveis?
   - São específicas o suficiente para guiar decisões?
   - São flexíveis o suficiente para casos especiais?

4. EXEMPLOS
   - Existem exemplos no projeto que demonstram o padrão correto?
   - Existem anti-exemplos a evitar?
</raciocinio>

<tarefa>
Gere um documento completo de regras de desenvolvimento com as seguintes seções:

1. **Regras de Arquitetura**
   - Dependências permitidas entre camadas
   - Responsabilidades de cada camada
   - Padrões estruturais obrigatórios

2. **Regras de Código**
   - SOLID aplicado ao projeto
   - Clean Code específico
   - Patterns obrigatórios e proibidos
   - Nomenclatura e formatação

3. **Regras de Segurança**
   - Validação de inputs
   - Autenticação/Autorização
   - Proteção de dados sensíveis
   - Práticas OWASP aplicáveis

4. **Regras de Performance**
   - Limites de complexidade algorítmica
   - Práticas de banco de dados
   - Async/await e concorrência
   - Caching

5. **Regras de Testes**
   - Cobertura mínima
   - Tipos de testes obrigatórios
   - Padrões de organização

6. **Regras de Tratamento de Erros**
   - Hierarquia de exceções
   - Logging obrigatório
   - Mensagens de erro

7. **Checklists**
   - Checklist pré-implementação
   - Checklist pós-implementação
   - Checklist de code review
</tarefa>

<formato-de-saida>

```markdown
# Regras de Desenvolvimento: [Nome do Projeto]

> **Última atualização**: [data]
> **Versão**: 1.0
> **Status**: Mandatório para todo código novo

---

## 1. Regras de Arquitetura

### 1.1 Dependências Entre Camadas

```
[Diagrama ASCII ou descrição das dependências permitidas]
```

| De → Para | Permitido | Notas |
|-----------|-----------|-------|
| | | |

### 1.2 Responsabilidades

| Camada | Deve Conter | NÃO Deve Conter |
|--------|-------------|-----------------|
| | | |

### 1.3 Padrões Estruturais

- [ ] [Padrão 1 obrigatório]
- [ ] [Padrão 2 obrigatório]

---

## 2. Regras de Código

### 2.1 SOLID

#### Single Responsibility
- [Regra específica]
- Exemplo correto: [código]
- Exemplo incorreto: [código]

#### Open/Closed
- [Regra específica]

#### Liskov Substitution
- [Regra específica]

#### Interface Segregation
- [Regra específica]

#### Dependency Inversion
- [Regra específica]

### 2.2 Clean Code

| Regra | Obrigatório | Exemplo |
|-------|-------------|---------|
| Tamanho máximo de função | X linhas | |
| Parâmetros máximos | X params | |
| Níveis de indentação | X níveis | |
| Nomenclatura | [padrão] | |

### 2.3 Patterns

| Pattern | Quando Usar | Quando NÃO Usar |
|---------|-------------|-----------------|
| | | |

### 2.4 Anti-Patterns Proibidos

- [Anti-pattern 1]: Por que é proibido
- [Anti-pattern 2]: Por que é proibido

---

## 3. Regras de Segurança

### 3.1 Validação de Inputs

- [ ] Todo input externo DEVE ser validado
- [ ] Validação no ponto de entrada
- [ ] Whitelist sobre blacklist
- Biblioteca padrão: [FluentValidation/etc]

### 3.2 Autenticação/Autorização

- [ ] [Regra específica]
- [ ] [Regra específica]

### 3.3 Dados Sensíveis

- [ ] [Como tratar senhas]
- [ ] [Como tratar PII]
- [ ] [Como tratar tokens/secrets]

### 3.4 Proteções Obrigatórias

| Vulnerabilidade | Proteção Obrigatória |
|-----------------|---------------------|
| SQL Injection | |
| XSS | |
| CSRF | |

---

## 4. Regras de Performance

### 4.1 Complexidade

| Operação | Máximo Aceitável |
|----------|-----------------|
| Busca | O(log n) |
| Listagem | O(n) |
| Ordenação | O(n log n) |

### 4.2 Banco de Dados

- [ ] Usar queries parametrizadas
- [ ] Evitar N+1 (usar Include/Join)
- [ ] Paginar resultados grandes (max: X registros)
- [ ] Índices para campos de busca frequente

### 4.3 Async/Await

- [ ] I/O bound: sempre async
- [ ] CPU bound: considerar Task.Run
- [ ] Nunca .Result ou .Wait() em código async

### 4.4 Caching

| Dado | Estratégia | TTL |
|------|------------|-----|
| | | |

---

## 5. Regras de Testes

### 5.1 Cobertura

| Tipo | Mínimo | Foco |
|------|--------|------|
| Unit | X% | Lógica de negócio |
| Integration | X% | APIs e repositórios |
| E2E | Crítico | Fluxos principais |

### 5.2 Estrutura

- Nomenclatura: [Método]_[Cenário]_[Resultado]
- Padrão: Arrange-Act-Assert
- Um assert por teste (quando possível)

### 5.3 Mocks

- Mockar: dependências externas, I/O
- NÃO mockar: lógica de domínio, value objects

---

## 6. Regras de Tratamento de Erros

### 6.1 Exceções

```
[Hierarquia de exceções do projeto]
```

### 6.2 Logging

| Nível | Quando Usar | Exemplo |
|-------|-------------|---------|
| Error | | |
| Warning | | |
| Info | | |
| Debug | | |

### 6.3 Mensagens de Erro

- Internas: detalhadas para debugging
- Externas: genéricas, sem expor implementação
- Formato: [padrão de mensagem]

---

## 7. Checklists

### 7.1 Antes de Implementar

- [ ] Entendi o requisito completamente?
- [ ] Identifiquei os componentes afetados?
- [ ] Considerei impacto em segurança?
- [ ] Considerei impacto em performance?
- [ ] Há testes que precisam ser criados/atualizados?

### 7.2 Após Implementar

- [ ] Código segue SOLID?
- [ ] Código segue Clean Code?
- [ ] Inputs são validados?
- [ ] Erros são tratados adequadamente?
- [ ] Logging está adequado?
- [ ] Testes foram criados?
- [ ] Documentação foi atualizada?

### 7.3 Code Review

- [ ] Nomenclatura clara e consistente?
- [ ] Funções pequenas e focadas?
- [ ] Sem código duplicado?
- [ ] Sem vulnerabilidades de segurança?
- [ ] Performance adequada?
- [ ] Testes adequados?
- [ ] Decisões justificadas?

---

## Justificativas

Para TODA decisão técnica, o desenvolvedor DEVE:
1. Citar o princípio, padrão ou regra que embasa a decisão
2. Explicar brevemente por que essa escolha é adequada

Formato: "Utilizando [PRINCÍPIO/PADRÃO] para [BENEFÍCIO]."
```

</formato-de-saida>

<instrucoes-finais>
- Seja específico: use nomes de classes/métodos reais do projeto quando possível
- Seja prático: regras devem ser verificáveis
- Seja realista: considere a maturidade atual do projeto
- Inclua exemplos de código quando ajudar na clareza
</instrucoes-finais>

Gere o documento de regras agora baseado no contexto do projeto já extraído.
```

---

## Exemplo de Uso

### Contexto Prévio
```
Projeto: API de Pagamentos
Stack: .NET 8, Clean Architecture, Domain-Driven Design
Criticidade: Alta (sistema financeiro)
Padrões: CQRS, Event Sourcing, Repository
```

### Output Esperado (Trecho)

```markdown
## 3. Regras de Segurança

### 3.3 Dados Sensíveis

- [ ] Números de cartão: NUNCA logar, armazenar tokenizado
- [ ] CPF/CNPJ: mascarar em logs (XXX.XXX.XXX-**)
- [ ] Senhas: BCrypt com work factor ≥ 12
- [ ] Tokens JWT: HttpOnly + Secure + SameSite=Strict

### 3.4 Proteções Obrigatórias

| Vulnerabilidade | Proteção Obrigatória |
|-----------------|---------------------|
| SQL Injection | EF Core com LINQ (nunca raw SQL) |
| XSS | Razor encoding automático |
| CSRF | ValidateAntiForgeryToken em todas as mutations |
| Timing Attack | Comparação segura de hashes |
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Regras muito genéricas | Falta contexto específico | Forneça exemplos de código do projeto |
| Faltam regras críticas | Contexto incompleto | Especifique a criticidade do projeto |
| Conflitos entre regras | Padrões mistos no projeto | Defina qual padrão é o "correto" |
| Muito extenso | Projeto muito complexo | Peça para priorizar top 10 regras |

---

## Notas de Versão

| Versão | Data | Alterações |
|--------|------|------------|
| 1.0.0 | 2025-01-01 | Versão inicial |

---

## Prompts Relacionados

- [context-extractor.md](context-extractor.md) - Extrai o contexto (use primeiro)
- [system-prompt-generator.md](system-prompt-generator.md) - Gera system prompt compacto
- [../developers/code-review/quality-reviewer.md](../developers/code-review/quality-reviewer.md) - Aplica as regras em reviews
