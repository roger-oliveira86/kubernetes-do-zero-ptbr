# Laboratório 01 — Deployment, Service, rollout e rollback

Este laboratório transforma um Pod isolado em uma aplicação declarada, replicada e atualizável. O objetivo não é apenas executar comandos: é observar como o Deployment reduz risco ao manter réplicas disponíveis durante uma mudança e como o rollback devolve o sistema a uma revisão conhecida.

## O que você aprenderá

Ao final, você conseguirá:

- criar um namespace isolado para o laboratório;
- aplicar um Deployment com três réplicas e estratégia RollingUpdate;
- expor os Pods por um Service do tipo ClusterIP;
- acompanhar uma atualização disparada por mudança no template;
- reconhecer um rollout que não progride;
- usar `kubectl rollout undo` para retornar à revisão anterior;
- limpar todos os recursos criados.

## Pré-requisitos

- cluster Kubernetes acessível;
- `kubectl` configurado para o contexto de laboratório;
- permissão para criar namespace, Deployment e Service;
- terminal nesta pasta.

Para não misturar o exercício com recursos reais, confira o contexto antes de começar:

```bash
kubectl config current-context
kubectl cluster-info
```

## Arquivos

| Arquivo | Responsabilidade |
| --- | --- |
| `namespace.yaml` | isola os recursos do laboratório |
| `deployment.yaml` | declara três réplicas, a estratégia de atualização e uma readiness probe |
| `service.yaml` | fornece um endereço estável para os Pods selecionados |

## 1. Criar a versão inicial

```bash
kubectl apply -f namespace.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl -n laboratorio-rollout rollout status deployment/web
kubectl -n laboratorio-rollout get deployment,rs,pods,service
```

Resultado esperado: o Deployment informa `successfully rolled out`, existem três Pods prontos e o Service `web` possui endpoints.

Verifique os endpoints:

```bash
kubectl -n laboratorio-rollout get endpointslices -l kubernetes.io/service-name=web
```

Opcionalmente, em outro terminal, encaminhe uma porta local para o Service:

```bash
kubectl -n laboratorio-rollout port-forward service/web 8080:80
```

Então acesse `http://localhost:8080` no navegador ou com:

```bash
curl -I http://localhost:8080
```

## 2. Observar um rollout controlado

Altere uma variável do template do Pod. A aplicação NGINX não usa essa variável; ela existe para demonstrar uma mudança declarativa que cria uma nova revisão sem depender de uma tag de imagem específica.

```bash
kubectl -n laboratorio-rollout set env deployment/web MENSAGEM_RELEASE=v2
kubectl -n laboratorio-rollout rollout status deployment/web
kubectl -n laboratorio-rollout rollout history deployment/web
kubectl -n laboratorio-rollout get rs
```

Observe que o Deployment cria um ReplicaSet novo e reduz o anterior de modo gradual. Os limites `maxUnavailable: 1` e `maxSurge: 1` tornam explícito o compromisso: durante a atualização, no máximo uma réplica desejada fica indisponível e no máximo uma réplica adicional é criada.

## 3. Simular uma mudança que não fica pronta

Agora introduza deliberadamente uma imagem inexistente. Isto é um exercício controlado; não use esse padrão para testar produção.

```bash
kubectl -n laboratorio-rollout set image deployment/web web=nginx:imagem-inexistente
kubectl -n laboratorio-rollout rollout status deployment/web --timeout=60s
```

O comando deve terminar por timeout, pois o novo Pod não conseguirá baixar a imagem. Investigue antes de corrigir:

```bash
kubectl -n laboratorio-rollout get pods
kubectl -n laboratorio-rollout describe deployment web
kubectl -n laboratorio-rollout describe pod -l app.kubernetes.io/name=web
kubectl -n laboratorio-rollout get events --sort-by=.metadata.creationTimestamp
```

Procure por mensagens como `ErrImagePull` ou `ImagePullBackOff`. A decisão não é “dar retry até funcionar”; é comparar o estado atual com o impacto e retornar a uma revisão conhecida quando a nova não progride.

## 4. Executar rollback

```bash
kubectl -n laboratorio-rollout rollout undo deployment/web
kubectl -n laboratorio-rollout rollout status deployment/web
kubectl -n laboratorio-rollout rollout history deployment/web
kubectl -n laboratorio-rollout get pods
```

Resultado esperado: o Deployment retorna à última revisão saudável e volta a ter três Pods prontos.

## 5. Limpar o ambiente

```bash
kubectl delete namespace laboratorio-rollout
```

Confirme:

```bash
kubectl get namespace laboratorio-rollout
```

O namespace deve deixar de aparecer após a exclusão ser concluída.

## Perguntas de revisão

1. Por que uma mudança em uma variável de ambiente cria uma nova revisão do Deployment?
2. Qual a diferença entre o estado de um Pod individual e o estado desejado do Deployment?
3. Qual evidência mostrou que a imagem inválida não era um rollout lento, mas uma atualização que não podia completar?
4. Em que situação você escolheria pausar uma atualização em vez de executar rollback?

## Referências

- [Deployments — Kubernetes](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Service — Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/)
- [kubectl rollout undo — Kubernetes](https://kubernetes.io/docs/reference/kubectl/generated/kubectl_rollout/kubectl_rollout_undo/)
