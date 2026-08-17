# Linux do Zero para Platform Engineering

Esta trilha prepara quem está começando em infraestrutura, DevOps, SRE e Kubernetes.

O objetivo não é decorar comandos. É aprender a investigar o sistema por evidências: processos, permissões, serviços, rede e logs.

## Para quem é

- Quem está começando em Linux e quer entrar em infraestrutura.
- Quem estuda Docker ou Kubernetes, mas ainda trava nos fundamentos.
- Quem quer construir uma base prática para Platform Engineering e SRE.

## Jornada

```text
Linux básico → Containers → containerd/kubeadm → Kubernetes → Platform Engineering
```

## Módulos

| Módulo | Tema | Resultado esperado |
|---|---|---|
| 01 | Distribuições e terminal | Criar um ambiente seguro e usar o shell |
| 02 | Arquivos e permissões | Organizar arquivos e aplicar menor privilégio |
| 03 | Processos e serviços | Investigar processos, systemd e serviços |
| 04 | Redes e troubleshooting | Diagnosticar DNS, portas, rotas e conectividade |
| 05 | Logs e evidências | Encontrar causas em logs reais |
| 06 | Bash e automação | Automatizar verificações operacionais |
| 07 | SSH e administração remota | Operar uma máquina remotamente com segurança |
| 08 | Containers por baixo | Relacionar Linux, Docker, namespaces e cgroups |

## Distribuição sugerida

Comece com Ubuntu LTS em uma máquina virtual. Depois avance para Debian Stable e, quando os fundamentos estiverem consolidados, explore Fedora/Rocky para conhecer o ecossistema empresarial e SELinux.

## Laboratórios

Cada módulo deve conter:

1. um cenário;
2. uma falha ou objetivo;
3. comandos de investigação;
4. evidências coletadas;
5. correção;
6. validação;
7. registro da causa raiz.

## Primeiro laboratório

### Objetivo

Preparar uma VM Ubuntu, atualizar o sistema, criar um usuário de laboratório e executar o kit inicial do terminal.

### Comandos iniciais

```bash
pwd
ls -la
whoami
uname -a
cat /etc/os-release
df -h
free -h
```

### Perguntas de investigação

- Qual distribuição e versão estão instaladas?
- Qual usuário está executando os comandos?
- Quanto espaço e memória estão disponíveis?
- Onde ficam os arquivos de configuração do sistema?

## Conexão com Kubernetes

Ao finalizar esta trilha, o aluno estará preparado para entender:

- por que o kubelet é um serviço do sistema;
- como investigar logs com `journalctl`;
- como portas e DNS afetam o cluster;
- por que permissões quebram workloads;
- como containers usam processos, namespaces e cgroups.

> A base de um bom Platform Engineer não é saber todos os comandos. É saber formular hipóteses, coletar evidências e validar a causa da falha.
