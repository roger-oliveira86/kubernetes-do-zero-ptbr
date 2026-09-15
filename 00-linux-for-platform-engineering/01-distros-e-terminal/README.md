# Módulo 01 — Distribuições Linux e Terminal

Este módulo apresenta Linux para quem está começando em infraestrutura, DevOps, SRE e Kubernetes.

## Objetivos

- Entender o que é Linux e o que é uma distribuição.
- Escolher uma distribuição adequada para começar.
- Criar uma máquina virtual Ubuntu.
- Usar os primeiros comandos do terminal.
- Observar o estado do sistema antes de fazer alterações.

## Distribuição recomendada

Comece com Ubuntu LTS em uma máquina virtual.

Depois avance para:

- Debian Stable;
- Fedora/Rocky Linux;
- Alpine Linux em containers.

## Primeiro laboratório

```bash
pwd
ls -la
whoami
uname -a
cat /etc/os-release
df -h
free -h
```

Antes de alterar qualquer configuração, responda: em qual diretório estou, qual usuário executará o comando e qual é o estado atual do sistema? Esse hábito reduz mudanças por tentativa e erro.

## Próximo passo

Siga para [Processos](../02-processos/README.md) e observe como programas aparecem, vivem e terminam no Linux.
