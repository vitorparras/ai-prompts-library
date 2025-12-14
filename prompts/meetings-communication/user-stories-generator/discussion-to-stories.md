# Gerador de User Stories a partir de Discussões

> **Categoria**: meetings-communication/user-stories-generator
> **Versão**: 1.0.0
> **Prioridade**: Alta - Uso frequente

---

## Quando Usar

- Após reuniões de refinamento ou discovery
- Quando receber requisitos informais de stakeholders
- Para transformar ideias em demandas estruturadas
- Após sessões de brainstorming
- Quando precisar criar backlog a partir de transcrições

## Quando NÃO Usar

- Quando já existem user stories definidas
- Para documentação técnica (use prompts de documentação)
- Para bugs (use formato de bug report)

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Projeto ágil (Scrum) | Formato user story com acceptance criteria |
| Projeto Kanban | Formato mais simples, sem story points |
| Requisitos técnicos | Adicione seção de considerações técnicas |
| MVP | Priorize por valor de negócio |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{DISCUSSAO}}` | Transcrição ou notas da discussão | [texto da reunião] | Sim |
| `{{PROJETO}}` | Nome/contexto do projeto | "App de delivery" | Não |
| `{{FORMATO}}` | Formato preferido | "Scrum", "Kanban", "simples" | Não |

---

## Prompt

```text
Atue como um **Product Owner Senior** especializado em transformar requisitos informais em user stories bem estruturadas e acionáveis.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Use esse conhecimento para criar stories alinhadas com a arquitetura e padrões existentes.
</contexto>

<raciocinio>
Antes de criar as user stories, analise:

1. IDENTIFICAÇÃO DE FUNCIONALIDADES
   - Quais funcionalidades foram discutidas?
   - Quais são requisitos explícitos vs implícitos?
   - Quais são os atores/usuários envolvidos?
   - Qual o valor de negócio de cada funcionalidade?

2. DECOMPOSIÇÃO
   - A funcionalidade pode ser dividida em stories menores?
   - Qual o tamanho ideal para cada story (entregável em 1 sprint)?
   - Há dependências entre stories?

3. COMPLETUDE
   - Os critérios de aceite estão claros?
   - Os edge cases foram considerados?
   - Requisitos não-funcionais foram mencionados?

4. PRIORIZAÇÃO
   - O que entrega mais valor primeiro?
   - O que é pré-requisito de outras stories?
   - O que pode ser deixado para depois (nice-to-have)?

5. VIABILIDADE TÉCNICA
   - A story é tecnicamente viável?
   - Há considerações técnicas importantes?
   - Impacta sistemas existentes?
</raciocinio>

<tarefa>
Transforme a seguinte discussão em user stories estruturadas:

**Discussão/Notas:**
```
{{DISCUSSAO}}
```

**Projeto:** {{PROJETO}}

**Formato preferido:** {{FORMATO}}
</tarefa>

<formato-de-user-story>
Para cada user story, use o formato INVEST:

**I**ndependente: Pode ser desenvolvida isoladamente
**N**egociável: Detalhes podem ser discutidos
**V**aliosa: Entrega valor ao usuário/negócio
**E**stimável: Pode ser estimada pelo time
**S**mall: Pequena o suficiente para um sprint
**T**estável: Tem critérios de aceite verificáveis
</formato-de-user-story>

<formato-de-resposta>
## Resumo da Discussão
[2-3 frases resumindo os principais pontos discutidos]

## Funcionalidades Identificadas
| # | Funcionalidade | Ator | Valor | Complexidade Est. |
|---|----------------|------|-------|-------------------|
| 1 | | | Alta/Média/Baixa | P/M/G |

## User Stories

---

### US-001: [Título Curto e Descritivo]

**Como** [tipo de usuário]
**Quero** [funcionalidade desejada]
**Para que** [benefício/valor]

#### Descrição
[Descrição detalhada da funcionalidade]

#### Critérios de Aceite
```gherkin
DADO que [contexto inicial]
QUANDO [ação do usuário]
ENTÃO [resultado esperado]

DADO que [outro contexto]
QUANDO [outra ação]
ENTÃO [outro resultado]
```

#### Regras de Negócio
- RN-01: [Regra de negócio 1]
- RN-02: [Regra de negócio 2]

#### Considerações Técnicas
- [Consideração técnica 1]
- [Consideração técnica 2]

#### Edge Cases
- [ ] [Edge case 1]
- [ ] [Edge case 2]

#### Dependências
- Depende de: [US-XXX] ou [sistema externo]
- Bloqueia: [US-YYY]

#### Estimativa Sugerida
- **Complexidade**: [Fibonacci: 1, 2, 3, 5, 8, 13]
- **Prioridade**: [Must Have / Should Have / Could Have / Won't Have]

---

### US-002: [Título]
...

---

## Épico (se aplicável)
[Se as stories fazem parte de um épico maior, descreva aqui]

## Backlog Priorizado

| Prioridade | Story | Valor | Esforço | Dependências |
|------------|-------|-------|---------|--------------|
| 1 | US-001 | Alto | 5 | - |
| 2 | US-002 | Alto | 3 | US-001 |

## Itens Pendentes de Clarificação
[Pontos que precisam de mais informação antes de refinar]

1. [Pergunta 1]
2. [Pergunta 2]

## Sugestões de Melhoria
[Funcionalidades não mencionadas que poderiam agregar valor]

1. [Sugestão 1]
2. [Sugestão 2]
</formato-de-resposta>

<justificativas>
Para cada user story:
1. Explique por que foi estruturada dessa forma
2. Explique a priorização sugerida
3. Identifique riscos ou incertezas

Para critérios de aceite:
1. Cubra happy path e principais edge cases
2. Seja específico e verificável
3. Evite ambiguidade
</justificativas>
```

---

## Exemplo de Uso

### Input

```
{{DISCUSSAO}}:
Reunião de refinamento - App de Delivery

João (PO): Pessoal, precisamos implementar a funcionalidade de cupom de desconto.
O usuário deve poder aplicar um cupom na tela de checkout.

Maria (Dev): Que tipos de cupom? Porcentagem ou valor fixo?

João: Os dois. E também precisamos de cupons de frete grátis.

Pedro (Dev): Tem limite de uso? Validade?

João: Sim, cada cupom tem validade e limite de usos. Também tem cupom
que vale só pra primeira compra. Ah, e cupom de frete grátis só vale
pra compras acima de R$50.

Maria: E se o cupom for inválido?

João: Mostra mensagem de erro explicando o motivo: expirado, já usado,
valor mínimo não atingido, etc.

{{PROJETO}}: App de Delivery - FlexFood
{{FORMATO}}: Scrum
```

### Output Esperado

```markdown
## Resumo da Discussão
Implementação de sistema de cupons de desconto com suporte a múltiplos tipos
(porcentagem, valor fixo, frete grátis), validações de uso e feedback ao usuário.

## User Stories

### US-001: Aplicar Cupom de Desconto Percentual

**Como** cliente
**Quero** aplicar um cupom de desconto percentual no checkout
**Para que** eu possa economizar um percentual do valor da compra

#### Critérios de Aceite
```gherkin
DADO que estou na tela de checkout com itens no carrinho
E possuo um cupom de desconto percentual válido
QUANDO informo o código do cupom e clico em "Aplicar"
ENTÃO o desconto percentual é aplicado ao subtotal
E o novo valor total é exibido
```
...
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Stories muito grandes | Discussão muito ampla | Peça para dividir em épicos |
| Faltam critérios | Discussão vaga | Forneça mais contexto |
| Muito técnico | Foco em implementação | Reforce foco no usuário |

---

## Prompts Relacionados

- [../transcription-summary/meeting-summarizer.md](../transcription-summary/meeting-summarizer.md) - Resumir a reunião primeiro
- [../action-items-extractor/action-extractor.md](../action-items-extractor/action-extractor.md) - Extrair ações
- [../../developers/code-generation/feature-implementation.md](../../developers/code-generation/feature-implementation.md) - Implementar as stories
