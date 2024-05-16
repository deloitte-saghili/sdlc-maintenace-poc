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

## 3. authenticate with ai backend 

### 3.1 OpenAI

```shell
k8sgpt generate
```


```shell
k8sgpt auth add --backend openai --model gpt-3.5-turbo
```

### 3.2 Amazon Bedrock

Authenticate with K8SGPT user credentials on AWS Sandbox

```shell
aws configure
```

```shell
k8sgpt auth add --backend amazonbedrock --model anthropic.claude-v2
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
export AI_BACKEND=amazonbedrock # or openai . run `k8sgpt auth list` for a complete list of available ai backends
k8sgpt analyse --explain --backend $AI_BACKEND
```
