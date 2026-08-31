# 04 — Redes e Troubleshooting

> Parte da trilha **Linux do Zero** (peça #3: *Redes, DNS, portas e sockets*).
> Artigos completos: [Medium](https://medium.com/@rogeroliveira86/linux-do-zero-515b4c682a91) · [DEV.to](https://dev.to/rogeroliveira86/linux-from-zero-networking-dns-ports-and-sockets-for-platform-engineers-240h)

## Objetivo

Praticar, em uma VM ou container Linux, os comandos e o raciocínio usados para investigar problemas de rede, DNS e conectividade — a base que reaparece, quase sem tradução, dentro de Kubernetes (`Service`, `CoreDNS`, `Ingress`).

Pré-requisito: terminal básico e permissões (peças #1 e #2 desta trilha).

## Estrutura deste laboratório

```text
04-redes-e-troubleshooting/
├── README.md                  # este arquivo
├── 01-interfaces-e-rotas.md   # ip a, ip route
├── 02-dns.md                  # dig, resolvectl, /etc/resolv.conf
├── 03-portas-e-sockets.md     # ss, LISTEN vs ESTABLISHED
└── 04-checklist-troubleshooting.md
```

## 1. Interfaces e rotas

```bash
ip a
ip route
ip route get 8.8.8.8
```

**Exercício:** rode `ip a` na sua máquina e identifique: qual interface tem a rota padrão, e qual endereço IP está associado a ela. Se você tiver Docker instalado, rode `ip a` de novo e note a interface nova (`docker0` ou `br-...`) que aparece.

**O que observar:** a saída de `ip route get <ip>` mostra explicitamente por qual interface e via qual gateway aquele destino específico seria alcançado — útil quando há mais de uma rota possível.

## 2. DNS

```bash
dig exemplo.com
dig exemplo.com +trace
resolvectl status
cat /etc/resolv.conf
```

**Exercício:** escolha um domínio que você sabe que não existe (ex.: `isso-nao-existe-de-verdade.com`) e rode `dig` nele. Compare a resposta (`NXDOMAIN`) com a de um domínio real. Depois rode `dig +trace` em um domínio real e leia a cadeia de servidores retornada — dos root servers até a resposta final.

**Padrão para memorizar:** "resolve por IP, não resolve por nome" quase sempre aponta para DNS, não para rede ou firewall.

## 3. Portas e sockets

```bash
ss -tulpn
ss -tan
```

**Exercício:** suba um servidor HTTP simples (`python3 -m http.server 8080`) e, em outro terminal, confirme com `ss -tulpn` que a porta 8080 aparece em `LISTEN`. Pare o servidor e rode o comando de novo — a porta some da lista. Depois, repita o teste trocando o bind para `127.0.0.1` apenas (`python3 -m http.server 8080 --bind 127.0.0.1`) e tente acessar via o IP da máquina em vez de `localhost` — reproduz, de forma controlada, o erro clássico de "escuta na porta certa, recusa a conexão".

## 4. Checklist de troubleshooting

Ordem recomendada para investigar "não conecta", sempre por evidência, nunca por suposição:

1. A porta está escutando? (`ss -tulpn` no host do serviço)
2. Em qual interface? (`0.0.0.0` vs `127.0.0.1`)
3. O nome resolve para o IP certo? (`dig`)
4. Existe rota até o destino? (`ip route get <ip>`)
5. Alguma regra de firewall descarta o pacote no caminho?
6. O handshake TCP completa? (`ss -tan` do lado do cliente durante a tentativa)

## Por que isso importa em Kubernetes

- `Service`/`ClusterIP` → regras de `iptables`/`ipvs` fazendo o mesmo trabalho de encaminhar pacote para o socket certo.
- `CoreDNS` → o mesmo problema de resolução de nome, rodando dentro do cluster.
- `Ingress` que não responde → geralmente os mesmos seis passos do checklist acima, com uma camada de abstração a mais.

## Próxima peça da trilha

`05-containers-under-the-hood/` (namespaces e cgroups) — em breve.

---

*Série Linux do Zero — [Roger Oliveira](https://www.linkedin.com/in/oliveiraroger/). Repositório: [kubernetes-do-zero-ptbr](https://github.com/roger-oliveira86/kubernetes-do-zero-ptbr).*
