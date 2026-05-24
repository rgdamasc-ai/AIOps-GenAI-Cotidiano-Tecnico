# Step-Back Prompting

**Dica prática**: você pode comparar a qualidade da resposta executando o mesmo cenário com e sem a parte do Step-Back, para sentir a diferença de profundidade na análise.

---

## Prompt 1 — Compra de carro com princípios de TCO

Prompt da demo não-técnica (Passo 1). Antes de recomendar um modelo, o modelo é forçado a mapear os princípios de Total Cost of Ownership (TCO) — depreciação, consumo, manutenção, seguro, revenda. Resultado: recomendação baseada em fundamentos, não em popularidade.

```
Preciso comprar um carro com orçamento de até R$100.000. É pra uso urbano, faço uns 30km por dia no trânsito de São Paulo. Qual carro você recomenda?

Antes de recomendar, dê um passo atrás e responda:
Quais são os princípios fundamentais para avaliar o custo total de propriedade (TCO) de um veículo para uso urbano? Considere depreciação, consumo, manutenção, seguro e revenda.

Depois, use esses princípios para recomendar o melhor carro para o meu cenário. Considere carro a combustão e elétrico
```

---

## Prompt 2 — Diagnóstico de CrashLoopBackOff com princípios de memória em containers

Prompt da Demo Técnica 1 (Passo 2). Pod em CrashLoopBackOff com OOMKilled. Antes de diagnosticar, o modelo abstrai os princípios de gerenciamento de memória em K8s (requests vs limits, OOM Killer, cgroups). Resultado: o modelo questiona se é under-provisioning ou memory leak em vez de simplesmente sugerir aumentar o limit.

```
Um pod payment-service está em CrashLoopBackOff no Kubernetes. Os logs mostram:

Container killed due to OOMKilled (exit code 137)
Last state: terminated with exit code 137
Restart count: 5
Resource limits: memory 256Mi
Current usage before crash: 312Mi

Antes de diagnosticar, dê um passo atrás e responda:
Quais são os princípios fundamentais de gerenciamento de memória em containers Kubernetes? Como requests, limits e OOM Killer interagem?

Depois, use esses princípios para diagnosticar o problema e sugerir soluções.
```

---

## Prompt 3 — Conectividade entre serviços com princípios de rede em VPC

Prompt da Demo Técnica 2 (Passo 3). Dois serviços em subnets diferentes de uma mesma VPC AWS não se conectam, mas o ping funciona. Antes de diagnosticar, o modelo mapeia as camadas de controle de tráfego em VPC. Resultado: diagnóstico sistemático por camada, eliminando hipóteses em vez de tentativa e erro.

```
O serviço order-service (subnet privada 10.0.1.0/24) não consegue se conectar ao serviço inventory-service (subnet privada 10.0.2.0/24) na porta 8080. Ambos estão na mesma VPC. O security group do inventory-service permite inbound na porta 8080 de 10.0.1.0/24. O ping entre as instâncias funciona normalmente.

Antes de diagnosticar, dê um passo atrás e responda:
Quais são as camadas de controle de tráfego em uma VPC AWS? Como Security Groups, NACLs, Route Tables e DNS resolution interagem em comunicação entre subnets?

Depois, use esses princípios para diagnosticar o problema de forma sistemática.
```

---

## Caderno de Experimentos

---

### Experimento 01 — Compra de carro com princípios de TCO

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Revisão

#### Contexto

Teste de Step-Back para recomendação de compra de carro: antes de recomendar, o modelo mapeia os princípios de Total Cost of Ownership (TCO), evitando recomendação baseada em popularidade ou preferência.

#### Prompt usado

```
Preciso comprar um carro com orçamento de até R$100.000. É pra uso urbano, faço uns 30km por dia no trânsito de São Paulo. Qual carro você recomenda?

Antes de recomendar, dê um passo atrás e responda:
Quais são os princípios fundamentais para avaliar o custo total de propriedade (TCO) de um veículo para uso urbano? Considere depreciação, consumo, manutenção, seguro e revenda.

Depois, use esses princípios para recomendar o melhor carro para o meu cenário. Considere carro a combustão e elétrico
```

#### Resposta da IA

Princípios de TCO mapeados explicitamente antes da recomendação; recomendação baseada em dados de cada componente de custo para o perfil específico (30km/dia, trânsito SP).

#### Output completo da IA

**Fase 1 — Princípios de TCO para uso urbano:**

1. **Depreciação:** Maior custo oculto. Carros perdem 15-25% no primeiro ano. Modelos com alta demanda no mercado usado depreciam menos. Elétricos têm depreciação maior no Brasil pela incerteza do mercado de baterias.

2. **Consumo:** Em SP, trânsito parado penaliza combustão (para-arranca). 30km/dia = ~700km/mês. Carro com 10 km/L gasta 70L/mês (~R$490/mês em gasolina a R$7). Elétrico gasta ~R$140/mês na mesma distância (tarifa residencial).

3. **Manutenção:** Elétrico tem manutenção 40-60% menor (sem troca de óleo, menos desgaste de freios por regeneração). Combustão exige revisões a cada 10.000km. Custo anual médio: R$2.500-4.000 para combustão, R$800-1.500 para elétrico.

4. **Seguro:** Varia por modelo, valor e perfil. Carros populares têm seguro mais barato. Elétricos têm seguro 20-40% mais caro no Brasil pela dificuldade de peças e conserto.

5. **Revenda:** Combustão tem mercado maduro e previsível. Elétrico tem liquidez menor no Brasil — menor base de compradores para revenda.

**Fase 2 — Recomendação:**

Para 30km/dia no trânsito de SP:

**Combustão:** Toyota Corolla Cross (R$95.000) — depreciação baixa, confiabilidade alta, seguro moderado. Custo total 5 anos estimado: R$180.000 (incluindo combustível, seguro, manutenção).

**Elétrico:** BYD Dolphin Mini (R$99.000) — economia de R$350/mês em combustível vs. gasolina. Custo total 5 anos estimado: R$165.000. Vantagem: trânsito SP favorece o elétrico (regeneração em para-arranca).

**Recomendação final:** BYD Dolphin Mini para perfil urbano intenso em SP. A economia em combustível e manutenção compensa o seguro mais caro e a depreciação maior no horizonte de 5 anos.

#### O que funcionou

- Princípios de TCO mapeados com dados quantitativos (não genéricos)
- Cálculo de consumo real para o perfil específico (30km/dia, SP)
- Análise comparativa elétrico vs combustão baseada nos princípios, não em preferência
- Recomendação com justificativa auditável

#### O que não funcionou

- Depreciação do elétrico estimada sem dados de mercado brasileiro concretos (mercado ainda em formação)
- Custo total de 5 anos estimado sem discriminar cada componente individualmente

#### Ajustes que eu fiz

- Em uso real: adicionar ao prompt "use dados de 2024-2025 para depreciação e seguro" e "discrimine o custo total por componente (combustível, manutenção, seguro, depreciação)".

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Step-Back evitou a armadilha comum de recomendar pelo nome/popularidade. A fase de princípios de TCO estruturou o raciocínio e tornou a recomendação do elétrico surpreendente mas justificada — sem o Step-Back, o modelo teria recomendado um Honda Civic ou HRV pelo nome.

#### O que eu faria diferente

Pedir ao modelo que compare o TCO em 3 anos vs 5 anos, já que o perfil de uso (30km/dia) pode tornar o elétrico mais atrativo em prazos mais longos e penalizar a comparação em prazos curtos.

---

### Experimento 02 — Diagnóstico de CrashLoopBackOff com princípios de memória em containers

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Teste de Step-Back técnico: pod em CrashLoopBackOff com OOMKilled. Antes de diagnosticar, o modelo abstrai os princípios de gerenciamento de memória em Kubernetes, evitando a sugestão simplista de "aumente o limit".

#### Prompt usado

```
Um pod payment-service está em CrashLoopBackOff no Kubernetes. Os logs mostram:

Container killed due to OOMKilled (exit code 137)
Last state: terminated with exit code 137
Restart count: 5
Resource limits: memory 256Mi
Current usage before crash: 312Mi

Antes de diagnosticar, dê um passo atrás e responda:
Quais são os princípios fundamentais de gerenciamento de memória em containers Kubernetes? Como requests, limits e OOM Killer interagem?

Depois, use esses princípios para diagnosticar o problema e sugerir soluções.
```

#### Resposta da IA

Princípios de memória em K8s mapeados explicitamente (requests vs limits, cgroups, OOM Killer); diagnóstico que diferencia under-provisioning de memory leak em vez de simplesmente sugerir aumentar o limit.

#### Output completo da IA

**Fase 1 — Princípios de gerenciamento de memória em Kubernetes:**

1. **Requests vs Limits:** `requests` é o mínimo garantido para o scheduler alocar o pod em um nó. `limits` é o máximo — se ultrapassado, o container é candidato a OOM Killer.

2. **cgroups e OOM Killer:** Linux usa cgroups para isolar recursos por container. Quando o container ultrapassa o `memory limit`, o kernel OOM Killer intervém e mata o processo (exit code 137). Isso não é um crash da aplicação — é uma decisão do kernel.

3. **Under-provisioning vs Memory Leak:** Há duas causas possíveis para OOMKilled:
   - **Under-provisioning:** o limit está abaixo da necessidade legítima da aplicação
   - **Memory leak:** a aplicação cresce indefinidamente e nenhum limit seria suficiente a longo prazo

4. **Restart policy:** O Kubernetes reinicia o container após OOMKilled (CrashLoopBackOff), mas sem investigar a causa. Os restarts acumulam até o backoff exponencial.

5. **Identificação:** Se a memória crescia gradualmente até 312Mi e depois crashou = possível leak. Se atingiu 312Mi imediatamente após o start = under-provisioning.

**Fase 2 — Diagnóstico:**

Dados: limit=256Mi, uso=312Mi no crash (56Mi acima do limit). 5 restarts.

**Diagnóstico diferenciado:**
- Se os logs mostram crescimento gradual de memória → **memory leak** (aumentar o limit é paliativo, não resolve)
- Se o uso atingiu 312Mi no primeiro start → **under-provisioning** (aumentar o limit resolve)

**Ações:**
1. Verificar métricas históricas de memória (Prometheus/Datadog) para identificar o padrão
2. Se leak: adicionar heap dump antes do crash (`-XX:+HeapDumpOnOutOfMemoryError` para JVM)
3. Se under-provisioning: aumentar limit para 512Mi e ajustar request para 256Mi
4. Adicionar VPA (Vertical Pod Autoscaler) para ajuste automático de resources

#### O que funcionou

- Distinção crítica entre under-provisioning e memory leak — sem Step-Back, a sugestão seria "aumente o limit"
- Princípio 3 (identificação pelo padrão de crescimento) foi o mais valioso para o diagnóstico
- Ações diferenciadas para cada causa (heap dump vs ajuste de limit)

#### O que não funcionou

- Sem logs históricos de memória, o diagnóstico ficou condicional ("se crescia gradualmente... se atingiu imediatamente...")
- Não mencionou `kubectl top pod` ou `kubectl describe pod` como comandos imediatos de investigação

#### Ajustes que eu fiz

- Em uso real: adicionar ao prompt "inclua os comandos kubectl para investigação imediata" e fornecer os logs de inicialização do pod para eliminar a condicionalidade do diagnóstico.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Step-Back foi essencial aqui. Sem os princípios de memória em K8s mapeados primeiro, o diagnóstico seria "aumente o limit de 256Mi para algo maior". Com Step-Back, o modelo questionou se é leak ou under-provisioning — que é a pergunta correta para este cenário.

#### O que eu faria diferente

Adicionar ao prompt: "após mapear os princípios, liste os 3 comandos kubectl mais importantes para coletar evidências antes de concluir o diagnóstico". Isso tornaria a análise diretamente acionável durante o troubleshooting.

---

### Experimento 03 — Conectividade entre serviços com princípios de rede em VPC

**Data:** 2026-05-23
**Ferramenta usada:** Claude Code
**Modelo:** Claude Sonnet 4.6
**Categoria:** Troubleshooting

#### Contexto

Teste de Step-Back técnico mais complexo: dois serviços em subnets diferentes de uma VPC AWS não se conectam, mas o ping funciona. Antes de diagnosticar, o modelo mapeia as camadas de controle de tráfego em VPC.

#### Prompt usado

```
O serviço order-service (subnet privada 10.0.1.0/24) não consegue se conectar ao serviço inventory-service (subnet privada 10.0.2.0/24) na porta 8080. Ambos estão na mesma VPC. O security group do inventory-service permite inbound na porta 8080 de 10.0.1.0/24. O ping entre as instâncias funciona normalmente.

Antes de diagnosticar, dê um passo atrás e responda:
Quais são as camadas de controle de tráfego em uma VPC AWS? Como Security Groups, NACLs, Route Tables e DNS resolution interagem em comunicação entre subnets?

Depois, use esses princípios para diagnosticar o problema de forma sistemática.
```

#### Resposta da IA

Quatro camadas de controle de tráfego mapeadas explicitamente; diagnóstico sistemático por eliminação que chegou a NACL como hipótese mais provável dado que ping funciona mas TCP/8080 não.

#### Output completo da IA

**Fase 1 — Camadas de controle de tráfego em VPC:**

1. **Security Groups (stateful):** Filtram tráfego por instância. Stateful = resposta automática permitida. Regras de inbound e outbound independentes.

2. **NACLs (stateless):** Filtram tráfego por subnet. Stateless = regras de inbound e outbound devem ser explícitas nos dois sentidos. Avaliadas por número (menor = maior prioridade). Aplicadas antes dos Security Groups.

3. **Route Tables:** Definem como o tráfego é roteado entre subnets e para a internet. Se não há rota entre subnets, o tráfego não chega ao destino independente dos Security Groups.

4. **DNS Resolution:** AWS VPC DNS resolve internamente para IPs privados. Se o serviço usa nome DNS em vez de IP direto, verificar se o hostname resolve para o IP correto na subnet destino.

**Interação:** Tráfego percorre Route Table → NACL de origem → Security Group de origem → rede → NACL de destino → Security Group de destino. Qualquer camada pode bloquear.

**Fase 2 — Diagnóstico sistemático:**

Ping funciona mas TCP/8080 não. Isso elimina algumas hipóteses:

**Eliminados:**
- Route Table: ping funciona = há rota entre subnets
- Security Group do inventory-service: já configurado corretamente (inbound 8080 de 10.0.1.0/24)
- DNS: se ping funciona por hostname, DNS está correto

**Hipóteses mais prováveis (do mais ao menos provável):**

1. **NACL bloqueando TCP outbound na subnet 10.0.1.0/24 (origem):** NACLs são stateless — mesmo com inbound liberado no destino, se o outbound da subnet de origem bloquear porta 8080, a conexão TCP não é estabelecida. Ping usa ICMP (protocolo diferente), por isso passa.

2. **NACL bloqueando TCP inbound na subnet 10.0.2.0/24 (destino):** Mesma lógica — NACL da subnet destino pode ter regra DENY implícita para TCP na porta 8080.

3. **Security Group do order-service bloqueando outbound:** Verificar se o SG do order-service tem regra outbound para 10.0.2.0/24:8080. Security Groups têm outbound ALLOW all por padrão, mas pode ter sido modificado.

**Ação imediata:**
```bash
# Verificar NACL da subnet 10.0.1.0/24
aws ec2 describe-network-acls --filters "Name=association.subnet-id,Values=subnet-XXXX"

# Verificar outbound do SG do order-service
aws ec2 describe-security-groups --group-ids sg-XXXX
```

#### O que funcionou

- Mapeamento das 4 camadas permitiu eliminação sistemática das hipóteses
- Distinção ICMP (ping) vs TCP (porta 8080) foi a chave do diagnóstico — sem Step-Back dos princípios de rede, essa distinção seria menos evidente
- NACL stateless como hipótese principal emergiu diretamente do princípio mapeado
- Comandos AWS CLI incluídos para investigação imediata

#### O que não funcionou

- Hipótese de "serviço não está ouvindo na porta 8080" não foi mencionada — o inventory-service pode estar ouvindo em 0.0.0.0 ou apenas em 127.0.0.1
- Sem verificar se é TCP ou UDP (a porta 8080 pode usar ambos dependendo da aplicação)

#### Ajustes que eu fiz

- Em uso real: adicionar ao prompt "inclua também hipóteses no lado da aplicação (porta de binding, firewall interno do OS)" para cobrir as camadas abaixo da VPC.

#### Nota (1 a 5)

**Nota:** 5/5

**Justificativa:** Step-Back transformou o diagnóstico de "tentativa e erro nas configurações" em eliminação sistemática por camada. A identificação de NACL como hipótese principal (por causa do comportamento diferente do ICMP vs TCP) só foi possível porque os princípios foram mapeados explicitamente antes.

#### O que eu faria diferente

Incluir no prompt uma camada 5: "aplicação/OS — verifique se o serviço está ouvindo no endereço correto (0.0.0.0 vs 127.0.0.1) e se o firewall do OS (iptables/nftables) está interferindo". Isso completaria o diagnóstico em todas as camadas do stack.
