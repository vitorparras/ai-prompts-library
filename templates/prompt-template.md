# [Nome do Prompt]

> **Categoria**: [categoria/subcategoria]
> **Versão**: 1.0.0
> **Última Atualização**: [data]

---

## Quando Usar

- [Cenário ideal 1]
- [Cenário ideal 2]
- [Cenário ideal 3]

## Quando NÃO Usar

- [Situação inadequada 1]
- [Situação inadequada 2]

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Projetos pequenos | [Como ajustar] |
| Projetos enterprise | [Como ajustar] |
| Times iniciantes | [Como ajustar] |
| Código legado | [Como ajustar] |

---

## Variáveis

> **Nota**: Preencha estas variáveis antes de usar o prompt.
> Variáveis de contexto geral (linguagem, arquitetura) já devem estar na conversa via `context-extractor`.

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{VARIAVEL_1}}` | [O que representa] | [Exemplo prático] | Sim |
| `{{VARIAVEL_2}}` | [O que representa] | [Exemplo prático] | Não |

---

## Prompt

```text
Atue como um [PAPEL] senior especialista em [ESPECIALIZAÇÃO].

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Considere a arquitetura, padrões de nomenclatura, estrutura de pastas, design patterns em uso e regras de negócio já estabelecidos.
</contexto>

<raciocinio>
Antes de executar a tarefa, analise sistematicamente:

1. COMPREENSÃO
   - Quais são os requisitos explícitos?
   - Quais são os requisitos implícitos?
   - Quais restrições técnicas existem?
   - Quais são os impactos potenciais?

2. ALTERNATIVAS
   - Abordagem A: [descrição]
     - Prós: [listar]
     - Contras: [listar]
   - Abordagem B: [descrição]
     - Prós: [listar]
     - Contras: [listar]
   - Escolha justificada baseada em [princípio/padrão]

3. VALIDAÇÃO
   - [ ] Segue SOLID principles?
   - [ ] Segue Clean Code?
   - [ ] Atende critérios de segurança (OWASP)?
   - [ ] Atende critérios de performance?
</raciocinio>

<tarefa>
[Descrição clara e específica da tarefa a ser executada]

Entrada:
{{VARIAVEL_1}}

Requisitos adicionais:
{{VARIAVEL_2}}
</tarefa>

<criterios-de-qualidade>
Aplique rigorosamente:

SOLID Principles:
- SRP: Cada classe/função deve ter uma única responsabilidade
- OCP: Aberto para extensão, fechado para modificação
- LSP: Subtipos devem ser substituíveis por seus tipos base
- ISP: Interfaces específicas são melhores que uma interface geral
- DIP: Dependa de abstrações, não de implementações concretas

Clean Code:
- Nomenclatura clara e expressiva
- Funções pequenas e focadas
- Sem duplicação de código (DRY)
- Comentários apenas quando necessário
- Tratamento de erros consistente
- Logging estruturado onde apropriado
</criterios-de-qualidade>

<criterios-de-seguranca>
Aplique as práticas OWASP:
- Valide e sanitize TODOS os inputs
- Previna SQL Injection usando queries parametrizadas
- Previna XSS escapando outputs
- Previna CSRF com tokens apropriados
- Gerencie secrets via variáveis de ambiente ou vault
- Implemente autenticação/autorização quando aplicável
- Use criptografia para dados sensíveis
</criterios-de-seguranca>

<criterios-de-performance>
Otimize considerando:
- Complexidade algorítmica (prefira O(1), O(log n), O(n))
- Evite queries N+1 em operações de banco
- Previna memory leaks (dispose, using, etc.)
- Use async/await corretamente para I/O
- Considere caching para dados frequentemente acessados
- Use connection pooling para conexões de banco/rede
</criterios-de-performance>

<formato-de-resposta>
[Especifique exatamente como a resposta deve ser formatada]
</formato-de-resposta>

<justificativas>
OBRIGATÓRIO: Para TODA decisão técnica você deve:
1. Citar o princípio, padrão ou best practice por nome
2. Explicar brevemente POR QUE essa escolha é a melhor para o contexto

Formato: "Utilizando [PRINCÍPIO/PADRÃO] para [BENEFÍCIO ESPECÍFICO]."

Exemplo: "Utilizando o padrão Repository (DDD) para isolar a lógica de acesso a dados, facilitando testes unitários e permitindo trocar a implementação do banco sem impactar a camada de domínio."
</justificativas>
```

---

## Exemplo de Uso

### Input

```
[Exemplo de como preencher as variáveis e usar o prompt]
```

### Output Esperado

```
[Exemplo do tipo de resposta esperada]
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Está muito genérico | Falta contexto do projeto | Rode o `context-extractor` primeiro |
| Ignora padrões do projeto | Contexto não foi absorvido | Reforce os padrões específicos na variável |
| Não segue SOLID | Prompt muito curto | Adicione exemplos do projeto atual |
| Falta segurança | Requisitos não enfatizados | Destaque os requisitos de segurança |
| Performance ruim | Cenário não claro | Especifique volume de dados esperado |
| Decisões não justificadas | IA ignorou seção | Reforce a obrigatoriedade das justificativas |

---

## Notas de Versão

| Versão | Data | Alterações |
|--------|------|------------|
| 1.0.0 | [data] | Versão inicial |

---

## Prompts Relacionados

- [prompt-relacionado-1.md](caminho/prompt-relacionado-1.md)
- [prompt-relacionado-2.md](caminho/prompt-relacionado-2.md)
