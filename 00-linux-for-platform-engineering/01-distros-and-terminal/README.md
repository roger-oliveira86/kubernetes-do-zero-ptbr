# Module 01 — Linux Distros and Terminal

This module introduces Linux to anyone starting out in infrastructure, DevOps, SRE and Kubernetes.

## Objectives

- Understand what Linux is and what a distribution is.
- Choose a suitable distribution to get started.
- Create an Ubuntu virtual machine.
- Use the first terminal commands.
- Observe the system's state before making changes.

## Recommended distribution

Start with Ubuntu LTS in a virtual machine.

Then move on to:

- Debian Stable;
- Fedora/Rocky Linux;
- Alpine Linux in containers.

## First lab

```bash
pwd
ls -la
whoami
uname -a
cat /etc/os-release
df -h
free -h
```

Before changing any configuration, answer: which directory am I in, which user will run the command, and what is the system's current state? This habit cuts down on trial-and-error changes.

## Next step

Move on to [Processes](../02-processes/README.md) and observe how programs appear, live and end on Linux.
