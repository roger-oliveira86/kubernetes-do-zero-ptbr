# Laboratório 01 — Primeiros comandos com kubectl

Neste laboratório, você verificará o cluster, criará um Pod e usará os primeiros comandos de diagnóstico do Kubernetes.

## Objetivos

Ao concluir, você saberá:

- verificar os nodes e namespaces do cluster;
- criar um Pod a partir de um manifesto YAML;
- consultar o estado do Pod;
- inspecionar eventos e detalhes;
- consultar logs;
- remover o recurso criado.

## Pré-requisitos

- acesso a um cluster Kubernetes;
- `kubectl` instalado e conectado ao cluster;
- terminal aberto nesta pasta.

Você também pode usar um ambiente de laboratório no navegador, como o Killercoda.

## 1. Verifique a conexão com o cluster

```bash
kubectl cluster-info
```

O resultado deve mostrar os endereços do control plane e dos serviços principais.

## 2. Liste os nodes

```bash
kubectl get nodes
```

Resultado esperado: pelo menos um node com o estado `Ready`.

## 3. Liste os namespaces

```bash
kubectl get namespaces
```

Você deve encontrar namespaces como `default`, `kube-system` e `kube-public`.

## 4. Crie o primeiro Pod

```bash
kubectl apply -f pod.yaml
```

Resultado esperado:

```text
pod/primeiro-pod created
```

## 5. Acompanhe o estado do Pod

```bash
kubectl get pods
```

Se a imagem ainda estiver sendo baixada, o estado pode aparecer temporariamente como `ContainerCreating`. Aguarde alguns segundos e execute o comando novamente. O estado final esperado é `Running`.

Para acompanhar a mudança em tempo real, use:

```bash
kubectl get pods --watch
```

Pressione `Ctrl+C` para encerrar o acompanhamento.

## 6. Inspecione o Pod

```bash
kubectl describe pod primeiro-pod
```

Observe especialmente:

- `Status`;
- `Containers`;
- `Conditions`;
- `Events`, no final da saída.

Os eventos ajudam a entender problemas de agendamento, download de imagens e inicialização de containers.

## 7. Consulte os logs

```bash
kubectl logs primeiro-pod
```

Resultado esperado:

```text
O primeiro Pod esta funcionando.
```

Para acompanhar novas mensagens:

```bash
kubectl logs -f primeiro-pod
```

Pressione `Ctrl+C` para sair.

## 8. Faça uma consulta usando labels

```bash
kubectl get pods -l app=primeiro-pod
```

O Kubernetes retornará somente os Pods que possuem a label `app=primeiro-pod`.

## 9. Limpe o ambiente

```bash
kubectl delete -f pod.yaml
```

Resultado esperado:

```text
pod "primeiro-pod" deleted
```

Confirme a remoção:

```bash
kubectl get pods
```

## Desafio opcional

Altere a mensagem no arquivo `pod.yaml`, aplique novamente o manifesto e confira os novos logs. Como o campo `command` do Pod não pode ser alterado diretamente, remova o Pod antes de recriá-lo:

```bash
kubectl delete -f pod.yaml
kubectl apply -f pod.yaml
kubectl logs primeiro-pod
```

## Checklist

- [ ] O cluster respondeu ao `kubectl cluster-info`.
- [ ] Pelo menos um node apareceu como `Ready`.
- [ ] O Pod chegou ao estado `Running`.
- [ ] O comando `describe` mostrou detalhes e eventos.
- [ ] O comando `logs` exibiu a mensagem da aplicação.
- [ ] O Pod foi removido ao final.
