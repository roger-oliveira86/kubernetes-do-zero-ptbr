# Executable Scenario Template

Use this template to build labs where the reader learns to think operationally, not just repeat commands.

## Scenario

Describe a concrete situation in one sentence. Example: "The application is healthy, but the Service has no endpoints."

## Investigation question

What question does the reader need to answer before acting?

> Does the Service's selector match the ready Pods' labels?

## Objective and definition of done

State the verifiable end state of the exercise.

- [ ] resource created in the lab namespace;
- [ ] expected signal observed;
- [ ] failure reproduced only in the disposable environment;
- [ ] cause identified with evidence;
- [ ] healthy state restored;
- [ ] resources removed.

## Safe environment

Give one browser option, one local alternative, and the cleanup command.

| Option | Use | Cleanup |
| --- | --- | --- |
| Killercoda | browser-based lab | end the session |
| kind | temporary local cluster | `kind delete cluster --name <name>` |
| minikube | guided local cluster | `minikube delete` |

Always include:

```bash
kubectl config current-context
kubectl version
```

If the context isn't disposable, the reader should stop.

## Initial state

List the manifests and explain what each one does. Then give the apply commands and the signals that confirm a healthy state.

## Controlled change

Introduce a small, reversible change. Explain:

- what changes in the desired state;
- which controller should react;
- what evidence shows progress.

## Intentional failure

The failure must be safe, predictable and confined to the lab namespace. Don't use real data, credentials or shared resources.

## Evidence-based investigation

Organize the investigation into questions, not trial and error:

1. which resource should produce this behavior?
2. which signal shows the impact?
3. which event, log or condition explains the cause?
4. what's the smallest fix or safe way out?

Include only the commands that are necessary.

## Recovery and validation

Explain whether the right move is to fix, pause or roll back. Then show how to prove the healthy state was restored.

## Cleanup

Remove the namespace or resources created, and ask for one last confirmation.

## Review questions

Ask questions that force the reader to justify a decision. Avoid questions that only ask for a command's syntax.

## References

Use official documentation for each core object and command in the scenario.
