Cilium virou a opção padrão, ou pelo menos a recomendada, de CNI em GKE, EKS e AKS, e o motivo é sempre o mesmo: eBPF no lugar do kube-proxy baseado em iptables. Isso muda como o roteamento pod-to-pod funciona por baixo, sem depender de milhares de regras de NAT acumuladas conforme o número de Services cresce.

Quem já operou um EKS grande conhece o problema: iptables escala mal, debugar regra de NAT é sofrimento, e cada Service novo é mais uma cadeia pra percorrer. Trocar isso por eBPF resolve boa parte da performance, mas também abre espaço pra Network Policy em L7 e visibilidade que antes só vinha com um service mesh separado.

O que me chama atenção não é a tecnologia em si, é a migração. Trocar CNI num cluster em produção não é como trocar Ingress Controller. Toca em roteamento, política de rede e, às vezes, em como o kube-dns resolve serviço interno. Não é drop-in.

Quem já fez essa migração de kube-proxy pra Cilium num cluster grande, o que mais doeu: a janela de corte ou a validação de política de rede depois?

Fonte: eBPF in 2026: The Kernel Revolution Powering Cloud-Native Security and Observability - https://dev.to/linou518/ebpf-in-2026-the-kernel-revolution-powering-cloud-native-security-and-observability-22jd
#eBPF #Kubernetes #PlatformEngineering
