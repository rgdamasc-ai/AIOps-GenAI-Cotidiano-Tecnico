# Experimento 01 — Diagnóstico de CrashLoopBackOff em pod Kubernetes

**Data:** 2026-05-08
**Ferramenta usada:** Claude
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

---

## Contexto

Pod de aplicação em ambiente Kubernetes (Minikube + ArgoCD) entrando em
`CrashLoopBackOff` com 10 reinicializações em 28 minutos. Container com
duração de 0 segundos — iniciava e terminava no mesmo timestamp.

---

## Prompt usado

```
Analise os logs abaixo de um sistema em produção.
Resuma o comportamento, agrupe eventos por categoria, identifique correlações entre os problemas e sugira a causa raiz mais provável.
O ambiente utiliza ArgoCD.

kubectl describe pod jogo-da-velha-6987bbdc5c-wgkq9 -n lab-aiops
Name:             jogo-da-velha-6987bbdc5c-wgkq9
Namespace:        lab-aiops
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Fri, 08 May 2026 14:20:34 -0300
Labels:           app=jogo-da-velha
                  pod-template-hash=6987bbdc5c
Annotations:      <none>
Status:           Running
IP:               10.244.0.125
IPs:
  IP:           10.244.0.125
Controlled By:  ReplicaSet/jogo-da-velha-6987bbdc5c
Containers:
  jogo-da-velha:
    Container ID:  docker://fa770dead43c8806b215a0c0030d7a1f41dc3b1a126202e3c5fd31222230f786
    Image:         jogo-da-velha:v7
    Image ID:      docker://sha256:9becda1834c48440ef2c640340cbf4eeaf58ff43531d9f3cbd81858336110f8c
    Port:          5000/TCP
    Host Port:     0/TCP
    Command:
      /bin/false
    State:          Waiting
      Reason:       CrashLoopBackOff
    Last State:     Terminated
      Reason:       Error
      Exit Code:    1
      Started:      Fri, 08 May 2026 14:46:33 -0300
      Finished:     Fri, 08 May 2026 14:46:33 -0300
    Ready:          False
    Restart Count:  10
    Limits:
      cpu:     500m
      memory:  128Mi
    Requests:
      cpu:        250m
      memory:     64Mi
    Liveness:     http-get http://:5000/login delay=5s timeout=1s period=10s #success=1 #failure=3
    Readiness:    http-get http://:5000/login delay=2s timeout=1s period=5s #success=1 #failure=3
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-2kshg (ro)
Conditions:
  Type                        Status
  PodReadyToStartContainers   True 
  Initialized                 True 
  Ready                       False 
  ContainersReady             False 
  PodScheduled                True 
Volumes:
  kube-api-access-2kshg:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   Burstable
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  28m                   default-scheduler  Successfully assigned lab-aiops/jogo-da-velha-6987bbdc5c-wgkq9 to minikube
  Normal   Started    12m (x9 over 28m)     kubelet            spec.containers{jogo-da-velha}: Container started
  Normal   Created    7m48s (x10 over 28m)  kubelet            spec.containers{jogo-da-velha}: Container created
  Warning  BackOff    3m37s (x38 over 28m)  kubelet            spec.containers{jogo-da-velha}: Back-off restarting failed container jogo-da-velha in pod jogo-da-velha-6987bbdc5c-wgkq9_lab-aiops(2a809467-66e7-457c-b9aa-aa1f3b2f3a4e)
  Normal   Pulled     2m42s (x11 over 28m)  kubelet            spec.containers{jogo-da-velha}: Container image "jogo-da-velha:v7" already present on machine and can be accessed by the pod
```

---

## Resposta da IA

A IA entregou um diagnóstico completo em duas camadas:

**Visual:** Gerou um diagrama interativo SVG com fluxo de causa raiz
agrupando os eventos em três categorias (Reinicializações repetidas,
Falha no entrypoint, Probes sem chance), convergindo para a causa raiz
e sugerindo ações corretivas com validação final.

**Textual:** Identificou que o campo `command: /bin/false` no manifesto
do Deployment era a causa raiz — `/bin/false` é um utilitário que sempre
retorna exit code 1, encerrando o container antes de qualquer inicialização.
A IA também apontou que:

- A imagem já estava presente no nó, descartando problema de pull
- Os resource limits estavam corretos e não eram a causa
- O ArgoCD continuaria recriando o mesmo pod quebrado em qualquer resync
  enquanto o manifesto não fosse corrigido
- A solução era remover a linha `command` do YAML e fazer push via Git

---

## O que funcionou

- Identificou a causa raiz (`/bin/false`) sem nenhuma dica adicional,
  apenas com a saída do `kubectl describe pod`
- Correlacionou corretamente os três grupos de sintomas (backoff, exit 1,
  probes sem execução) como consequências de uma única falha de configuração
- Considerou o contexto do ArgoCD espontaneamente: explicou por que deletar
  o pod manualmente não resolveria e que o fix precisa acontecer no Git
- Entregou o trecho YAML corretivo pronto para copiar e colar
- O diagrama visual foi gerado automaticamente sem ser pedido,
  facilitando a leitura da cadeia de causalidade

---

## O que não funcionou

- Nenhuma falha crítica identificada neste experimento
- O diagrama SVG usa `sendPrompt()` para aprofundamento de tópicos,
  o que só funciona dentro da interface Claude — não é exportável

---

## Ajustes que eu fiz

- Nenhum ajuste necessário. A resposta de primeira rodada foi suficiente
  para fechar o diagnóstico completo.

---

## Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** A IA leu um único bloco de texto (`kubectl describe pod`),
identificou uma causa raiz não óbvia (`/bin/false` como command intencional
de falha), cruzou com o contexto do ArgoCD sem ser solicitada e entregou
diagnóstico visual + ações corretivas em uma única resposta. Zero iterações
adicionais necessárias.

---

## O que eu faria diferente

Incluir no prompt a saída de `kubectl logs` do container (mesmo que vazia
ou curta) para dar à IA mais contexto sobre o que acontece nos instantes
antes do crash — útil em cenários onde a causa raiz está no código da
aplicação e não no manifesto. Para falhas de configuração de Kubernetes,
o `describe pod` foi suficiente.