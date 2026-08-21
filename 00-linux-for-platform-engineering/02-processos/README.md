# 02 — Processos

> Trilha: Linux → **processos** → redes → containers → Docker → containerd → Kubernetes → observabilidade → SRE → Platform Engineering → arquitetura.

## Objetivo deste capítulo

Entender o que um processo realmente é no Linux: identidade (PID), hierarquia (PPID), estado (STAT) e tempo de vida (ETIME) — a base conceitual para os capítulos de namespaces e containers, mais à frente.

## Experimento guiado

1. Crie um processo de teste:
   ```bash
   sleep 300 &
   ```
2. Inspecione-o:
   ```bash
   ps -eo pid,ppid,stat,etime,cmd | grep sleep
   ```
3. Leia a hierarquia completa do sistema:
   ```bash
   ps -eo pid,ppid,stat,etime,cmd --forest
   ```

## Referência rápida — campos do `ps`

| Campo | O que significa | Por que importa em produção |
|---|---|---|
| `PID` | Identificador único do processo | Base para qualquer ação de diagnóstico ou kill |
| `PPID` | PID do processo pai | Explica reparentamento e por que "matar o pai" derruba os filhos |
| `STAT` | Estado atual (`R`, `S`, `D`, `Z`, ...) | `D` prolongado costuma indicar gargalo de I/O/disco, não bug de app |
| `ETIME` | Tempo de vida do processo | Ajuda a distinguir processo recém-criado de processo travado há horas |

## Exercícios

- [ ] Rode `ps -eo pid,ppid,stat,etime,cmd --forest` e identifique 3 relações pai/filho no seu sistema.
- [ ] Encontre (ou não encontre) um processo `Z` — registre o resultado.
- [ ] Compare a saída estática de `ps aux` com a visão contínua de `top`/`htop`.

## Por que este capítulo importa para Kubernetes

Namespaces do kernel Linux (o assunto do próximo capítulo) isolam exatamente essas mesmas árvores de processo. Um Pod do Kubernetes, por baixo, é um ou mais processos Linux isolados por namespace — entender processo aqui é o que faz `kubectl describe pod` fazer sentido lá na frente.

## Artigo completo

Leia a versão narrativa completa deste capítulo em [`ARTIGO-linux-do-zero-02.md`](./ARTIGO-linux-do-zero-02.md), também publicada no [Medium](https://medium.com/@rogeroliveira86) e no [DEV.to](https://dev.to/rogeroliveira86).

## Próximo capítulo

`03-namespaces` — o que realmente isola um processo do outro.
