# Lab 01 — Deployment, Service, Rollout and Rollback

This lab turns an isolated Pod into a declared, replicated, updatable application. The goal isn't just to run commands: it's to observe how a Deployment reduces risk by keeping replicas available during a change, and how a rollback returns the system to a known revision.

## What you'll learn

By the end, you'll be able to:

- create an isolated namespace for the lab;
- apply a Deployment with three replicas and a RollingUpdate strategy;
- expose the Pods through a ClusterIP Service;
- follow an update triggered by a template change;
- recognize a rollout that isn't progressing;
- use `kubectl rollout undo` to return to the previous revision;
- clean up every resource created.

## Prerequisites

- an accessible Kubernetes cluster;
- `kubectl` configured for the lab context;
- permission to create a namespace, Deployment and Service;
- a terminal open in this folder.

To avoid mixing the exercise with real resources, check your context before starting:

```bash
kubectl config current-context
kubectl cluster-info
```

## Where to run it

Pick exactly one disposable environment. This lab **must not** be run against a work or production cluster.

| Option | When to use it | Start and cleanup |
| --- | --- | --- |
| [Killercoda Kubernetes Playground](https://killercoda.com/playgrounds/scenario/kubernetes) | Best option to get started: runs in the browser, no local install needed. | Open the playground, run the commands in this guide, end the session when done. |
| [kind](https://kind.sigs.k8s.io/) | To repeat the scenario locally with Docker, Podman or nerdctl. | `kind create cluster --name kubernetes-from-scratch`, and at the end `kind delete cluster --name kubernetes-from-scratch`. |
| [minikube](https://minikube.sigs.k8s.io/docs/start/) | For anyone already using a guided local cluster. | `minikube start`, and at the end `minikube delete`. |

Before applying any file, record the version and context you're using. This lets you compare results without turning a command into a blind recipe:

```bash
kubectl version
kubectl config current-context
```

If the context or version isn't what you expect, stop. The first step of good operations is validating where the change will happen.

## Files

| File | Responsibility |
| --- | --- |
| `namespace.yaml` | isolates the lab's resources |
| `deployment.yaml` | declares three replicas, the update strategy and a readiness probe |
| `service.yaml` | provides a stable address for the selected Pods |

## 1. Create the initial version

```bash
kubectl apply -f namespace.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl -n lab-rollout rollout status deployment/web
kubectl -n lab-rollout get deployment,rs,pods,service
```

Expected result: the Deployment reports `successfully rolled out`, three Pods are ready, and the `web` Service has endpoints.

Check the endpoints:

```bash
kubectl -n lab-rollout get endpointslices -l kubernetes.io/service-name=web
```

Optionally, in another terminal, forward a local port to the Service:

```bash
kubectl -n lab-rollout port-forward service/web 8080:80
```

Then access `http://localhost:8080` in your browser or with:

```bash
curl -I http://localhost:8080
```

## 2. Observe a controlled rollout

Change an environment variable in the Pod template. The NGINX application doesn't use this variable; it exists to demonstrate a declarative change that creates a new revision without depending on a specific image tag.

```bash
kubectl -n lab-rollout set env deployment/web RELEASE_MESSAGE=v2
kubectl -n lab-rollout rollout status deployment/web
kubectl -n lab-rollout rollout history deployment/web
kubectl -n lab-rollout get rs
```

Notice that the Deployment creates a new ReplicaSet and gradually scales down the previous one. The `maxUnavailable: 1` and `maxSurge: 1` limits make the trade-off explicit: during the update, at most one desired replica is unavailable and at most one extra replica is created.

## 3. Simulate a change that never becomes ready

Now deliberately introduce a nonexistent image. This is a controlled exercise; don't use this pattern to test production.

```bash
kubectl -n lab-rollout set image deployment/web web=nginx:nonexistent-image
kubectl -n lab-rollout rollout status deployment/web --timeout=60s
```

The command should end with a timeout, since the new Pod won't be able to pull the image. Investigate before fixing it:

```bash
kubectl -n lab-rollout get pods
kubectl -n lab-rollout describe deployment web
kubectl -n lab-rollout describe pod -l app.kubernetes.io/name=web
kubectl -n lab-rollout get events --sort-by=.metadata.creationTimestamp
```

Look for messages like `ErrImagePull` or `ImagePullBackOff`. The decision isn't "retry until it works" — it's comparing the current state against the impact and returning to a known revision when the new one isn't progressing.

## 4. Run the rollback

```bash
kubectl -n lab-rollout rollout undo deployment/web
kubectl -n lab-rollout rollout status deployment/web
kubectl -n lab-rollout rollout history deployment/web
kubectl -n lab-rollout get pods
```

Expected result: the Deployment returns to the last healthy revision and is back to three ready Pods.

## 5. Clean up the environment

```bash
kubectl delete namespace lab-rollout
```

Confirm:

```bash
kubectl get namespace lab-rollout
```

The namespace should stop appearing once deletion completes.

## Review questions

1. Why does changing an environment variable create a new Deployment revision?
2. What's the difference between an individual Pod's state and the Deployment's desired state?
3. What evidence showed that the invalid image wasn't a slow rollout, but an update that couldn't complete?
4. In what situation would you choose to pause an update instead of running a rollback?

## References

- [Deployments — Kubernetes](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Service — Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/)
- [kubectl rollout undo — Kubernetes](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_rollout/kubectl_rollout_undo/)
