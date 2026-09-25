# Linux From Scratch #2 — Processes: what runs underneath your terminal

*Part 2 of the "Linux From Scratch" series. If you haven't read #1 yet, it's on Medium and DEV.to — start there to understand the premise: investigate by evidence, not by memorizing commands.*

## The problem nobody quite explains

Have you ever run `ps aux` and watched a huge list of lines scroll by, without really knowing what to do with it? Most tutorials teach you to run the command, but not to *think* about the result. This post is about that: turning a list of numbers into reasoning about what your machine is actually doing.

## The experiment

Open two terminals side by side. In the first one, run:

```bash
sleep 300 &
```

This creates a process that just "sleeps" for 300 seconds — doing nothing useful, on purpose, so it can serve as our guinea pig.

In the second terminal, run:

```bash
ps -eo pid,ppid,stat,etime,cmd | grep sleep
```

You'll see a line similar to this:

```
  PID   PPID STAT     ELAPSED CMD
12345   9876 S           0:03 sleep 300
```

Now the question that matters: **what is each of these columns telling you about this process's life?**

- `PID` — this process's unique identity for as long as it exists.
- `PPID` — who created this process (the parent process). In your case, probably the first terminal's own shell.
- `STAT` — the current state. `S` means "sleeping" (interruptible). You'll see `R` (running), `Z` (zombie) and `D` (waiting on uninterruptible I/O) in other contexts — each one tells a different story about what is or isn't stalling the system.
- `ETIME` — how long it has existed.

## Why this actually matters

In production, nobody runs `sleep` on purpose — but every performance incident I've ever investigated started with exactly this question: "which processes are running, who is whose parent, and what state are they stuck in?" A process stuck in `D` (uninterruptible sleep) for a long time, for example, is almost always a symptom of a disk or storage system struggling — not an application bug.

Now try killing the process through its parent, not through `sleep` directly:

```bash
kill -TERM <your_shell's_PPID>
```

**Don't actually run this in your main terminal** — it's just for you to picture mentally what would happen: killing the parent process usually also kills the child `sleep` (or it gets "adopted" by `init`/`systemd`, depending on the system). This reparenting behavior is the same logic behind why, when a container dies unexpectedly, the processes it hosted disappear along with it.

## The bridge to where this track is heading

This process parent/child relationship is exactly what Linux uses as the foundation for isolating processes into namespaces — the mechanism that, further along this track, becomes containers, and that Kubernetes orchestrates at scale. Understanding process, today, without rushing, is what makes the "aha" moment happen when you reach `cgroups` and `namespaces` two or three posts from now.

## For you to investigate on your own

Before the next post, try answering this on your own machine (no need to reply to me, it's for you):

1. Run `ps -eo pid,ppid,stat,etime,cmd --forest` and visually identify which process is the parent of which.
2. Find a process in state `Z` (zombie) — if you don't find one, that's already information: your system is healthy on that front right now.
3. Compare `ps aux` with `top` (or `htop`) running at the same time — what changes between a static snapshot and a continuous view?

---
*Next in the track: namespaces, and what actually isolates one process from another — the foundation of everything we now call a "container".*
