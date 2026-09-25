# Lab 01 — First commands with kubectl

In this lab, you'll check the cluster, create a Pod, and use the first Kubernetes diagnostic commands.

## Objectives

By the end, you'll know how to:

- check the cluster's nodes and namespaces;
- create a Pod from a YAML manifest;
- check the Pod's state;
- inspect events and details;
- check logs;
- remove the created resource.

## Prerequisites

- access to a Kubernetes cluster;
- `kubectl` installed and connected to the cluster;
- a terminal open in this folder.

You can also use a browser-based lab environment, such as Killercoda.

## 1. Check the connection to the cluster

```bash
kubectl cluster-info
```

The result should show the addresses of the control plane and the main services.

## 2. List the nodes

```bash
kubectl get nodes
```

Expected result: at least one node in the `Ready` state.

## 3. List the namespaces

```bash
kubectl get namespaces
```

You should find namespaces like `default`, `kube-system` and `kube-public`.

## 4. Create the first Pod

```bash
kubectl apply -f pod.yaml
```

Expected result:

```text
pod/first-pod created
```

## 5. Track the Pod's state

```bash
kubectl get pods
```

If the image is still being pulled, the state may temporarily show as `ContainerCreating`. Wait a few seconds and run the command again. The expected final state is `Running`.

To follow the change in real time, use:

```bash
kubectl get pods --watch
```

Press `Ctrl+C` to stop watching.

## 6. Inspect the Pod

```bash
kubectl describe pod first-pod
```

Pay special attention to:

- `Status`;
- `Containers`;
- `Conditions`;
- `Events`, at the end of the output.

The events help you understand scheduling problems, image pulls and container startup.

## 7. Check the logs

```bash
kubectl logs first-pod
```

Expected result:

```text
The first Pod is working.
```

To follow new messages:

```bash
kubectl logs -f first-pod
```

Press `Ctrl+C` to exit.

## 8. Query using labels

```bash
kubectl get pods -l app=first-pod
```

Kubernetes will return only the Pods that have the label `app=first-pod`.

## 9. Clean up the environment

```bash
kubectl delete -f pod.yaml
```

Expected result:

```text
pod "first-pod" deleted
```

Confirm the removal:

```bash
kubectl get pods
```

## Optional challenge

Change the message in the `pod.yaml` file, apply the manifest again, and check the new logs. Since the Pod's `command` field can't be changed directly, remove the Pod before recreating it:

```bash
kubectl delete -f pod.yaml
kubectl apply -f pod.yaml
kubectl logs first-pod
```

## Checklist

- [ ] The cluster responded to `kubectl cluster-info`.
- [ ] At least one node showed up as `Ready`.
- [ ] The Pod reached the `Running` state.
- [ ] The `describe` command showed details and events.
- [ ] The `logs` command displayed the application's message.
- [ ] The Pod was removed at the end.
