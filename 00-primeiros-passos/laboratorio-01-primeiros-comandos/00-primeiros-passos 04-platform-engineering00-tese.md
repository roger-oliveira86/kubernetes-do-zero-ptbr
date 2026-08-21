# Platform Engineering começa antes do Kubernetes

> Nota de tese desta trilha, referenciada a partir da seção "Platform Engineering".

Este repositório segue deliberadamente a ordem:
Linux → processos → redes → containers → Docker → containerd → Kubernetes → observabilidade → SRE → Platform Engineering → arquitetura.

Isso não é enrolação — é a tese central da trilha: Platform Engineering se define pelo problema que resolve (reduzir risco, acelerar entrega, dar autonomia ao time de desenvolvimento), não pela ferramenta usada para resolvê-lo. Quem entende Linux, processo e rede antes de abrir o primeiro manifest YAML debuga Kubernetes como quem entende o motor — não só o painel.

Três perguntas que valem mais do que saber `kubectl`:
1. Quem é o cliente real da plataforma?
2. O que acontece quando algo quebra às 3h da manhã (rollback, RTO, dono do incidente)?
3. Como você mede se a plataforma ajuda ou atrapalha?

Post completo (Medium): [[link](https://medium.com/@rogeroliveira86/platform-engineering-come%C3%A7a-antes-do-kubernetes-0c0f353c7f82?postPublishedType=initial)]
