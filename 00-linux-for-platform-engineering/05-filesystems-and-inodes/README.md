# 05 — Filesystems and Inodes

> Part of the **Linux From Scratch** track (piece #5: *Filesystems and inodes*).
> Full article: [DEV.to](https://dev.to/rogeroliveira86/-linux-from-zero-inodes-and-the-filesystem-limit-df-h-doesnt-show-for-platform-engineers-4jml)

## Objective

Understand why a disk can be "full" with bytes of free space left over — and how that same limit, when ignored, turns into `DiskPressure` on a Kubernetes node.

Prerequisite: basic terminal (piece #1) and users/permissions (piece #4) from this track.

## Structure of this lab

```text
05-filesystems-and-inodes/
└── README.md   # this file
```

## 1. The problem `df -h` doesn't show

Everyone learns that `df -h` shows disk space. Few learn that there's a second limit, independent of space in bytes: the number of **inodes**. A filesystem reserves a fixed number of inodes at creation time — and every file, no matter how small, consumes exactly one. A directory with millions of small files (logs, sessions, cache) can exhaust inodes while the disk still shows gigabytes free.

The classic symptom: `No space left on device` when trying to create a file, while `df -h` says dozens of gigabytes are still available.

## 2. What an inode actually holds

An inode doesn't hold the file's name. It holds the metadata: owner, permissions, timestamps, size and the pointers to the data blocks on disk. The file's name only lives in the directory entry, which points to an inode number. That's why renaming a file is instant (it only changes the directory entry), and why a `hard link` is, in practice, two names pointing at the same inode.

```bash
ls -i /etc/hostname
```

**Exercise:** run the command above and note the number. Create a hard link (`ln /etc/hostname /tmp/hostname-link`) and run `ls -i` on both — the number should be identical.

## 3. Seeing the inode limit

```bash
df -i /
```

If `IUse%` is close to 100% while `df -h` shows free space, the problem is inodes, not bytes.

**Exercise:** compare the output of `df -h /` and `df -i /` on your machine. Then create 200,000 empty files in a test directory (`for i in $(seq 1 200000); do touch /tmp/test/f$i; done`) and watch `df -i` change while `df -h` barely moves. Don't forget to clean up the directory afterward.

## 4. Finding who's consuming the inodes

```bash
for dir in /var/log /var/cache /tmp; do
  echo "$dir: $(find "$dir" -xdev | wc -l) files"
done
```

In production, the usual suspects are: web application session directories, dead mail queues, and rotated logs with no retention limit.

## 5. `lsof +L1` — the other way to lose space

There's a second classic form of "disk full with free space left": a process keeps a deleted file open. `unlink` removes the directory entry, but the inode is only freed once the last process closes the descriptor. In the meantime, the space doesn't show up in `du`, but it doesn't come back to `df` either.

```bash
lsof +L1
```

A line with `NLINK 0` is the alert: zero links in the directory, but the file still occupies space because some process is keeping it open. The fix isn't to delete it again — there's no longer an entry to delete — it's to restart or signal the process so it closes and reopens the file (`logrotate` with `copytruncate`, or a `kill -HUP`).

**Exercise:** in one terminal, open a file with `tail -f file.log`; in another terminal, delete the file (`rm file.log`); run `lsof +L1` and watch the line with `NLINK 0` while the first terminal still keeps it open.

## 6. The connection to Kubernetes: `DiskPressure`

The kubelet monitors the node's disk on two axes, not one: free bytes and free inodes. When either one crosses the configured threshold (`nodefs.inodesFree`, around 5% by default), the kubelet marks the node with the `DiskPressure` condition and starts evicting pods to free up space — starting with the lowest-priority ones, then the ones consuming the most in `emptyDir` and container logs.

```bash
kubectl describe node <node-name> | grep -A2 DiskPressure
```

A node can be `DiskPressure=True` and show a "normal" `df -h` at the same time, if the cause is inodes exhausted by an application generating lots of small files in `/var/lib/containerd` or in the logs volume. Investigating without knowing this leads to the wrong conclusion ("there's space, it's not disk"), and the root cause is never found.

## Investigation checklist

1. Run `df -h` **and** `df -i` together, always — never one without the other.
2. If the volume has lots of small, short-lived files, ask whether it should have more inodes reserved, or whether the application should clean up after itself.
3. In production, monitor `node_filesystem_files_free` (Prometheus/node_exporter) with the same alert that already exists for free bytes.

The examples are educational. Confirm behavior, version and permissions in the environment where they'll run.

---

*Linux From Scratch series — [Roger Oliveira](https://www.linkedin.com/in/oliveiraroger/). Repository: [kubernetes-do-zero-ptbr](https://github.com/roger-oliveira86/kubernetes-do-zero-ptbr).*
