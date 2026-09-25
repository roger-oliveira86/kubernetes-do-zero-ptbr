# Essays

Opinion and reflection pieces on career, Platform Engineering and infrastructure engineering — connected to this repository's technical track, but outside the module numbering (00–05), which follows the track's roadmap.

---

## Platform Engineering starts before Kubernetes

*Why the right question isn't "where do I start with Kubernetes," but "what problem am I solving."*

### The statement that tends to ruffle feathers

Every time someone asks me "where do I start to become a Platform Engineer," the answer they expect is "Kubernetes." The answer I give is: no. Kubernetes is the tool you use after you already know what you're trying to solve. Platform Engineering is about the problem, not about the orchestrator.

### What actually separates someone who "knows Kubernetes" from someone doing Platform Engineering

After years running and evolving Kubernetes platforms in production — with hundreds of workloads and thousands of pods running in a regulated corporate environment — the difference I see in practice isn't in `kubectl`. It's in three questions most tutorials never make you ask yourself:

1. **Who is your platform's customer?** It's not the cluster. It's the development team that needs to deploy without calling you every time. If you can't name who uses what you build, you're operating infrastructure, not building a platform.
2. **What happens when something breaks at 3am?** A tested rollback, a defined RTO, a clear incident owner — that's a design decision, made long before any YAML manifest exists. A platform that wasn't designed to fail well, fails badly.
3. **How do you measure whether the platform is helping or getting in the way?** If the answer is "I don't know," you have a cluster, not a platform. Capacity planning, adoption metrics, reduced rework across teams — that's what turns operations into an internal product.

### Where Linux fits into this

This is the connection the "Linux From Scratch" track has been building since the first post: every abstraction Kubernetes offers you — pods, namespaces, resource limits, network policies — is an elegant repackaging of concepts that already exist in plain Linux: processes, kernel namespaces, cgroups, iptables/nftables. When you understand the process before the pod, the kernel namespace before the Kubernetes namespace, you stop treating the cluster as a magic black box and start debugging like someone who understands the engine, not just the dashboard.

It's no coincidence that the worst production incidents I've ever resolved had their root cause one layer below Kubernetes — disk I/O, DNS, kernel limits — and not in the orchestrator itself. Someone who only knows how to operate Kubernetes goes blind exactly where the problem lives.

### The path I advocate for (and that this track follows)

Linux → processes → networking → containers → Docker → containerd → Kubernetes → observability → SRE → Platform Engineering → architecture.

This order isn't an accident. It's the order that gives you solid ground before it gives you height. Each layer explains the one above it. Jumping straight to "Kubernetes from scratch" is like learning to fly a plane without understanding what makes it fly — you can memorize the control commands, but you can't diagnose when something goes off script.

### Where this takes you in practice

If you're trying to evolve from operations to Platform Engineering — or from Platform Engineer to Staff/Principal — the real test isn't "how many YAML manifests have you written." It's: can you explain, in one sentence, to a non-technical executive, why the platform you build reduces risk and speeds up delivery? If yes, you're already thinking like a Platform Engineer. Kubernetes is just the tool you chose to execute that vision today — tomorrow it might be something else.

---

*Full version also published on [Medium](https://medium.com/@rogeroliveira86) and [DEV.to](https://dev.to/rogeroliveira86). Technical track: see [`00-linux-for-platform-engineering`](./00-linux-for-platform-engineering) onward.*
