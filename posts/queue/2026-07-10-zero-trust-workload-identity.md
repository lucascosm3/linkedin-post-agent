<!--
tema: Zero-Trust Workload Identity em Kubernetes com SPIFFE, SPIRE e Cilium
fonte: https://www.freecodecamp.org/news/implement-zero-trust-workload-identity-in-kubernetes-with-spiffe-spire-and-cilium/
gerado: 2026-07-10
status: draft
-->

Passei os últimos meses tirando certificados estáticos de dentro de pods, um por um, e trocando por identidade de workload de verdade. Essa semana vi um artigo bom sobre SPIFFE e SPIRE combinados com Cilium pra isso, e bateu direto com o que fizemos.

O problema de sempre: service account token ou certificado hardcoded no Secret, válido por um ano, ninguém sabe rotacionar sem derrubar o serviço. Rodamos algo parecido com HashiCorp Vault fazendo emissão de certificados de curta duração, mas o ponto que o artigo levanta e eu concordo é que sem um identity provider dedicado tipo SPIRE, cada time acaba reinventando essa rotação do jeito dele, com scripts diferentes, prazos diferentes, sem padrão.

Cilium entrando no meio pra fazer enforcement de rede baseado nessa identidade (não só IP ou namespace) resolve outro ponto que sempre foi frágil pra mim: policy de rede sobrevive a um pod sendo recriado com IP novo, porque a identidade não muda.

Já alguém migrou uma malha inteira pra esse modelo em produção? Quero saber quanto tempo levou e o que quebrou no meio do caminho.

Fonte: How to Implement Zero-Trust Workload Identity in Kubernetes with SPIFFE, SPIRE, and Cilium — https://www.freecodecamp.org/news/implement-zero-trust-workload-identity-in-kubernetes-with-spiffe-spire-and-cilium/
#Kubernetes #ZeroTrust #PlatformEngineering
