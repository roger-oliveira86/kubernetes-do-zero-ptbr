<div align="center">

# Kubernetes From Scratch

**Linux, Kubernetes and Platform Engineering fundamentals, written by someone who runs platforms in production.**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=flat-square)](./LICENSE)
[![Last commit](https://img.shields.io/github/last-commit/roger-oliveira86/kubernetes-do-zero-ptbr?style=flat-square)](https://github.com/roger-oliveira86/kubernetes-do-zero-ptbr/commits/main)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)](https://kernel.org/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](./CONTRIBUTING.md)

</div>

---

## The thesis

Platform Engineering starts before Kubernetes. Pods, namespaces, limits and network policies are repackaged Linux concepts: processes, kernel namespaces, cgroups, permissions and networking. Whoever understands the layer underneath investigates failures with evidence, instead of memorizing commands.

That's why this track starts with the operating system and only then climbs to the orchestrator:

**Linux → processes → networking → containers → Kubernetes → observability → SRE → Platform Engineering**

## Contents

- [What's already published](#whats-already-published)
- [In progress](#in-progress)
- [How to study](#how-to-study)
- [Main sources](#main-sources)
- [Contributing](#contributing)
- [Author](#author)

## What's already published

### Track — Linux for Platform Engineering

| # | Chapter | What you take away |
| --- | --- | --- |
| 01 | [Distros and terminal](./00-linux-for-platform-engineering/01-distros-e-terminal/README.md) | How to inspect an unfamiliar Linux system |
| 02 | [Processes](./00-linux-for-platform-engineering/02-processos/README.md) | PID, PPID, states and how to investigate a process |
| 03 | [Networking and troubleshooting](./00-linux-for-platform-engineering/04-redes-e-troubleshooting/README.md) | Routes, DNS, ports, sockets and the real causes of a `connection refused` |
| 04 | [Users and permissions](./00-linux-for-platform-engineering/03-usuarios-e-permissoes/README.md) | UID/GID, the `x` bit on directories, `umask` and how it maps to `securityContext` |
| 05 | [Filesystems and inodes](./00-linux-for-platform-engineering/05-sistema-de-arquivos-e-inodes/README.md) | Why a disk can "fill up" with free space left: inodes, `lsof +L1` and `DiskPressure` |

Full track index: [Linux for Platform Engineering](./00-linux-for-platform-engineering/README.md).

### Essays — platform decisions

| Essay | Topic |
| --- | --- |
| [Platform Engineering starts before Kubernetes](./ENSAIOS.md) | Who the platform's customer is, how it fails, and how to measure whether it helps |
| [Observability isn't a dashboard: it's decision support](./artigos/ensaios/observabilidade-orientada-a-decisao.md) | Metrics, logs and traces as support for deciding the next safe step of a change |
| [Wave migration with go/rollback criteria](./artigos/ensaios/migracao-em-ondas-observabilidade-para-decidir-com-seguranca.md) | When to advance, pause or roll back a progressive change, and on which signals |
| [The order of the waves: what each stage needs to teach](./artigos/ensaios/ordem-das-ondas-o-que-cada-etapa-precisa-ensinar.md) | Ordering risky changes by what each stage teaches the next, not by ease |

The essays use sanitized examples: no real system, data or incident is identifiable.

## In progress

The Kubernetes modules (workloads, cluster architecture, networking, storage and troubleshooting) are being written one at a time, always with a reproducible lab. The plan and order live in the [ROADMAP](./ROADMAP.md); the starting point is [First steps](./00-primeiros-passos/README.md).

## How to study

1. Read the chapter and run the commands in a VM or a disposable environment.
2. Write down what you observed before moving to the next one.
3. Compare against the official documentation.
4. Never run mutating commands in production without understanding the effect and having a safe way out.

## Main sources

- [Official Kubernetes documentation](https://kubernetes.io/docs/)
- [Linux man-pages](https://man7.org/linux/man-pages/)

## Contributing

Suggestions and corrections are welcome. Read [CONTRIBUTING.md](./CONTRIBUTING.md) before opening an issue or pull request.

## License

[Apache License 2.0](./LICENSE).

## Author

**Roger Oliveira** — Platform Engineering, SRE, Kubernetes and Observability. 12+ years in infrastructure, with technical leadership of platform migrations in a regulated environment.

[LinkedIn](https://www.linkedin.com/in/oliveiraroger/) · [DEV.to](https://dev.to/rogeroliveira86)
