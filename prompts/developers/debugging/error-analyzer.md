# Analisador de Erros

> **Categoria**: developers/debugging
> **Versão**: 1.0.0

---

## Quando Usar

- Diagnosticar exceções e stack traces
- Entender a causa raiz de um erro
- Resolver bugs em produção ou desenvolvimento
- Analisar logs de erro

## Quando NÃO Usar

- Problemas de performance (use prompts de performance)
- Melhorias de código sem erro (use code-improver)
- Implementar features novas (use feature-implementation)

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| Erro em produção | Priorize solução rápida + análise de impacto |
| Erro intermitente | Foque em condições de corrida e estado |
| Erro de integração | Foque em contratos de API e formatos |
| Memory leak | Foque em ciclo de vida de objetos |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{ERRO}}` | Mensagem de erro, stack trace, ou log | [stack trace completo] | Sim |
| `{{CODIGO_RELACIONADO}}` | Código onde o erro ocorre | [código fonte] | Não |
| `{{CONTEXTO}}` | Quando/como o erro ocorre | "Ao processar pagamentos acima de R$1000" | Não |
| `{{TENTATIVAS}}` | O que já foi tentado | "Já tentei aumentar timeout" | Não |

---

## Prompt

```text
Atue como um **Engenheiro de Software Senior** especialista em debugging e análise de causa raiz.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Use esse conhecimento para entender melhor onde o erro pode estar ocorrendo.
</contexto>

<raciocinio>
Conduza a análise de forma sistemática:

1. COMPREENSÃO DO ERRO
   - Qual é a mensagem de erro exata?
   - Qual é o tipo de exceção?
   - Onde exatamente o erro ocorre (linha, método, classe)?
   - Qual é a call stack completa?

2. ANÁLISE DE CONTEXTO
   - Quando o erro ocorre? (sempre, às vezes, condições específicas)
   - Quais dados estão envolvidos?
   - O que mudou recentemente? (deploy, dados, configuração)
   - O erro é reproduzível?

3. HIPÓTESES
   Formule múltiplas hipóteses ordenadas por probabilidade:

   - Hipótese 1: [mais provável]
     - Evidências a favor: [...]
     - Como verificar: [...]

   - Hipótese 2: [segunda mais provável]
     - Evidências a favor: [...]
     - Como verificar: [...]

   - Hipótese 3: [menos provável, mas possível]
     - Evidências a favor: [...]
     - Como verificar: [...]

4. CAUSA RAIZ
   - Qual é a causa raiz verdadeira (não apenas o sintoma)?
   - Por que o código permitiu que isso acontecesse?
   - Há outros lugares com o mesmo problema potencial?

5. PREVENÇÃO
   - Como evitar que isso aconteça novamente?
   - Que validações estão faltando?
   - Que testes deveriam existir?
</raciocinio>

<tarefa>
Analise e resolva o seguinte erro:

**Erro/Stack Trace:**
```
{{ERRO}}
```

**Código relacionado (se disponível):**
```
{{CODIGO_RELACIONADO}}
```

**Contexto de quando ocorre:**
{{CONTEXTO}}

**O que já foi tentado:**
{{TENTATIVAS}}
</tarefa>

<formato-de-resposta>
Estruture sua análise assim:

## Diagnóstico Rápido
[1-2 frases identificando o problema principal]

## Análise do Stack Trace
[Explicação linha a linha das partes relevantes do stack trace]

## Causa Raiz
**Problema**: [Descrição clara do que está causando o erro]
**Por que ocorre**: [Explicação técnica]
**Evidências**: [Dados do stack trace/código que suportam]

## Hipóteses Consideradas
| Hipótese | Probabilidade | Descartada? | Motivo |
|----------|--------------|-------------|--------|
| | | | |

## Solução

### Correção Imediata
```[linguagem]
[código corrigido]
```
**Justificativa**: [Por que essa correção resolve o problema, citando princípio se aplicável]

### Correção Completa (se diferente)
[Se a correção imediata é um band-aid, qual a solução ideal]

## Prevenção

### Validações a Adicionar
```[linguagem]
[código de validação]
```

### Testes a Criar
```[linguagem]
[teste que preveniria este bug]
```

### Melhorias de Logging
[Que logs ajudariam a diagnosticar isso mais rápido no futuro]

## Verificação
[Como confirmar que o fix funcionou]

## Outros Lugares para Verificar
[Outras partes do código que podem ter o mesmo problema]
</formato-de-resposta>

<justificativas>
Para a solução proposta:
1. Explique POR QUE essa é a solução correta (não apenas um band-aid)
2. Cite princípios de tratamento de erros ou padrões aplicáveis
3. Explique como a prevenção evitaria recorrência
</justificativas>
```

---

## Exemplo de Uso

### Input

```
{{ERRO}}:
System.NullReferenceException: Object reference not set to an instance of an object.
   at OrderService.ProcessOrder(Order order) in /src/Services/OrderService.cs:line 45
   at OrderController.Create(CreateOrderRequest request) in /src/Controllers/OrderController.cs:line 23
   at lambda_method(Closure, Object, Object[])

{{CODIGO_RELACIONADO}}:
public void ProcessOrder(Order order)
{
    var customer = _customerRepository.GetById(order.CustomerId);
    var discount = customer.LoyaltyProgram.DiscountPercentage;  // linha 45
    // ...
}

{{CONTEXTO}}:
Ocorre apenas para alguns clientes, não para todos.

{{TENTATIVAS}}:
Verificamos e o CustomerId está sempre preenchido.
```

### Output Esperado

```markdown
## Diagnóstico Rápido
NullReferenceException na linha 45 ao acessar `customer.LoyaltyProgram.DiscountPercentage`.
O problema é que `customer` ou `customer.LoyaltyProgram` está null.

## Causa Raiz
**Problema**: Acesso a propriedade de objeto que pode ser null sem verificação.
**Por que ocorre**: Clientes sem programa de fidelidade têm `LoyaltyProgram = null`.
**Evidências**: Erro só ocorre para "alguns clientes" - provavelmente os sem programa.

## Solução

### Correção Imediata
```csharp
public void ProcessOrder(Order order)
{
    var customer = _customerRepository.GetById(order.CustomerId)
        ?? throw new NotFoundException($"Customer {order.CustomerId} not found");

    var discount = customer.LoyaltyProgram?.DiscountPercentage ?? 0m;
    // ...
}
```
**Justificativa**: Utilizando null-conditional operator (?.) e null-coalescing (??)
para tratar de forma segura a ausência de LoyaltyProgram, seguindo o princípio
de defensive programming...
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Diagnóstico incorreto | Stack trace incompleto | Forneça stack trace completo |
| Solução superficial | Falta contexto | Explique quando/como ocorre |
| Muito genérico | Código não fornecido | Adicione o código relevante |

---

## Prompts Relacionados

- [../code-review/quality-reviewer.md](../code-review/quality-reviewer.md) - Revisar o fix
- [../testing/unit-test-generator.md](../testing/unit-test-generator.md) - Criar teste de regressão
