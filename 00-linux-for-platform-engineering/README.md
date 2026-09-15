# Linux para Platform Engineering

Esta trilha constrói os fundamentos que reaparecem em containers, Kubernetes, observabilidade e troubleshooting.

A ordem conceitual é: **Linux → processos → redes → containers → Kubernetes → observabilidade → SRE → Platform Engineering**. Os nomes das pastas preservam a evolução do material; use este índice como ponto de entrada.

| Peça | Assunto | Formato | Situação |
| --- | --- | --- | --- |
| [01 — Distribuições e terminal](./01-distros-e-terminal/README.md) | Ambiente Linux e inspeção inicial do sistema | Guia + comandos | Publicado |
| [02 — Processos](./02-processos/README.md) | PID, PPID, estados e investigação | Guia + experimento | Publicado |
| [03 — Redes e troubleshooting](./04-redes-e-troubleshooting/README.md) | Rotas, DNS, portas e sockets | Laboratório guiado | Publicado |
| [04 — Usuários e permissões](./03-usuarios-e-permissoes/README.md) | UID, GID, permissões, ACLs e `securityContext` | Guia + exercício | Publicado |

## Ensaios

[Ensaios — Linux para Platform Engineering](./ENSAIOS.md) reúne análises curtas que conectam esses fundamentos a decisões de plataforma.

## Como usar

1. Execute os comandos em uma VM ou ambiente de laboratório descartável.
2. Registre o que observou antes de passar para a próxima peça.
3. Não reproduza comandos de alteração em sistemas produtivos sem entender o efeito e possuir uma saída segura.

Os exemplos são educacionais. Confirme comportamento, versão e permissões no ambiente em que forem executados.
