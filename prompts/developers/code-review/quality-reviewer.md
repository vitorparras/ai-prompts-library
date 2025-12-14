# Revisor de Qualidade de Código

> **Categoria**: developers/code-review
> **Versão**: 1.0.0
> **Uso diário recomendado**

---

## Quando Usar

- Revisar Pull Requests antes de merge
- Avaliar código próprio antes de submeter
- Revisar código legado para modernização
- Garantir aderência a padrões de qualidade

## Quando NÃO Usar

- Análise específica de segurança (use `security-reviewer`)
- Análise específica de performance (use `performance-reviewer`)
- Debugging de erros (use `error-analyzer`)

## Como Adaptar

| Contexto | Adaptação |
|----------|-----------|
| PR pequeno | Foco em detalhes específicos |
| PR grande | Foco em arquitetura e design |
| Código crítico | Rigor máximo |
| Código experimental | Rigor moderado, foco em direção |

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{CODIGO}}` | O código a ser revisado | [código fonte] | Sim |
| `{{CONTEXTO_ADICIONAL}}` | Informações extras sobre o código | "Parte de migração de monolito" | Não |
| `{{FOCO}}` | Área específica de foco | "Tratamento de erros" | Não |

---

## Prompt

```text
Atue como um **Tech Lead Senior** conduzindo um code review rigoroso com mentalidade de melhoria contínua.

<contexto>
Você já possui conhecimento sobre o projeto através do contexto fornecido anteriormente nesta conversa.
Use esse conhecimento para avaliar se o código segue os padrões estabelecidos.
</contexto>

<raciocinio>
Conduza a revisão de forma sistemática:

1. VISÃO GERAL
   - Qual o propósito deste código?
   - Como ele se encaixa na arquitetura existente?
   - Está no lugar certo (camada/módulo correto)?

2. ANÁLISE SOLID
   - SRP: Cada classe/função tem uma única responsabilidade?
   - OCP: O código é extensível sem modificação?
   - LSP: Herança está correta (se aplicável)?
   - ISP: Interfaces são coesas?
   - DIP: Dependências são abstrações?

3. ANÁLISE CLEAN CODE
   - Nomenclatura é clara e expressiva?
   - Funções são pequenas e focadas?
   - Há duplicação de código?
   - Comentários são necessários e úteis?

4. ANÁLISE DE PADRÕES
   - Segue os design patterns do projeto?
   - Há anti-patterns presentes?
   - Convenções de nomenclatura estão corretas?

5. ANÁLISE DE ROBUSTEZ
   - Erros são tratados adequadamente?
   - Inputs são validados?
   - Edge cases são considerados?
   - Logging é apropriado?

6. TESTABILIDADE
   - O código é fácil de testar?
   - Dependências podem ser mockadas?
   - Estado é gerenciado corretamente?
</raciocinio>

<tarefa>
Revise o seguinte código com foco em qualidade:

```
{{CODIGO}}
```

Contexto adicional:
{{CONTEXTO_ADICIONAL}}

Foco específico (se houver):
{{FOCO}}
</tarefa>

<criterios-de-avaliacao>
Avalie cada aspecto como:
- ✅ APROVADO: Segue o padrão corretamente
- ⚠️ SUGESTÃO: Funciona, mas pode melhorar
- ❌ PROBLEMA: Precisa ser corrigido antes do merge

Critérios obrigatórios (❌ se violado):
- Violações de SOLID
- Vulnerabilidades de segurança óbvias
- Problemas de performance sérios
- Código não testável
- Violação de arquitetura do projeto
</criterios-de-avaliacao>

<formato-de-resposta>
Estruture sua revisão assim:

## Resumo Executivo
[1-3 frases sobre a qualidade geral e decisão: APROVAR / APROVAR COM SUGESTÕES / REPROVAR]

## Pontos Positivos
[O que está bem feito - importante para feedback construtivo]

## Problemas (devem ser corrigidos)
Para cada problema:
### ❌ [Título do Problema]
- **Localização**: [arquivo:linha ou bloco de código]
- **Problema**: [Descrição clara]
- **Princípio violado**: [SOLID/Clean Code/Pattern]
- **Impacto**: [Por que isso é um problema]
- **Correção sugerida**:
```[linguagem]
[código corrigido]
```
- **Justificativa**: [Por que essa correção é melhor, citando princípio]

## Sugestões (melhorias opcionais)
Para cada sugestão:
### ⚠️ [Título da Sugestão]
- **Localização**: [arquivo:linha]
- **Situação atual**: [O que está]
- **Melhoria proposta**: [O que poderia ser]
- **Benefício**: [Por que seria melhor]

## Perguntas
[Perguntas sobre decisões que precisam de esclarecimento]

## Checklist Final
- [ ] SOLID principles respeitados
- [ ] Clean Code seguido
- [ ] Padrões do projeto respeitados
- [ ] Erros tratados adequadamente
- [ ] Inputs validados
- [ ] Código testável
- [ ] Logging apropriado

## Veredicto
[APROVAR / APROVAR COM SUGESTÕES / REPROVAR - com justificativa]
</formato-de-resposta>

<justificativas>
OBRIGATÓRIO: Para CADA problema ou sugestão:
1. Cite o princípio, padrão ou best practice violado/sugerido
2. Explique o IMPACTO de manter o código como está
3. Explique o BENEFÍCIO da correção/sugestão

Seja construtivo: o objetivo é melhorar o código, não criticar o autor.
</justificativas>
```

---

## Exemplo de Uso

### Input

```csharp
{{CODIGO}}:
public class OrderService
{
    private SqlConnection _connection = new SqlConnection("Server=prod;Password=123");

    public void ProcessOrder(Order order)
    {
        // Validate
        if (order == null) throw new Exception("Order is null");

        // Calculate total
        decimal total = 0;
        foreach (var item in order.Items)
        {
            total += item.Price * item.Quantity;
            if (item.HasDiscount)
                total -= item.Price * item.Quantity * 0.1m;
        }

        // Save to database
        _connection.Open();
        var cmd = new SqlCommand($"INSERT INTO Orders VALUES ('{order.Id}', {total})", _connection);
        cmd.ExecuteNonQuery();
        _connection.Close();

        // Send email
        var smtp = new SmtpClient();
        smtp.Send("orders@company.com", order.CustomerEmail, "Order Confirmed", $"Your order {order.Id} total: {total}");
    }
}
```

### Output Esperado

```markdown
## Resumo Executivo
**REPROVAR** - O código apresenta problemas críticos de segurança (SQL Injection,
senha hardcoded), viola SRP (faz 4 coisas), e não trata erros adequadamente.

## Problemas

### ❌ SQL Injection
- **Localização**: linha 18
- **Problema**: Concatenação direta de variáveis na query SQL
- **Princípio violado**: OWASP A03 - Injection
- **Impacto**: Atacante pode executar SQL arbitrário no banco
- **Correção sugerida**:
```csharp
var cmd = new SqlCommand("INSERT INTO Orders VALUES (@Id, @Total)", _connection);
cmd.Parameters.AddWithValue("@Id", order.Id);
cmd.Parameters.AddWithValue("@Total", total);
```
- **Justificativa**: Queries parametrizadas previnem SQL Injection...

### ❌ Credenciais Hardcoded
...
```

---

## Troubleshooting

| Se o resultado... | Possível Causa | Tente... |
|-------------------|----------------|----------|
| Muito superficial | Código muito grande | Divida em partes menores |
| Ignora padrões do projeto | Contexto não estabelecido | Rode context-extractor primeiro |
| Muito rigoroso | Código experimental | Indique que é protótipo |
| Falta justificativa | IA ignorou instrução | Reforce obrigatoriedade |

---

## Prompts Relacionados

- [security-reviewer.md](security-reviewer.md) - Para análise focada em segurança
- [performance-reviewer.md](performance-reviewer.md) - Para análise focada em performance
- [../refactoring/code-improver.md](../refactoring/code-improver.md) - Para melhorar o código
