# 04 — Networking and Troubleshooting

> Part of the **Linux From Scratch** track (piece #3: *Networking, DNS, ports and sockets*).
> Full articles: [Medium](https://medium.com/@rogeroliveira86/linux-do-zero-515b4c682a91) · [DEV.to](https://dev.to/rogeroliveira86/linux-from-zero-networking-dns-ports-and-sockets-for-platform-engineers-240h)

## Objective

Practice, on a Linux VM or container, the commands and reasoning used to investigate networking, DNS and connectivity problems — the foundation that reappears, almost untranslated, inside Kubernetes (`Service`, `CoreDNS`, `Ingress`).

Prerequisite: basic terminal and permissions (pieces #1 and #2 of this track).

## Structure of this lab

```text
04-networking-and-troubleshooting/
├── README.md                  # this file
├── 01-interfaces-e-rotas.md   # ip a, ip route
├── 02-dns.md                  # dig, resolvectl, /etc/resolv.conf
├── 03-portas-e-sockets.md     # ss, LISTEN vs ESTABLISHED
└── 04-checklist-troubleshooting.md
```

## 1. Interfaces and routes

```bash
ip a
ip route
ip route get 8.8.8.8
```

**Exercise:** run `ip a` on your machine and identify: which interface holds the default route, and which IP address is attached to it. If you have Docker installed, run `ip a` again and note the new interface (`docker0` or `br-...`) that shows up.

**What to observe:** the output of `ip route get <ip>` explicitly shows through which interface and via which gateway that specific destination would be reached — useful when more than one route is possible.

## 2. DNS

```bash
dig example.com
dig example.com +trace
resolvectl status
cat /etc/resolv.conf
```

**Exercise:** pick a domain you know doesn't exist (e.g. `this-really-does-not-exist.com`) and run `dig` against it. Compare the answer (`NXDOMAIN`) with that of a real domain. Then run `dig +trace` on a real domain and read the chain of servers it returns — from the root servers down to the final answer.

**Pattern worth memorizing:** "resolves by IP, doesn't resolve by name" almost always points to DNS, not to networking or the firewall.

## 3. Ports and sockets

```bash
ss -tulpn
ss -tan
```

**Exercise:** start a simple HTTP server (`python3 -m http.server 8080`) and, in another terminal, confirm with `ss -tulpn` that port 8080 shows up as `LISTEN`. Stop the server and run the command again — the port disappears from the list. Then repeat the test, binding only to `127.0.0.1` (`python3 -m http.server 8080 --bind 127.0.0.1`), and try to reach it via the machine's IP instead of `localhost` — this reproduces, in a controlled way, the classic "listening on the right port, connection refused" error.

## 4. Troubleshooting checklist

Recommended order for investigating "can't connect", always by evidence, never by assumption:

1. Is the port listening? (`ss -tulpn` on the service's host)
2. On which interface? (`0.0.0.0` vs `127.0.0.1`)
3. Does the name resolve to the right IP? (`dig`)
4. Is there a route to the destination? (`ip route get <ip>`)
5. Does any firewall rule drop the packet along the way?
6. Does the TCP handshake complete? (`ss -tan` on the client side during the attempt)

## Why this matters in Kubernetes

- `Service`/`ClusterIP` → `iptables`/`ipvs` rules doing the same job of forwarding a packet to the right socket.
- `CoreDNS` → the same name-resolution problem, running inside the cluster.
- An `Ingress` that doesn't respond → usually the same six steps from the checklist above, with one extra layer of abstraction.

## Next piece of the track

`05-containers-under-the-hood/` (namespaces and cgroups) — coming soon.

---

*Linux From Scratch series — [Roger Oliveira](https://www.linkedin.com/in/oliveiraroger/). Repository: [kubernetes-do-zero-ptbr](https://github.com/roger-oliveira86/kubernetes-do-zero-ptbr).*
