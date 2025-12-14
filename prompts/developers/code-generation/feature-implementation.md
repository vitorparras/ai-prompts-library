# Implementador de Features

> **Categoria**: developers/code-generation
> **Versão**: 1.0.0

---

## Quando Usar

- Implementar uma nova funcionalidade completa
- Criar um novo módulo/componente
- Adicionar um novo endpoint de API
- Implementar uma regra de negócio

## Quando NÃO Usar

- Funções simples e isoladas (use `function-creator`)
- Refatoração de código existente (use `code-improver`)
- Correção de bugs (use `error-analyzer`)

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Feature simples | Reduza o escopo da análise |
| Feature crítica | Enfatize segurança e testes |
| Feature de performance | Enfatize otimizações |
| Integração externa | Enfatize tratamento de erros |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{DESCRICAO_FEATURE}}` | O que a feature deve fazer | "Permitir que usuários redefinam senha via email" | Sim |
| `{{REQUISITOS}}` | Requisitos específicos ou restrições | "Deve expirar em 24h, máximo 3 tentativas" | Não |
| `{{ARQUIVOS_RELACIONADOS}}` | Arquivos existentes que serão afetados | "UserController.cs, EmailService.cs" | Não |

---

## Prompt

```text
Atue como um **Desenvolvedor Senior** especialista em implementar features de alta qualidade.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Considere a arquitetura, padrões de nomenclatura, estrutura de pastas, design patterns em uso e regras de negócio já estabelecidos.
</contexto>

<raciocinio>
Antes de implementar, analise sistematicamente:

1. COMPREENSÃO DO REQUISITO
   - O que exatamente a feature deve fazer?
   - Quais são os casos de uso principais?
   - Quais são os edge cases?
   - Quais são as restrições técnicas?
   - Como essa feature se integra com o sistema existente?

2. DESIGN DA SOLUÇÃO
   Considere múltiplas abordagens:

   - Abordagem A: [descrição]
     - Prós: [listar]
     - Contras: [listar]
     - Complexidade: [baixa/média/alta]

   - Abordagem B: [descrição]
     - Prós: [listar]
     - Contras: [listar]
     - Complexidade: [baixa/média/alta]

   Escolha justificada: [qual abordagem e por quê, citando princípios]

3. VALIDAÇÃO
   Antes de escrever o código, verifique:
   - [ ] Segue os padrões de arquitetura do projeto?
   - [ ] Está alinhado com SOLID principles?
   - [ ] Considera aspectos de segurança?
   - [ ] Considera aspectos de performance?
   - [ ] É testável?
</raciocinio>

<tarefa>
Implemente a seguinte feature:

{{DESCRICAO_FEATURE}}

Requisitos adicionais:
{{REQUISITOS}}

Arquivos relacionados (se houver):
{{ARQUIVOS_RELACIONADOS}}
</tarefa>

<criterios-de-qualidade>
O código DEVE:

ARQUITETURA
- Respeitar as camadas e responsabilidades do projeto
- Seguir os design patterns já utilizados
- Manter a separação de concerns

SOLID
- SRP: Cada classe/método com responsabilidade única
- OCP: Extensível sem modificar código existente
- LSP: Subtipos substituíveis
- ISP: Interfaces coesas
- DIP: Dependências via abstração (injeção de dependência)

CLEAN CODE
- Nomenclatura clara e expressiva
- Funções pequenas (< 20 linhas idealmente)
- Sem duplicação de código
- Código auto-documentado

SEGURANÇA
- Validar TODOS os inputs
- Sanitizar dados para prevenir injection
- Não expor informações sensíveis
- Aplicar autorização quando necessário

PERFORMANCE
- Complexidade algorítmica adequada
- Evitar queries N+1
- Usar async/await para I/O
- Considerar caching quando apropriado
</criterios-de-qualidade>

<formato-de-resposta>
Estruture sua resposta assim:

## Análise
[Seu raciocínio sobre o problema e a solução escolhida]

## Decisões de Design
[Justificativas para as principais decisões, citando princípios]

## Arquivos a Criar/Modificar
[Lista de arquivos com descrição do que cada um faz]

## Implementação

### [Nome do Arquivo 1]
```[linguagem]
[código completo]
```
**Justificativa**: [Por que este design, citando princípio/padrão]

### [Nome do Arquivo 2]
```[linguagem]
[código completo]
```
**Justificativa**: [Por que este design, citando princípio/padrão]

## Testes Sugeridos
[Lista de casos de teste que devem ser implementados]

## Considerações de Segurança
[Aspectos de segurança considerados]

## Considerações de Performance
[Aspectos de performance considerados]
</formato-de-resposta>

<justificativas>
OBRIGATÓRIO: Para CADA decisão de design significativa:
1. Cite o princípio, padrão ou best practice aplicado
2. Explique POR QUE essa escolha é a melhor para este contexto

Exemplo: "Utilizando o padrão Strategy para encapsular os diferentes métodos de envio de notificação (email, SMS, push), permitindo adicionar novos canais sem modificar o código existente (OCP)."
</justificativas>
```

---

## Exemplo de Uso

### Input

```
{{DESCRICAO_FEATURE}}: Implementar funcionalidade de recuperação de senha via email

{{REQUISITOS}}:
- Link de recuperação expira em 24 horas
- Máximo de 3 tentativas por hora
- Email deve usar template HTML
- Deve invalidar tokens anteriores ao gerar novo

{{ARQUIVOS_RELACIONADOS}}:
- UserController.cs
- EmailService.cs
- User.cs
```

### Output Esperado

```markdown
## Análise
A feature requer: geração de token seguro, envio de email, validação temporal,
rate limiting e invalidação de tokens anteriores...

## Decisões de Design
1. **Utilizando padrão Repository** para acesso aos tokens de recuperação,
   isolando a lógica de persistência (DIP).
2. **Utilizando serviço dedicado** (PasswordRecoveryService) para orquestrar
   o fluxo, mantendo o controller fino (SRP)...

## Implementação
...
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Não segue a arquitetura | Contexto insuficiente | Rode o context-extractor primeiro |
| Muito complexo | Feature muito grande | Divida em sub-features |
| Faltam validações | Requisitos incompletos | Especifique edge cases |
| Sem justificativas | IA ignorou instrução | Reforce a obrigatoriedade |

---

## Prompts Relacionados

- [function-creator.md](function-creator.md) - Para funções específicas
- [../testing/unit-test-generator.md](../testing/unit-test-generator.md) - Para criar testes
- [../code-review/quality-reviewer.md](../code-review/quality-reviewer.md) - Para revisar a implementação
