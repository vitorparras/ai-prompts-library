# Compositor de Emails Profissionais

> **Categoria**: meetings-communication/professional-emails
> **Versão**: 1.0.0

---

## Quando Usar

- Redigir comunicações profissionais
- Escalar problemas para gestão
- Alinhar com stakeholders
- Comunicar decisões técnicas

---

## Variáveis

| Variável | Descrição | Exemplo | Obrigatória |
|----------|-----------|---------|-------------|
| `{{OBJETIVO}}` | O que deseja comunicar | "Solicitar extensão de prazo" | Sim |
| `{{CONTEXTO}}` | Informações relevantes | "Projeto atrasou por dependência externa" | Sim |
| `{{DESTINATARIO}}` | Para quem | "Gerente de projeto" | Não |
| `{{TOM}}` | Tom desejado | "Formal", "Assertivo", "Diplomático" | Não |

---

## Prompt

```text
Atue como um **Comunicador Corporativo Senior** especializado em comunicação profissional eficaz.

<raciocinio>
Antes de redigir, considere:

1. OBJETIVO
   - O que preciso que o destinatário faça/entenda?
   - Qual a call-to-action?

2. AUDIÊNCIA
   - Quem é o destinatário?
   - Qual seu nível de conhecimento técnico?
   - Qual a relação hierárquica?

3. TOM
   - Qual tom é apropriado?
   - Há sensibilidades a considerar?

4. ESTRUTURA
   - Informação mais importante primeiro
   - Contexto suficiente mas conciso
   - Call-to-action claro
</raciocinio>

<tarefa>
Redija um email profissional com:

**Objetivo:** {{OBJETIVO}}

**Contexto:** {{CONTEXTO}}

**Destinatário:** {{DESTINATARIO}}

**Tom:** {{TOM}}
</tarefa>

<formato-de-resposta>
## Opção 1: [Tom principal]

**Assunto:** [Linha de assunto clara e direta]

---

[Saudação]

[Parágrafo 1: Contexto breve - por que estou escrevendo]

[Parágrafo 2: Informação principal - o que preciso comunicar]

[Parágrafo 3: Call-to-action - o que espero do destinatário]

[Parágrafo 4 (opcional): Próximos passos ou oferta de ajuda]

[Fechamento]
[Nome]

---

## Opção 2: [Tom alternativo]
[Versão alternativa se aplicável]

---

## Dicas de Envio
- Melhor horário para enviar
- CC sugeridos
- Attachments necessários
</formato-de-resposta>
```

---

## Prompts Relacionados

- [../status-reports/report-generator.md](../status-reports/report-generator.md) - Para reports de status
