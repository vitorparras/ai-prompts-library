# AI Prompts Library

Biblioteca de prompts de IA de alta qualidade para profissionais de tecnologia. Focada em entregar código de **nível senior** com critérios rigorosos de qualidade, segurança e performance.

## Filosofia

Esta biblioteca foi projetada com base em três pilares:

1. **Chain-of-Thought**: Todos os prompts exigem raciocínio antes da ação
2. **Justificativas Embasadas**: Toda decisão técnica cita o princípio ou padrão aplicado
3. **Qualidade Senior**: Critérios rigorosos de SOLID, Clean Code, OWASP e Performance

---

## Como Usar

### Fluxo Recomendado

```
┌─────────────────────────────────────────────────────────────────┐
│  1. INÍCIO DA CONVERSA                                          │
│     └─> Rode o prompt "Extrator de Contexto"                    │
│         - A IA entende arquitetura, padrões, regras do projeto  │
│         - Gera conhecimento na conversa                         │
│         - (Opcional) Gera arquivo PROJECT_CONTEXT.md            │
├─────────────────────────────────────────────────────────────────┤
│  2. DURANTE A CONVERSA                                          │
│     └─> Cole apenas o próximo prompt                            │
│         - Contexto já está na conversa                          │
│         - Sem variáveis repetitivas                             │
│         - Apenas variáveis específicas do prompt                │
├─────────────────────────────────────────────────────────────────┤
│  3. NOVA CONVERSA (mesmo projeto)                               │
│     └─> Cole o PROJECT_CONTEXT.md gerado                        │
│         - Ou rode o Extrator novamente                          │
└─────────────────────────────────────────────────────────────────┘
```

### Passo a Passo

1. **Inicie com Contexto**
   - Abra `prompts/foundation/context-extractor.md`
   - Cole o prompt em uma nova conversa
   - Forneça informações do seu projeto (código, estrutura, descrição)
   - Salve o `PROJECT_CONTEXT.md` gerado para uso futuro

2. **Use os Prompts Específicos**
   - Navegue até a categoria desejada (ex: `prompts/developers/code-review/`)
   - Escolha o prompt adequado
   - Cole na mesma conversa (o contexto já está lá)
   - Preencha apenas as variáveis específicas do prompt

3. **Reutilize em Novas Conversas**
   - Cole o `PROJECT_CONTEXT.md` no início
   - Continue usando os prompts normalmente

---

## Estrutura de Pastas

```
ai-prompts-library/
├── docs/                              # Documentação e critérios
│   ├── getting-started.md
│   ├── prompt-engineering-guide.md
│   ├── quality-criteria.md           # SOLID, Clean Code
│   ├── security-criteria.md          # OWASP, validação
│   └── performance-criteria.md       # Big O, N+1, caching
│
├── templates/                         # Templates para criar prompts
│   ├── prompt-template.md
│   └── context-output-template.md
│
├── prompts/
│   ├── foundation/                    # Prompts fundamentais (use primeiro!)
│   │   ├── context-extractor.md      # Extrai contexto do projeto
│   │   ├── system-prompt-generator.md # Gera system prompt reutilizável
│   │   └── development-rules.md       # Define regras de desenvolvimento
│   │
│   ├── developers/                    # Desenvolvimento de software
│   │   ├── code-generation/
│   │   ├── debugging/
│   │   ├── refactoring/
│   │   ├── code-review/
│   │   ├── testing/
│   │   └── documentation/
│   │
│   ├── architects/                    # Arquitetura de software
│   │   ├── design-patterns/
│   │   ├── system-design/
│   │   ├── architecture-decisions/
│   │   ├── technical-documentation/
│   │   └── api-design/
│   │
│   ├── frontend/                      # Desenvolvimento frontend
│   │   ├── components/
│   │   ├── state-management/
│   │   ├── accessibility/
│   │   └── responsive-design/
│   │
│   ├── backend/                       # Desenvolvimento backend
│   │   ├── apis/
│   │   ├── messaging/
│   │   ├── caching/
│   │   └── queues/
│   │
│   ├── database/                      # Banco de dados
│   │   ├── modeling/
│   │   ├── optimization/
│   │   ├── migrations/
│   │   └── queries/
│   │
│   ├── infrastructure/                # Infraestrutura
│   │   ├── server-configuration/
│   │   ├── networking/
│   │   ├── monitoring/
│   │   └── cloud-services/
│   │
│   ├── devops/                        # DevOps e CI/CD
│   │   ├── ci-cd/
│   │   ├── containers/
│   │   ├── orchestration/
│   │   ├── automation/
│   │   └── iac/
│   │
│   ├── security/                      # Segurança
│   │   ├── owasp/
│   │   ├── authentication/
│   │   ├── encryption/
│   │   ├── vulnerability-assessment/
│   │   └── code-security-review/
│   │
│   ├── performance/                   # Performance
│   │   ├── profiling/
│   │   ├── optimization/
│   │   ├── caching/
│   │   ├── database-performance/
│   │   └── memory-management/
│   │
│   ├── risk-management/               # Gestão de riscos
│   │   ├── risk-assessment/
│   │   ├── mitigation/
│   │   └── contingency/
│   │
│   └── meetings-communication/        # Reuniões e comunicação
│       ├── transcription-summary/
│       ├── action-items-extractor/
│       ├── user-stories-generator/
│       ├── professional-emails/
│       └── status-reports/
│
└── examples/                          # Exemplos de uso
    ├── context-output-example.md
    └── workflow-example.md
```

---

## Categorias

| Categoria | Descrição | Prompts |
|-----------|-----------|---------|
| **Foundation** | Prompts fundamentais - use primeiro! | 3 |
| **Developers** | Código, debug, refactoring, testes | 20 |
| **Architects** | Design patterns, system design, ADRs | 15 |
| **Frontend** | Componentes, state, acessibilidade | 10 |
| **Backend** | APIs, mensageria, cache, filas | 10 |
| **Database** | Modelagem, queries, migrations | 10 |
| **Infrastructure** | Servers, networking, cloud | 10 |
| **DevOps** | CI/CD, containers, IaC | 15 |
| **Security** | OWASP, auth, criptografia | 10 |
| **Performance** | Profiling, otimização, cache | 10 |
| **Risk Management** | Avaliação e mitigação de riscos | 5 |
| **Meetings** | User stories, resumos, emails | 10 |

---

## Critérios de Qualidade

Todos os prompts de código seguem estes critérios:

### Qualidade de Código
- SOLID Principles (SRP, OCP, LSP, ISP, DIP)
- Clean Code (nomenclatura, funções pequenas, sem duplicação)
- Design Patterns apropriados
- Tratamento de erros padronizado
- Logging estruturado
- Validação de inputs
- Testabilidade

### Segurança (OWASP)
- Validação e sanitização de inputs
- Prevenção de SQL Injection, XSS, CSRF
- Autenticação/Autorização adequadas
- Secrets management
- Criptografia apropriada

### Performance
- Complexidade algorítmica otimizada (Big O)
- Prevenção de queries N+1
- Memory leak prevention
- Async/await correto
- Caching strategies
- Connection pooling

---

## Estrutura de um Prompt

Cada prompt segue esta estrutura:

```markdown
# Nome do Prompt

## Quando Usar
- Cenários ideais para uso

## Quando NÃO Usar
- Situações onde não é adequado

## Como Adaptar
- Adaptações para diferentes contextos

## Variáveis
| Variável | Descrição | Exemplo |

## Prompt
[O prompt em si, com Chain-of-Thought integrado]

## Troubleshooting
| Problema | Causa | Solução |
```

---

## Começo Rápido

1. Clone o repositório
2. Abra `prompts/foundation/context-extractor.md`
3. Cole o prompt no Claude ou ChatGPT
4. Forneça informações do seu projeto
5. Guarde o contexto gerado
6. Use os demais prompts conforme necessário

---

## Contribuindo

Veja [CONTRIBUTING.md](CONTRIBUTING.md) para diretrizes de contribuição.

---

## Licença

MIT License - veja [LICENSE](LICENSE) para detalhes.
