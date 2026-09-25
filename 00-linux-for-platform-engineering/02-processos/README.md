# 02 — Processes

> Track: Linux → **processes** → networking → containers → Docker → containerd → Kubernetes → observability → SRE → Platform Engineering → architecture.

## Objective of this chapter

Understand what a process actually is on Linux: identity (PID), hierarchy (PPID), state (STAT) and lifetime (ETIME) — the conceptual base for the namespaces and containers chapters ahead.

## Guided experiment

1. Create a test process:
   ```bash
   sleep 300 &
   ```
2. Inspect it:
   ```bash
   ps -eo pid,ppid,stat,etime,cmd | grep sleep
   ```
3. Read the system's full hierarchy:
   ```bash
   ps -eo pid,ppid,stat,etime,cmd --forest
   ```

## Quick reference — `ps` fields

| Field | What it means | Why it matters in production |
|---|---|---|
| `PID` | Unique process identifier | Basis for any diagnostic or kill action |
| `PPID` | Parent process's PID | Explains reparenting and why "killing the parent" takes down the children |
| `STAT` | Current state (`R`, `S`, `D`, `Z`, ...) | A prolonged `D` points to uninterruptible waiting, often tied to I/O or storage; investigate the resource being waited on before concluding the cause |
| `ETIME` | Process lifetime | Helps distinguish a freshly created process from one stuck for hours |

## Exercises

- [ ] Run `ps -eo pid,ppid,stat,etime,cmd --forest` and identify 3 parent/child relationships on your system.
- [ ] Find (or fail to find) a `Z` process — write down the result.
- [ ] Compare the static output of `ps aux` with the continuous view of `top`/`htop`.

## Why this chapter matters for Kubernetes

Linux kernel namespaces (the subject of the next chapter) isolate exactly these same process trees. A Kubernetes Pod, underneath, is one or more Linux processes isolated by namespace — understanding process here is what makes `kubectl describe pod` make sense later on.

## Full article

Read the full narrative version of this chapter in [`ARTIGO-linux-do-zero-02.md`](./ARTIGO-linux-do-zero-02.md), also published on [Medium](https://medium.com/@rogeroliveira86) and [DEV.to](https://dev.to/rogeroliveira86).

## Next chapter

`03-namespaces` — what actually isolates one process from another.
