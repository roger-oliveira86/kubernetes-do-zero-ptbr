# 04 — Redes, DNS, Portas e Sockets

## Objetivo
Diferenciar, com evidência, as três causas mais comuns de falha de conectividade:
falha de DNS, porta fechada e bloqueio por firewall. Ver o artigo completo no
Medium para o experimento passo a passo com Docker.

## Pré-requisitos
- Módulo 03 (Processos, sinais e systemd)
- Docker ou Podman instalado

## Modelo mental


Cada seta é um ponto de falha isolado. Cada ponto tem uma ferramenta que testa
*só ele*, sem misturar variáveis.

## Tabela de referência — sintoma → causa → evidência

| Sintoma observado | Causa provável | Comando de evidência |
|---|---|---|
| Nome não resolve / resolve errado | DNS | `nslookup <nome>` + testar por IP direto |
| `Connection refused` imediato | Porta fechada (processo não escuta) | `ss -tlnp` no destino |
| Timeout silencioso, sem erro | Firewall com `DROP` | `tcpdump -i <iface> -n port <porta>` |
| Timeout com RST de volta | Firewall com `REJECT` ou porta fechada no SO | RST visível no `tcpdump` |

## Comandos essenciais (referência rápida)

| Comando | Camada testada | O que confirma |
|---|---|---|
| `nslookup` / `dig` | DNS | Se o nome resolve e para qual IP |
| `ping` | Rede/ICMP | Se há rota — **não confirma porta TCP aberta** |
| `ss -tlnp` | Processo/porta | Se algo está escutando localmente |
| `curl -v` / `wget -O-` | Aplicação | Resposta HTTP fim a fim |
| `nc -zv <host> <porta>` | Porta TCP | Se a porta aceita conexão, sem depender de protocolo de app |
| `iptables -L -n -v` / `nft list ruleset` | Filtro | Regras ativas de firewall |
| `tcpdump -i <iface> -n` | Pacote | Se o pacote saiu e voltou, e como |

## Laboratório reproduzível

Ver o experimento completo (3 cenários: DNS quebrado, porta fechada, firewall
bloqueando) no artigo do Medium: [link do artigo].

Resumo dos comandos para reproduzir localmente:

```bash
docker network create rede-lab
docker run -d --name servidor --network rede-lab nginx:alpine
docker run -it --name cliente --network rede-lab busybox sh
```

## Checklist de exercícios

- [ ] Reproduzir o Cenário A (DNS quebrado) e confirmar via acesso por IP direto.
- [ ] Reproduzir o Cenário B (porta fechada) e confirmar via `ss -tlnp` no servidor.
- [ ] Reproduzir o Cenário C (firewall com DROP) e confirmar via `tcpdump`.
- [ ] Repetir o Cenário C trocando `DROP` por `REJECT` e comparar o sintoma (timeout vs RST imediato).
- [ ] Preencher a tabela de referência de memória, sem consultar o README, e comparar com o original.
- [ ] (Avançado) Reproduzir os três cenários dentro de um cluster Kubernetes local (kind/minikube), mapeando: DNS quebrado → CoreDNS mal configurado; porta fechada → Service sem Endpoints; firewall → NetworkPolicy bloqueando.

## Próximo módulo
`05-containers-under-the-hood/` — como namespaces de rede isolam containers e
por que isso explica os três sintomas deste capítulo em um contexto de container.
