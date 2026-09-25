# Linux for Platform Engineering

This track builds the fundamentals that reappear in containers, Kubernetes, observability and troubleshooting.

The conceptual order is: **Linux → processes → networking → containers → Kubernetes → observability → SRE → Platform Engineering**. Folder names preserve the material's evolution; use this index as the entry point.

| Piece | Subject | Format | Status |
| --- | --- | --- | --- |
| [01 — Distros and terminal](./01-distros-e-terminal/README.md) | Linux environment and initial system inspection | Guide + commands | Published |
| [02 — Processes](./02-processos/README.md) | PID, PPID, states and investigation | Guide + experiment | Published |
| [03 — Networking and troubleshooting](./04-redes-e-troubleshooting/README.md) | Routes, DNS, ports and sockets | Guided lab | Published |
| [04 — Users and permissions](./03-usuarios-e-permissoes/README.md) | UID, GID, permissions, ACLs and `securityContext` | Guide + exercise | Published |
| [05 — Filesystems and inodes](./05-sistema-de-arquivos-e-inodes/README.md) | Inodes, `df -i`, deleted-but-open files and `DiskPressure` | Guide + exercise | Published |

## Essays

[Essays — Linux for Platform Engineering](./ENSAIOS.md) collects short analyses connecting these fundamentals to platform decisions.

## How to use it

1. Run the commands in a VM or a disposable lab environment.
2. Write down what you observed before moving to the next piece.
3. Don't reproduce mutating commands on production systems without understanding the effect and having a safe way out.

The examples are educational. Confirm behavior, version and permissions in the environment where they'll run.
