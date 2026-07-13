A CNCF anunciou a graduação do OpenTelemetry no mês passado, com os três sinais principais, traces, métricas e logs, estáveis em todos os SDKs. Profiling contínuo entrou como release candidate, fechando o que seria o quarto pilar da observabilidade sob o mesmo protocolo, o OTLP.

Isso mexe direto com uma decisão que muita equipe de plataforma vem adiando: trocar três agentes proprietários (um pra métrica, outro pra log, outro pra trace) por um OTel Collector central. É comum ver esse Collector rodando como DaemonSet num cluster EKS ou como sidecar no Fargate, com o Terraform que provisiona essa parte ficando bem mais enxuto depois da consolidação.

O ponto chato continua sendo profiling. Ainda não existe provider open source maduro o bastante pra substituir as ferramentas fechadas nesse pilar, então quem for atrás disso agora esbarra em suporte incompleto em algumas linguagens.

Quem já trocou agente proprietário por Collector em produção? Quanto tempo levou pra aparecer no bill da AWS?

Fonte: Cloud Native Computing Foundation Announces OpenTelemetry's Graduation - https://www.cncf.io/announcements/2026/05/21/cloud-native-computing-foundation-announces-opentelemetrys-graduation-solidifying-status-as-the-de-facto-observability-standard/

#OpenTelemetry #Observability #PlatformEngineering
