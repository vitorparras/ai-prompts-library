# Meetings & Communication - Prompts para Reuniões e Comunicação

> Prompts para transformar reuniões em artefatos acionáveis e comunicação profissional.

---

## Categorias

| Categoria | Descrição | Prompts |
|-----------|-----------|---------|
| [transcription-summary/](transcription-summary/) | Resumir reuniões | 1 |
| [action-items-extractor/](action-items-extractor/) | Extrair ações | 1 |
| [user-stories-generator/](user-stories-generator/) | Criar user stories | 1 |
| [professional-emails/](professional-emails/) | Redigir emails | 1 |
| [status-reports/](status-reports/) | Criar relatórios | 1 |

---

## Prompts Mais Usados

| Prompt | Caso de Uso | Link |
|--------|-------------|------|
| Discussion to Stories | Transformar discussão em user stories | [user-stories-generator/discussion-to-stories.md](user-stories-generator/discussion-to-stories.md) |
| Meeting Summarizer | Resumir reunião | [transcription-summary/meeting-summarizer.md](transcription-summary/meeting-summarizer.md) |
| Action Extractor | Extrair action items | [action-items-extractor/action-extractor.md](action-items-extractor/action-extractor.md) |

---

## Fluxo Pós-Reunião

```
┌───────────────────────┐
│ Transcrição da Reunião │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│ Meeting Summarizer     │ ─────> Resumo executivo
└──────────┬────────────┘
           │
           ├──────────────────────┐
           │                      │
           ▼                      ▼
┌───────────────────────┐  ┌───────────────────────┐
│ Action Extractor       │  │ Discussion to Stories  │
│ (O que fazer)          │  │ (O que desenvolver)    │
└───────────────────────┘  └───────────────────────┘
           │                      │
           ▼                      ▼
      [Task List]           [User Stories]
```

---

## Lista Completa de Prompts

### Transcription Summary
- [meeting-summarizer.md](transcription-summary/meeting-summarizer.md) - Resumir reuniões

### Action Items
- [action-extractor.md](action-items-extractor/action-extractor.md) - Extrair ações de reuniões

### User Stories
- [discussion-to-stories.md](user-stories-generator/discussion-to-stories.md) - Criar user stories de discussões

### Professional Emails
- [email-composer.md](professional-emails/email-composer.md) - Redigir emails profissionais

### Status Reports
- [report-generator.md](status-reports/report-generator.md) - Criar relatórios de status
