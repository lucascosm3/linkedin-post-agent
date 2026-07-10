# Fontes de temas

Usadas pela skill `linkedin_post_draft` para escolher o assunto do dia.
Priorize 1 fonte de notícia + Hacker News filtrado + 1 fonte de IA aplicada
a operações, para variar o ângulo do post.

## Curadoria geral / agregadores

- **Hacker News** — https://news.ycombinator.com/
  API pública, sem necessidade de scraping: `https://hn.algolia.com/api/v1/search?query=<termo>&tags=story`
  Termos sugeridos: `kubernetes`, `terraform`, `sre`, `observability`, `platform engineering`,
  `incident response`, `aiops`, `ebpf`, `argocd`.
- **SRE Weekly** — https://sreweekly.com (newsletter semanal: incidentes, postmortems, automação)
- **Ship It Weekly** — https://rss.com/podcasts/ship-it-weekly/ (recap de outages, releases, ferramentas)
- **The New Stack** — https://thenewstack.io (cloud-native, cruza bastante SRE + IA)

## Cloud / infraestrutura

- **Last Week in AWS** (Corey Quinn) — https://www.lastweekinaws.com
- **Amazon EKS Newsletter** — atualizações de Kubernetes/EKS direto da AWS
- **Google Cloud DevOps & SRE Blog** — https://cloud.google.com/blog/products/devops-sre

## IA aplicada a SRE / DevOps / Platform Engineering (AIOps)

- **awesome-ai-sre (GitHub)** — https://github.com/agamm/awesome-ai-sre
  Lista curada de 100+ ferramentas de IA para SRE (agentes, observabilidade, AIOps,
  chaos engineering). Boa fonte de "ferramenta nova" para comentar.
- **incident.io Blog** — https://incident.io/blog/sre-ai-tools-transform-devops-2025
- **The New Stack — AI DevOps vs. SRE agents** — https://thenewstack.io/ai-devops-vs-sre-agents-compare-ai-incident-response-tools/

## Regra de uso

Nunca usar só uma manchete solta como tema — cruzar com sua vivência real
(AWS/EKS/Terraform, ArgoCD/GitLab CI, Vault, migrações de Ingress, builds
ARM64/Graviton) para dar um ângulo pessoal, não só repetir a notícia.
