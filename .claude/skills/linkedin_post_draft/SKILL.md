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
   - **Nunca invente uma experiência pessoal específica não confirmada**
     (ex: "isso bateu numa auditoria que fiz ano passado", "vi isso quebrar
     num cluster meu"). Falar em termos gerais ("é comum ver isso em
     clusters que...", "muita gente configura errado por padrão...") é
     aceitável; alegar um caso pessoal concreto que não foi verificado com
     o usuário não é — pode ser inverdade e mina a credibilidade do post.
   - Estruture em parágrafos curtos (2-4 linhas cada), com quebra de linha
     dupla entre eles, pra ficar escaneável no feed do LinkedIn.
   - Termine com uma pergunta ou convite à discussão, não com um CTA genérico.
   - Depois da pergunta final, pule uma linha e feche com:
     - **Citação da fonte**: nome da publicação/matéria + link, formato
       `Fonte: <título da matéria> — <url>`.
     - **Hashtags** (máximo 3, só se agregarem): linha própria logo abaixo
       da fonte, ex. `#SRE #PlatformEngineering #AIOps`. Prefira termos já
       usados em `sources/sources.md` ou específicos da ferramenta/tema do
       post em vez de hashtags genéricas demais.

3. **Humanizar**
   - Rode o rascunho pela skill `humanizer` (`.claude/skills/humanizer/SKILL.md`)
     antes de usar.
   - Se o usuário tiver fornecido amostras de posts anteriores dele em
     `sources/voice_samples.md`, use-as para calibração de voz.

4. **Salvar o rascunho**
   - Grave o texto final em `posts/queue/YYYY-MM-DD-tema.md` antes de abrir
     o navegador (assim nada se perde se algo falhar no meio do caminho).

5. **Imagem de capa (sempre buscar, sem perguntar)**
   - Prefira imagem própria do usuário quando existir uma relevante em
     `sources/`.
   - Caso contrário, busque automaticamente uma imagem genérica relacionada
     ao tema em banco de imagem de licença livre (Unsplash/Pexels, licença
     sem exigência de atribuição). Não é necessário perguntar ao usuário
     antes de baixar imagens de banco livre para esse fim — ele já
     autorizou esse fluxo de forma permanente (2026-07-10). Baixe direto.
   - Nunca baixe/reuse diagramas ou fotos proprietárias de blogs/empresas
     de terceiros sem licença clara de reuso — isso continua exigindo
     confirmação explícita do usuário, dado o risco de violação de
     direitos autorais em post público. A autorização automática vale só
     para banco de imagem de licença livre.
   - Baixe a imagem para `~/Downloads/linkedin-posts/YYYY-MM-DD-tema.jpg`
     (criar a pasta se não existir) — nunca para dentro do repo. Esse
     caminho fixo facilita o usuário achar e anexar a imagem manualmente no
     LinkedIn, já que o upload de arquivo abre um diálogo nativo do SO que
     a automação do Chrome não consegue preencher sozinha.
   - Se a busca não achar nada minimamente relevante, siga sem imagem em
     vez de forçar algo genérico demais.

6. **Preencher no LinkedIn via Chrome**
   - Abra `linkedin.com/feed`.
   - Clique em "Começar publicação" / "Start a post".
   - Digite o texto final no campo (digitação simulada, não colar via
     clipboard), incluindo a citação da fonte e as hashtags no final.
   - Se o LinkedIn gerar automaticamente um card de link a partir de algum
     texto/URL da citação da fonte, remova o card (ele costuma linkar pra
     home do site, não pro artigo específico) — deixe só o texto.
   - Se houver imagem baixada: **não tente clicar no ícone de foto/anexar
     mais de uma vez.** Esse botão abre um diálogo nativo do SO que a
     automação do Chrome não consegue enxergar nem preencher — tentar de
     novo (ou checar `input[type=file]` via JS) não resolve, só reabre o
     diálogo e trava a automação. Tente no máximo 1 vez para confirmar que
     não há um input acessível; se não houver, desista imediatamente e só
     informe o caminho do arquivo em `~/Downloads/linkedin-posts/` pro
     usuário anexar manualmente.
   - **Pare aqui.** Não clique em "Publicar".

7. **Avisar o usuário**
   - Mensagem final: "Post preenchido no LinkedIn, aba aberta. Revise e
     clique em Publicar quando quiser."
   - Registre em `logs/published.csv` a linha do post como `status=draft`;
     o usuário (ou uma execução futura) atualiza para `status=published`
     depois de conferir manualmente.

## Cadência

Pensado para rodar até 3x por semana (ex: segunda, quarta, sexta). Não
execute mais de uma vez no mesmo dia sem pedido explícito do usuário.
