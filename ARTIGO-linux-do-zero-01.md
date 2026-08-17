# Linux do Zero #1: o que é Linux e qual distribuição escolher?

Quando alguém começa a estudar DevOps, SRE ou Kubernetes, normalmente encontra uma lista de ferramentas: Docker, Terraform, Kubernetes, Helm, ArgoCD e muitas outras.

Mas existe uma camada anterior a tudo isso: o sistema operacional.

Se você não entende processos, permissões, serviços, rede e logs, qualquer erro em um container ou em um cluster parece misterioso.

## Linux não é apenas o terminal

Linux é o núcleo que coordena recursos como CPU, memória, armazenamento, rede e processos. Uma distribuição Linux reúne esse núcleo com ferramentas, bibliotecas, gerenciadores de pacotes e uma forma de instalação.

Por isso existem distribuições diferentes. Ubuntu, Debian, Fedora, Rocky Linux e Alpine usam Linux, mas possuem escolhas diferentes para gerenciamento, atualizações, segurança e objetivos de uso.

## Qual distribuição escolher?

### Ubuntu LTS

É minha recomendação para o primeiro laboratório. Possui grande comunidade, bastante material didático e é muito comum em ambientes cloud.

Use para:

- aprender Linux;
- criar a primeira máquina virtual;
- estudar Docker e Kubernetes;
- montar laboratórios locais.

### Debian Stable

É uma excelente escolha para quem quer entender uma base estável de servidor e o ecossistema Debian de forma mais direta.

Use depois do primeiro contato, quando você quiser comparar instalação, pacotes e administração de um sistema mais conservador.

### Fedora, Rocky Linux e RHEL

Essas distribuições aproximam o aluno do ecossistema Red Hat, bastante presente em empresas. Fedora costuma trazer tecnologias mais recentes; Rocky Linux e RHEL são mais associados a ambientes corporativos.

Um tema importante nessa trilha é o SELinux, que mostra que segurança não deve ser resolvida simplesmente desativando controles.

### Alpine Linux

Alpine é muito útil para entender imagens pequenas e containers minimalistas. Porém, não é minha recomendação para começar Linux do zero.

Primeiro aprenda processos, arquivos, permissões, rede e logs em uma distribuição mais completa. Depois compare o que muda em uma imagem Alpine.

## Minha recomendação prática

Siga esta ordem:

```text
Ubuntu LTS → Debian Stable → Fedora/Rocky → Alpine em containers
```

Não precisa substituir seu computador principal. Comece em uma máquina virtual, faça snapshots e trate cada erro como parte do laboratório.

## O primeiro exercício

Depois de iniciar uma VM Ubuntu, execute:

```bash
pwd
ls -la
whoami
uname -a
cat /etc/os-release
df -h
free -h
```

Agora responda:

1. Em qual diretório você está?
2. Qual usuário está executando os comandos?
3. Qual distribuição e versão estão instaladas?
4. Quanto espaço em disco está disponível?
5. Quanto de memória o sistema possui?

Parece simples, mas essa prática já começa a formar a mentalidade correta: antes de alterar o sistema, observe o estado atual.

## A mentalidade de Platform Engineering

O caminho tradicional é decorar um comando para cada erro.

O caminho profissional é investigar:

- o processo está em execução?
- o serviço está ativo?
- a porta está aberta?
- o DNS resolve?
- o usuário tem permissão?
- o log mostra qual causa?

Essa mentalidade será essencial quando chegarmos a Docker, containerd, kubelet e Kubernetes.

## Próximo passo

No próximo artigo, vamos explorar o terminal e os comandos que formam o kit diário de quem trabalha com infraestrutura:

- `pwd`;
- `ls`;
- `cd`;
- `grep`;
- `ps`;
- `ss`;
- `journalctl`.

O objetivo não é decorar uma lista. É saber qual pergunta cada comando ajuda a responder.

> Qual distribuição Linux você usou no seu primeiro servidor ou laboratório?

## Referências oficiais

- Ubuntu Desktop Guide: https://help.ubuntu.com/
- Fedora Beginner’s Guide: https://docs.fedoraproject.org/en-US/beginners-guide/
- Debian Installation Guide: https://www.debian.org/releases/stable/amd64/
