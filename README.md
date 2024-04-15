# Gen AI SDLC - Maitenance PoC



## 1. install kind cluster

```shell
brew install kind
kind create cluster --name k8sgpt-demo
```

## 2. install k8sgpt

```shell
brew tap k8sgpt-ai/k8sgpt
brew install k8sgpt
```

## 3. authenticate with openai

```shell
k8sgpt generate
```

```shell
k8sgpt auth add --backend openai --model gpt-3.5-turbo
```

## 4. create a broken pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: broken-pod
  namespace: default
spec:
  containers:
    - name: broken-pod
      image: nginx:1.a.b.c
      livenessProbe:
        httpGet:
          path: /
          port: 81
        initialDelaySeconds: 3
        periodSeconds: 3
```
```shell
kubectl apply -f broken-pod.yml
```

## 5. analyze the issue

```shell
k8sgpt analyze
```
```shell
k8sgpt analyse --explain
```
