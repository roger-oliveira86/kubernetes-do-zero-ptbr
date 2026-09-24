# Artigos técnicos

Esta área reúne textos sobre decisões de plataforma, confiabilidade, operação e observabilidade.

O objetivo não é documentar ambientes reais. Cada ensaio usa exemplos sanitizados, separa fatos de interpretações e cita fontes primárias quando necessário.

## Ensaios publicados no repositório

| Ensaio | Tema |
| --- | --- |
| [Observabilidade não é dashboard: é suporte à decisão](./ensaios/observabilidade-orientada-a-decisao.md) | Como métricas, logs e traces sustentam decisões sobre mudança, impacto e próximo passo seguro. |
| [Migração em ondas com critérios de avanço e rollback](./ensaios/migracao-em-ondas-observabilidade-para-decidir-com-seguranca.md) | Critérios para avançar, pausar ou reverter uma mudança progressiva. |
| [A ordem das ondas: o que cada etapa precisa ensinar](./ensaios/ordem-das-ondas-o-que-cada-etapa-precisa-ensinar.md) | Como ordenar mudanças em ondas pelo aprendizado que cada etapa gera para a próxima. |

## Fluxo editorial

1. O workflow cria um arquivo em `artigos/rascunhos/` numa branch e abre um Pull Request.
2. O texto é revisado quanto a precisão técnica, clareza e sanitização.
3. Antes do merge, o arquivo aprovado é promovido para `artigos/ensaios/` e recebe o status `publicado-no-repositorio`.
4. Uma adaptação para LinkedIn, DEV.to ou Medium só ocorre por decisão explícita; o merge no GitHub não publica em outra plataforma.

Assim, o repositório mantém evidência técnica sem transformar rascunhos em publicação automática.
