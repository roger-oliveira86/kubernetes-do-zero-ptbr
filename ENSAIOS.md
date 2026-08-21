# Ensaios

Textos de opinião e reflexão sobre carreira, Platform Engineering e engenharia de infraestrutura — conectados com a trilha técnica deste repositório, mas fora da numeração de módulos (00–05), que segue o roadmap de preparação para a CKA.

---

## Platform Engineering começa antes do Kubernetes

*Por que a pergunta certa não é "por onde eu começo o Kubernetes", e sim "qual problema estou resolvendo".*

### A afirmação que costuma incomodar

Toda vez que alguém me pergunta "por onde eu começo para virar Platform Engineer", a resposta que a pessoa espera é "Kubernetes". A resposta que eu dou é: não. Kubernetes é a ferramenta que você usa depois de já saber o que está tentando resolver. Platform Engineering é sobre o problema, não sobre o orquestrador.

### O que realmente separa quem "sabe Kubernetes" de quem faz Platform Engineering

Depois de anos administrando e evoluindo plataformas Kubernetes em produção — com centenas de workloads e milhares de pods rodando em ambiente corporativo regulado —, a diferença que eu vejo na prática não está no `kubectl`. Está em três perguntas que a maioria dos tutoriais nunca faz você se perguntar:

1. **Quem é o cliente da sua plataforma?** Não é o cluster. É o time de desenvolvimento que precisa fazer deploy sem te chamar toda vez. Se você não sabe nomear quem usa o que você constrói, você está operando infraestrutura, não construindo plataforma.
2. **O que acontece quando algo quebra às 3h da manhã?** Rollback testado, RTO definido, dono do incidente claro — isso é decisão de design, tomada muito antes de qualquer manifest YAML existir. Plataforma que não foi desenhada para falhar bem, falha mal.
3. **Como você mede se a plataforma está ajudando ou atrapalhando?** Se a resposta for "não sei", você tem um cluster, não uma plataforma. Capacity planning, indicadores de adoção, redução de retrabalho entre times — isso é o que transforma operação em produto interno.

### Onde o Linux entra nisso

Essa é a conexão que a trilha "Linux do Zero" está construindo desde o primeiro post: cada abstração que o Kubernetes te oferece — pods, namespaces, resource limits, network policies — é uma reembalagem elegante de conceitos que já existem no Linux puro: processos, namespaces do kernel, cgroups, iptables/nftables. Quando você entende o processo antes do pod, o namespace do kernel antes do namespace do Kubernetes, você para de tratar o cluster como uma caixa-preta mágica e começa a debugar como quem entende o motor, não só o painel.

Não é coincidência que os piores incidentes em produção que já resolvi tinham a causa raiz em uma camada abaixo do Kubernetes — I/O de disco, DNS, limites de kernel — e não no orquestrador em si. Quem só sabe operar o Kubernetes fica cego exatamente onde o problema mora.

### O caminho que eu defendo (e que essa trilha segue)

Linux → processos → redes → containers → Docker → containerd → Kubernetes → observabilidade → SRE → Platform Engineering → arquitetura.

Não é ordem por acaso. É a ordem que te dá terreno sólido antes de te dar altura. Cada camada explica a de cima. Pular direto para "Kubernetes do zero" é como aprender a pilotar avião sem entender o que faz ele voar — você consegue decorar os comandos dos controles, mas não consegue diagnosticar quando algo sai do script.

### Onde isso te leva na prática

Se você está tentando evoluir de operação para Platform Engineering — ou de Platform Engineer para Staff/Principal — o teste real não é "quantos manifests YAML você já escreveu". É: você consegue explicar, em uma frase, para um executivo não técnico, por que a plataforma que você constrói reduz risco e acelera entrega? Se sim, você já está pensando como Platform Engineer. Kubernetes é só a ferramenta que você escolheu para executar essa visão hoje — amanhã pode ser outra.

---

*Versão completa também publicada no [Medium](https://medium.com/@rogeroliveira86) e no [DEV.to](https://dev.to/rogeroliveira86). Trilha técnica: veja [`00-linux-for-platform-engineering`](./00-linux-for-platform-engineering) em diante.*
