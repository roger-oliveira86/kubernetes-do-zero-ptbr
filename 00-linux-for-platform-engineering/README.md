<div align="center">

# Linux for Platform Engineering

**The track that builds the fundamentals reappearing in containers, Kubernetes, observability and troubleshooting.**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=flat-square)](../LICENSE)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat-square&logo=linux&logoColor=black)](https://kernel.org/)

[← Back to main README](../README.md)

</div>

---

The conceptual order is: **Linux → processes → networking → containers → Kubernetes → observability → SRE → Platform Engineering**. Folder names preserve the material's evolution; use this index as the entry point.

## Contents

- [Chapters](#chapters)
- [Essays](#essays)
- [How to use it](#how-to-use-it)

## Chapters

| Piece | Subject | Format | Status |
| --- | --- | --- | --- |
| [01 — Distros and terminal](./01-distros-and-terminal/README.md) | Linux environment and initial system inspection | Guide + commands | Published |
| [02 — Processes](./02-processes/README.md) | PID, PPID, states and investigation | Guide + experiment | Published |
| [03 — Networking and troubleshooting](./04-networking-and-troubleshooting/README.md) | Routes, DNS, ports and sockets | Guided lab | Published |
| [04 — Users and permissions](./03-users-and-permissions/README.md) | UID, GID, permissions, ACLs and `securityContext` | Guide + exercise | Published |
| [05 — Filesystems and inodes](./05-filesystems-and-inodes/README.md) | Inodes, `df -i`, deleted-but-open files and `DiskPressure` | Guide + exercise | Published |

## Essays

[Essays — Linux for Platform Engineering](./ENSAIOS.md) collects short analyses connecting these fundamentals to platform decisions.

## How to use it

1. Run the commands in a VM or a disposable lab environment.
2. Write down what you observed before moving to the next piece.
3. Don't reproduce mutating commands on production systems without understanding the effect and having a safe way out.

The examples are educational. Confirm behavior, version and permissions in the environment where they'll run.
