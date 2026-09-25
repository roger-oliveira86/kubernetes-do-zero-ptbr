# Technical articles

This area collects writing on platform decisions, reliability, operations and observability.

The goal isn't to document real environments. Each essay uses sanitized examples, separates facts from interpretation, and cites primary sources when necessary.

## Essays published in this repository

| Essay | Topic |
| --- | --- |
| [Observability isn't a dashboard: it's decision support](./essays/observabilidade-orientada-a-decisao.md) | How metrics, logs and traces support decisions about change, impact and the next safe step. |
| [Wave migration with go/rollback criteria](./essays/migracao-em-ondas-observabilidade-para-decidir-com-seguranca.md) | Criteria for advancing, pausing or rolling back a progressive change. |
| [The order of the waves: what each stage needs to teach](./essays/ordem-das-ondas-o-que-cada-etapa-precisa-ensinar.md) | How to order changes in waves by the learning each stage generates for the next. |

## Editorial flow

1. The workflow creates a file in `articles/drafts/` on a branch and opens a Pull Request.
2. The text is reviewed for technical accuracy, clarity and sanitization.
3. Before the merge, the approved file is promoted to `articles/essays/` and gets the `publicado-no-repositorio` status.
4. An adaptation for LinkedIn, DEV.to or Medium only happens by explicit decision; merging on GitHub doesn't publish it on another platform.

This way, the repository keeps technical evidence without turning drafts into automatic publication.
