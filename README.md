# Kubernetes do Zero — PT-BR

**Fundamentos de Linux, Kubernetes e Platform Engineering, escritos por quem opera plataforma em produção.**

> **In English:** A Portuguese-language, hands-on track on Linux fundamentals, Kubernetes and Platform Engineering. The published chapters cover processes, networking, users and permissions — always ending in the Kubernetes behaviour they explain. Essays discuss platform decisions, reliability and observability. English articles: [dev.to/rogeroliveira86](https://dev.to/rogeroliveira86).

---

## A tese

Platform Engineering começa antes do Kubernetes. Pods, namespaces, limits e network policies são reembalagens de conceitos do Linux: processos, namespaces do kernel, cgroups, permissões e rede. Quem entende a camada de baixo investiga falhas por evidência, em vez de decorar comandos.

Por isso a trilha começa pelo sistema operacional e só depois sobe para o orquestrador:

**Linux → processos → redes → containers → Kubernetes → observabilidade → SRE → Platform Engineering**

## O que já está publicado

### Trilha — Linux para Platform Engineering

| # | Capítulo | O que você leva |
| --- | --- | --- |
| 01 | [Distribuições e terminal](./00-linux-for-platform-engineering/01-distros-e-terminal/README.md) | Como inspecionar um sistema Linux desconhecido |
| 02 | [Processos](./00-linux-for-platform-engineering/02-processos/README.md) | PID, PPID, estados e como investigar um processo |
| 03 | [Redes e troubleshooting](./00-linux-for-platform-engineering/04-redes-e-troubleshooting/README.md) | Rotas, DNS, portas, sockets e as causas reais de um `connection refused` |
| 04 | [Usuários e permissões](./00-linux-for-platform-engineering/03-usuarios-e-permissoes/README.md) | UID/GID, o `x` em diretório, `umask` e a tradução para `securityContext` |

Índice completo da trilha: [Linux para Platform Engineering](./00-linux-for-platform-engineering/README.md).

### Ensaios — decisões de plataforma

| Ensaio | Tema |
| --- | --- |
| [Platform Engineering começa antes do Kubernetes](./ENSAIOS.md) | Quem é o cliente da plataforma, como ela falha e como se mede se ela ajuda |
| [Observabilidade não é dashboard: é suporte à decisão](./artigos/ensaios/observabilidade-orientada-a-decisao.md) | Métricas, logs e traces como apoio para decidir o próximo passo seguro de uma mudança |
| [Migração em ondas com critérios de avanço e rollback](./artigos/ensaios/migracao-em-ondas-observabilidade-para-decidir-com-seguranca.md) | Quando avançar, pausar ou reverter uma mudança progressiva, e com quais sinais |

Os ensaios usam exemplos sanitizados: nenhum sistema, dado ou incidente real é identificável.

## Em construção

Os módulos de Kubernetes (workloads, arquitetura do cluster, networking, storage e troubleshooting) estão sendo escritos um de cada vez, sempre com laboratório reproduzível. O plano e a ordem estão no [ROADMAP](./ROADMAP.md); o ponto de partida é [Primeiros passos](./00-primeiros-passos/README.md).

## Como estudar

1. Leia o capítulo e execute os comandos numa VM ou ambiente descartável.
2. Anote o que observou antes de seguir para o próximo.
3. Compare com a documentação oficial.
4. Nunca rode comandos de alteração em produção sem entender o efeito e ter uma saída segura.

## Fontes principais

- [Documentação oficial do Kubernetes](https://kubernetes.io/docs/)
- [man-pages do Linux](https://man7.org/linux/man-pages/)

## Contribuições

Sugestões e correções são bem-vindas. Leia o [CONTRIBUTING.md](./CONTRIBUTING.md) antes de abrir uma issue ou pull request.

## Licença

[Apache License 2.0](./LICENSE).

## Autor

**Roger Oliveira** — Engenharia de Plataforma, SRE, Kubernetes e Observabilidade. Mais de 12 anos em infraestrutura, com liderança técnica de migrações de plataforma em ambiente regulado.

[LinkedIn](https://www.linkedin.com/in/oliveiraroger/) · [DEV.to (English)](https://dev.to/rogeroliveira86)
