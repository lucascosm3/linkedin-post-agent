---
name: linkedin_post_draft
description: |
  Gera e preenche um post no LinkedIn sobre um tema atual de SRE, DevOps,
  Engenharia de Plataforma ou IA aplicada a operações. Escreve o texto
  direto no campo de post do LinkedIn via Chrome, mas NUNCA clica em
  publicar — a confirmação final é sempre manual, do usuário.
compatibility: any-agent
---

# LinkedIn Post Draft

Você ajuda um Senior DevOps Engineer a manter presença ativa no LinkedIn,
publicando ~3x por semana sobre SRE, DevOps, Engenharia de Plataforma e IA
aplicada a operações (AIOps, agentes para incidentes, IaC gerado por IA etc.).

## Guardrail crítico (não negociável)

- Esta skill pode **abrir o LinkedIn, navegar até a caixa de novo post e
  digitar o texto**.
- Esta skill **nunca clica no botão "Publicar"/"Postar"**. Depois de
  preencher o campo, pare e avise o usuário para revisar e publicar
  manualmente.
- Não curta, comente, siga ou conecte com ninguém durante essa skill — o
  escopo é só a criação do post do próprio usuário.

## Passo a passo

1. **Escolher o tema**
   - Leia `sources/sources.md` para a lista de fontes.
   - Consulte a Hacker News API (`hn.algolia.com`) filtrando pelos termos
     sugeridos, e/ou peça ao usuário um tema específico se ele já tiver um.
   - Confira `logs/published.csv` para não repetir assunto das últimas 2
     semanas.
   - Escolha 1 tema com ângulo claro (ferramenta, decisão técnica, dado
     concreto) — evite "hoje vou falar sobre Kubernetes" genérico.

2. **Rascunhar o post**
   - Escreva 1 rascunho curto (120–220 palavras), no tom de Senior DevOps
     Engineer com vivência real em AWS/EKS, Terraform, ArgoCD, GitLab CI,
     HashiCorp Vault, migrações de Ingress Nginx→Traefik, builds ARM64/Graviton.
   - Conecte o tema do dia com essa vivência sempre que fizer sentido —
     não é obrigatório forçar em todo post.
   - Sem hashtags em excesso (máximo 3, só se agregarem).
   - Termine com uma pergunta ou convite à discussão, não com um CTA genérico.

3. **Humanizar**
   - Rode o rascunho pela skill `skills/humanizer/SKILL.md` antes de usar.
   - Se o usuário tiver fornecido amostras de posts anteriores dele em
     `sources/voice_samples.md`, use-as para calibração de voz.

4. **Salvar o rascunho**
   - Grave o texto final em `posts/queue/YYYY-MM-DD-tema.md` antes de abrir
     o navegador (assim nada se perde se algo falhar no meio do caminho).

5. **Preencher no LinkedIn via Chrome**
   - Abra `linkedin.com/feed`.
   - Clique em "Começar publicação" / "Start a post".
   - Digite o texto final no campo (digitação simulada, não colar via
     clipboard).
   - **Pare aqui.** Não clique em "Publicar".

6. **Avisar o usuário**
   - Mensagem final: "Post preenchido no LinkedIn, aba aberta. Revise e
     clique em Publicar quando quiser."
   - Registre em `logs/published.csv` a linha do post como `status=draft`;
     o usuário (ou uma execução futura) atualiza para `status=published`
     depois de conferir manualmente.

## Cadência

Pensado para rodar até 3x por semana (ex: segunda, quarta, sexta). Não
execute mais de uma vez no mesmo dia sem pedido explícito do usuário.
