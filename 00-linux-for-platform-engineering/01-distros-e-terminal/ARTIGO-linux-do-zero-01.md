
# Linux From Scratch #1: what is Linux, and which distribution should you pick?

When someone starts studying DevOps, SRE or Kubernetes, they usually run into a list of tools: Docker, Terraform, Kubernetes, Helm, ArgoCD and many others.

But there's a layer underneath all of that: the operating system.

If you don't understand processes, permissions, services, networking and logs, any error inside a container or a cluster looks like a mystery.

## Linux isn't just the terminal

Linux is the kernel that coordinates resources like CPU, memory, storage, networking and processes. A Linux distribution bundles that kernel with tools, libraries, package managers and an installation method.

That's why different distributions exist. Ubuntu, Debian, Fedora, Rocky Linux and Alpine all use Linux, but they make different choices around management, updates, security and intended use.

## Which distribution should you pick?

### Ubuntu LTS

This is my recommendation for the first lab. It has a large community, plenty of learning material, and is very common in cloud environments.

Use it to:

- learn Linux;
- create your first virtual machine;
- study Docker and Kubernetes;
- build local labs.

### Debian Stable

An excellent choice for understanding a stable server base and the Debian ecosystem more directly.

Use it after your first contact with Linux, when you want to compare installation, packages and administration on a more conservative system.

### Fedora, Rocky Linux and RHEL

These distributions bring you closer to the Red Hat ecosystem, which is very present in companies. Fedora tends to ship newer technology; Rocky Linux and RHEL are more associated with corporate environments.

An important topic on this track is SELinux, which shows that security shouldn't be solved by simply disabling controls.

### Alpine Linux

Alpine is very useful for understanding small images and minimal containers. It is not, however, my recommendation for learning Linux from scratch.

Learn processes, files, permissions, networking and logs on a more complete distribution first. Then compare what changes on an Alpine image.

## My practical recommendation

Follow this order:

```text
Ubuntu LTS → Debian Stable → Fedora/Rocky → Alpine in containers
```

You don't need to replace your main computer. Start in a virtual machine, take snapshots, and treat every mistake as part of the lab.

## The first exercise

After starting an Ubuntu VM, run:

```bash
pwd
ls -la
whoami
uname -a
cat /etc/os-release
df -h
free -h
```

Now answer:

1. Which directory are you in?
2. Which user is running the commands?
3. Which distribution and version are installed?
4. How much disk space is available?
5. How much memory does the system have?

It looks simple, but this practice already starts building the right mindset: observe the current state before changing the system.

## The Platform Engineering mindset

The traditional path is to memorize a command for every error.

The professional path is to investigate:

- is the process running?
- is the service active?
- is the port open?
- does DNS resolve?
- does the user have permission?
- what does the log say the cause is?

This mindset becomes essential once we get to Docker, containerd, kubelet and Kubernetes.

## Next step

In the next article, we'll explore the terminal and the commands that make up the daily toolkit of anyone working with infrastructure:

- `pwd`;
- `ls`;
- `cd`;
- `grep`;
- `ps`;
- `ss`;
- `journalctl`.

The goal isn't to memorize a list. It's to know which question each command helps you answer.

> Which Linux distribution did you use on your first server or lab?

## Official references

- Ubuntu Desktop Guide: https://help.ubuntu.com/
- Fedora Beginner's Guide: https://docs.fedoraproject.org/en-US/beginners-guide/
- Debian Installation Guide: https://www.debian.org/releases/stable/amd64/
