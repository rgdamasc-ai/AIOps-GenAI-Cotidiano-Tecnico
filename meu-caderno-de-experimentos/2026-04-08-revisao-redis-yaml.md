# Experimento 01 — Revisão de Manifesto Kubernetes

**Data:** 2025-05-08
**Ferramenta usada:** Claude
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

## Contexto

Revisão crítica de um manifesto Kubernetes multi-recurso composto por Namespace, PersistentVolumeClaim, Deployment e Service para uma aplicação Redis em ambiente de laboratório (lab-aiops). O objetivo foi identificar problemas de segurança, resiliência e boas práticas antes de promover o manifesto para produção.

## Prompt usado

```
Realize uma revisão crítica e detalhada do manifesto YAML a seguir:

apiVersion: v1
kind: Namespace
metadata:
  name: lab-aiops
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: redis-pvc
  namespace: lab-aiops
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
  namespace: lab-aiops
  labels:
    app: redis
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
        - name: redis
          image: redis:alpine
          ports:
            - containerPort: 6379
              name: redis
          resources:
            limits:
              cpu: 100m
              memory: 128Mi
            requests:
              cpu: 100m
              memory: 128Mi
          livenessProbe:
            tcpSocket:
              port: 6379
            initialDelaySeconds: 30
            periodSeconds: 10
          volumeMounts:
            - name: redis-data
              mountPath: /data
      volumes:
        - name: redis-data
          persistentVolumeClaim:
            claimName: redis-pvc
---
apiVersion: v1
kind: Service
metadata:
  name: redis
  namespace: lab-aiops
spec:
  selector:
    app: redis
  ports:
    - protocol: TCP
      port: 6379
      targetPort: 6379
  type: ClusterIP
```

## Resposta da IA

A IA entregou uma revisão estruturada em três camadas de severidade. Entre os **problemas críticos**: uso de tag mutável `redis:alpine`, ausência de `readinessProbe`, antipadrão Deployment + PVC `ReadWriteOnce` (recomendado migrar para `StatefulSet`) e ausência de `securityContext` (container rodando como root). Entre os **pontos de atenção**: `livenessProbe` sem `timeoutSeconds`/`failureThreshold`, `resources.requests == limits` com CPU potencialmente baixa, PVC sem `storageClassName` explícito, `targetPort` numérico em vez de nomeado e ausência de `NetworkPolicy`. Como **melhorias adicionais** sugeriu `PodDisruptionBudget`, `ResourceQuota`/`LimitRange` no namespace e autenticação do Redis. Cada item veio acompanhado de snippet YAML pronto para correção.

## O que funcionou

- Identificou de primeira o antipadrão Deployment + RWO PVC e sugeriu `StatefulSet` de imediato
- Forneceu snippets YAML prontos para cada correção (readinessProbe, securityContext, NetworkPolicy)
- Organizou a resposta por severidade (crítico / atenção / melhorias), facilitando priorização
- Apontou o risco do `targetPort` numérico sem que fosse solicitado explicitamente

## O que não funcionou

- Não levou em conta o contexto de "lab" — as recomendações de segurança foram apresentadas com a mesma urgência de produção, gerando ruído
- Não perguntou se havia um `StorageClass` padrão configurado no cluster antes de sinalizar o problema do PVC
- A sugestão de usar digest da imagem foi mencionada sem exemplo concreto de como obtê-lo

## Ajustes que eu fiz

- Não foi necessário reformular o prompt — a primeira resposta já cobriu os principais pontos
- Adicionei contexto sobre o ambiente de lab em um follow-up para filtrar recomendações excessivas
- Pedi um exemplo de `StatefulSet` equivalente ao `Deployment` atual para facilitar a migração

## Nota (1 a 5)

**Nota:** 4/5

**Justificativa:** Revisão abrangente e bem estruturada em primeira tentativa. Descontou-se um ponto pela falta de gradação entre contexto de lab e produção — todas as recomendações foram apresentadas com a mesma urgência, o que pode gerar ruído em ambiente experimental.

## O que eu faria diferente

Incluiria o contexto de ambiente (lab vs. produção) já no prompt inicial, com uma instrução explícita do tipo "priorize problemas de segurança apenas se o manifesto fosse para produção, sinalize os demais como nice-to-have em lab". Isso evitaria a lista longa de recomendações de mesma severidade e tornaria a saída mais acionável. No Módulo 02, valeria explorar prompts com persona ("atue como engenheiro de plataforma sênior revisando para ambiente de staging") para calibrar melhor o nível de detalhe.