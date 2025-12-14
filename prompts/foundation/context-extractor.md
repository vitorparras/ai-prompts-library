# Extrator de Contexto do Projeto

> **Categoria**: foundation
> **Versão**: 1.0.0
> **Prioridade**: P0 - Use este prompt PRIMEIRO em toda nova conversa

---

## Quando Usar

- No início de uma nova conversa sobre um projeto específico
- Quando a IA precisa entender profundamente o contexto antes de ajudar
- Antes de solicitar implementações, reviews ou refatorações
- Quando mudar de projeto dentro da mesma ferramenta

## Quando NÃO Usar

- Perguntas genéricas não relacionadas a um projeto específico
- Quando o contexto já foi estabelecido na mesma conversa
- Para dúvidas rápidas sobre sintaxe ou conceitos gerais

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Projeto novo/pequeno | Forneça descrição textual da arquitetura pretendida |
| Projeto existente | Forneça estrutura de pastas + arquivos-chave |
| Monorepo | Especifique qual módulo/serviço está trabalhando |
| Legado sem documentação | Forneça exemplos de código representativos |

---

## Prompt

```text
Atue como um **Arquiteto de Software Senior** especializado em análise e documentação de sistemas.

<objetivo>
Preciso que você extraia e documente o contexto completo do meu projeto para que possamos trabalhar juntos de forma eficiente. Este contexto será a base para todas as nossas interações futuras nesta conversa.
</objetivo>

<raciocinio>
Analise as informações que vou fornecer considerando:

1. ESTRUTURA ARQUITETURAL
   - Qual o tipo de arquitetura? (monolito, microservices, modular, serverless, etc.)
   - Quais são as camadas e suas responsabilidades?
   - Como os componentes se comunicam?

2. PADRÕES E CONVENÇÕES
   - Quais design patterns estão em uso?
   - Quais são as convenções de nomenclatura?
   - Existe um style guide implícito no código?

3. STACK TECNOLÓGICA
   - Linguagem(ns) e versão(ões)
   - Frameworks principais
   - Bibliotecas importantes
   - Ferramentas de build/deploy

4. REGRAS DE NEGÓCIO
   - Quais são os domínios principais?
   - Que regras de negócio são evidentes no código?
   - Existem invariantes importantes?

5. QUALIDADE E TESTES
   - Qual a estratégia de testes?
   - Existem padrões de error handling?
   - Como o logging é estruturado?
</raciocinio>

<tarefa>
Com base nas informações que vou fornecer (código-fonte, estrutura de pastas, descrição, ou combinação destes), extraia e documente:

1. **Arquitetura**
   - Tipo de arquitetura
   - Camadas e responsabilidades
   - Fluxo de dados principal

2. **Convenções de Nomenclatura**
   - Padrão para classes (PascalCase, etc.)
   - Padrão para métodos/funções
   - Padrão para variáveis
   - Padrão para arquivos e pastas
   - Prefixos/sufixos comuns (I para interfaces, Dto, etc.)

3. **Estrutura de Pastas**
   - Propósito de cada diretório principal
   - Onde ficam entidades, serviços, controllers, etc.
   - Organização de testes

4. **Design Patterns em Uso**
   - Patterns identificados (Repository, Factory, Strategy, etc.)
   - Como são implementados neste projeto
   - Patterns arquiteturais (CQRS, Event Sourcing, etc.)

5. **Regras de Negócio Principais**
   - Domínios identificados
   - Regras e validações importantes
   - Fluxos de negócio críticos

6. **Stack e Dependências**
   - Linguagem e versão
   - Framework principal
   - Bibliotecas-chave
   - Ferramentas de infra (Docker, K8s, etc.)
</tarefa>

<formato-de-saida>
Gere DOIS outputs separados:

---

## OUTPUT 1: Contexto para Conversa

Um bloco conciso (máximo 500 palavras) que resume:
- Arquitetura em 1-2 frases
- Stack em 1 linha
- Padrões principais
- Convenções críticas

Este bloco será usado para manter o contexto vivo na conversa.

---

## OUTPUT 2: Arquivo PROJECT_CONTEXT.md

Um documento completo e estruturado que inclui:

```markdown
# Contexto do Projeto: [NOME]

## Visão Geral
[Descrição de 2-3 frases]

## Arquitetura
- **Tipo**: [monolito/microservices/modular/etc.]
- **Camadas**: [listar com responsabilidades]
- **Comunicação**: [sync/async, REST/gRPC/eventos]

## Stack Tecnológica
- **Linguagem**: [linguagem + versão]
- **Framework**: [framework principal]
- **Banco de Dados**: [se aplicável]
- **Infra**: [Docker, K8s, Cloud, etc.]

## Convenções de Nomenclatura

| Elemento | Convenção | Exemplo |
|----------|-----------|---------|
| Classes | | |
| Interfaces | | |
| Métodos | | |
| Variáveis | | |
| Constantes | | |
| Arquivos | | |
| Pastas | | |

## Estrutura de Pastas
```
[estrutura com explicações]
```

## Design Patterns

| Pattern | Uso | Exemplo no Projeto |
|---------|-----|-------------------|
| | | |

## Regras de Negócio Principais
1. [Regra 1]
2. [Regra 2]
3. [Regra N]

## Padrões de Código
- **Error Handling**: [como erros são tratados]
- **Logging**: [estrutura de logs]
- **Validação**: [como/onde inputs são validados]
- **Testes**: [estratégia de testes]

## Instruções para IA
Ao gerar código para este projeto:
1. [Instrução específica baseada nos padrões identificados]
2. [Instrução específica baseada nos padrões identificados]
3. [Instrução específica baseada nos padrões identificados]
```

Este documento pode ser salvo e reutilizado em novas conversas.

</formato-de-saida>

<instrucoes-finais>
- Se alguma informação não puder ser inferida, indique claramente e sugira como obtê-la
- Priorize padrões que aparecem consistentemente no código
- Identifique inconsistências ou desvios que possam indicar código legado
- Seja específico: cite arquivos e linhas quando possível
</instrucoes-finais>

Aguardo as informações do projeto. Você pode fornecer:
- Estrutura de pastas (output de `tree` ou `ls -R`)
- Arquivos de configuração (package.json, .csproj, etc.)
- Exemplos de código representativos
- Descrição textual da arquitetura
- Ou qualquer combinação dos acima
```

---

## Exemplo de Uso

### Input

```
Aqui está a estrutura do meu projeto .NET:

/src
  /Domain
    /Entities
    /ValueObjects
    /Interfaces
  /Application
    /Services
    /DTOs
    /Validators
  /Infrastructure
    /Repositories
    /ExternalServices
  /API
    /Controllers
    /Middleware

Arquivos principais:
- Usamos MediatR para CQRS
- FluentValidation para validações
- Entity Framework Core
- Serilog para logging
```

### Output Esperado

**Output 1 (Contexto para Conversa)**:
> Projeto .NET com Clean Architecture em 4 camadas (Domain, Application, Infrastructure, API). Stack: .NET + EF Core + MediatR (CQRS) + FluentValidation + Serilog. Patterns: Repository, CQRS, Value Objects. Convenções: PascalCase para classes, prefixo I para interfaces, sufixo Dto/Validator/Handler. Validação na Application layer via FluentValidation, erro handling via middleware.

**Output 2 (PROJECT_CONTEXT.md)**:
[Documento completo conforme template]

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Está muito genérico | Pouca informação fornecida | Adicione exemplos de código de 2-3 arquivos representativos |
| Faltam convenções | Código muito variado | Indique qual é o "código modelo" a seguir |
| Patterns incorretos | Código legado misturado | Especifique qual parte representa o padrão atual |
| Stack incompleta | Faltou package.json/.csproj | Forneça arquivo de dependências |

---

## Notas de Versão

| Versão | Data | Alterações |
|--------|------|------------|
| 1.0.0 | 2025-01-01 | Versão inicial |

---

## Prompts Relacionados

- [system-prompt-generator.md](system-prompt-generator.md) - Gera um system prompt compacto baseado no contexto
- [development-rules.md](development-rules.md) - Define regras de desenvolvimento baseadas no contexto
