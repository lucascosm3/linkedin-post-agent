---
name: linkedin_post_draft
description: |
  Gera e preenche um post no LinkedIn sobre um tema atual de SRE, DevOps,
  Engenharia de Plataforma ou IA aplicada a operações. Levanta algumas
  notícias/temas candidatos do dia e deixa o usuário escolher qual vira o
  post antes de escrever o rascunho. Escreve o texto direto no campo de
  post do LinkedIn via Chrome, mas NUNCA clica em publicar — a
  confirmação final é sempre manual, do usuário. Também atende ao pedido
  de "preencher o próximo rascunho no LinkedIn" / "postar o próximo
  rascunho", que pega um rascunho já gerado (por essa skill local ou pela
  rotina agendada na nuvem) e só faz o preenchimento.
compatibility: any-agent
---

# LinkedIn Post Draft

Você ajuda um Senior DevOps Engineer a manter presença ativa no LinkedIn,
publicando ~3x por semana sobre SRE, DevOps, Engenharia de Plataforma e IA
aplicada a operações (AIOps, agentes para incidentes, IaC gerado por IA etc.).

## Um repositório só

Código, skills e conteúdo de post (`posts/queue/*.md`, `posts/queue/images/`,
`logs/published.csv`) vivem todos neste mesmo repositório
(`linkedin-post-agent`), que é **público**. O usuário está ciente e topou
que os rascunhos fiquem visíveis no histórico do Git (decisão de
2026-07-10) — não pergunte de novo por causa disso.

## Guardrail crítico (não negociável)

- Esta skill pode **abrir o LinkedIn, navegar até a caixa de novo post e
  digitar o texto**.
- Esta skill **nunca clica no botão "Publicar"/"Postar"**. Depois de
  preencher o campo, pare e avise o usuário para revisar e publicar
  manualmente.
- Não curta, comente, siga ou conecte com ninguém durante essa skill — o
  escopo é só a criação do post do próprio usuário.

## Modo A — Gerar um rascunho novo

Use quando não há rascunho pendente, ou o usuário pede um tema específico.

1. **Escolher o tema**
   - Leia `sources/sources.md` para a lista de fontes.
   - Consulte a Hacker News API (`hn.algolia.com`) filtrando pelos termos
     sugeridos, e/ou peça ao usuário um tema específico se ele já tiver um.
   - Confira `logs/published.csv` para não repetir assunto das últimas 2
     semanas.
   - Levante de 3 a 5 notícias/temas candidatos do dia, cada um com ângulo
     claro (ferramenta, decisão técnica, dado concreto) — evite "hoje vou
     falar sobre Kubernetes" genérico.
   - **Apresente as opções ao usuário e espere ele escolher** antes de
     escrever o rascunho — não escolha sozinho. Liste título curto + fonte
     de cada candidato (ex: uma lista numerada) para ele bater o olho e
     responder com o número/tema desejado. Só pule essa pergunta se o
     usuário já tiver pedido um tema específico de antemão.

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
     o navegador.
   - Imagem de capa (sempre buscar, sem perguntar): se não houver imagem
     própria relevante em `sources/`, busque uma imagem genérica de banco
     de licença livre (Unsplash/Pexels, sem exigência de atribuição) e
     baixe pra `posts/queue/images/YYYY-MM-DD-tema.jpg`. Não precisa
     perguntar — já autorizado permanentemente (2026-07-10). Nunca baixe
     de blog/empresa terceira sem licença clara (aí sim precisa
     perguntar). Se não achar nada relevante, siga sem imagem.
   - Adicione a linha em `logs/published.csv` com `status=draft`.
   - `git add`, `git commit`, `git push`.

5. Siga direto para **"Preencher no LinkedIn"** abaixo.

## Modo B — Preencher o próximo rascunho pendente

Gatilhos em linguagem natural: "preenche o próximo rascunho no LinkedIn",
"posta o próximo rascunho", "roda o fluxo de postar". Use quando já existe
rascunho pronto (gerado pela rotina na nuvem ou por uma execução anterior
do Modo A) esperando ser preenchido no navegador.

1. `git pull`.
2. Leia `logs/published.csv`, ache a linha mais antiga com `status=draft`.
   Se não houver nenhuma, avise o usuário e ofereça rodar o Modo A.
3. Abra o `.md` correspondente em `posts/queue/`.
4. **Revise antes de preencher** — mesmo critérios do Modo A: sem
   experiência pessoal inventada, estrutura em parágrafos curtos, citação
   da fonte, até 3 hashtags. Se algo estiver fora do padrão (ex: a rotina
   na nuvem gerou algo que quebra alguma dessas regras), corrija o
   arquivo, `git commit` + `git push` a correção antes de preencher.
5. Confira se existe imagem em `posts/queue/images/` pro mesmo tema, ou em
   `~/Downloads/linkedin-posts/` (caso tenha sido gerada numa execução
   local anterior).
6. Siga para **"Preencher no LinkedIn"** abaixo.
7. Depois de preencher, atualize `status=draft` para `status=filled` na
   linha correspondente de `logs/published.csv`, `git commit` + `git push`.
   (O usuário ainda decide se publica; `filled` só indica "já preenchido
   no LinkedIn, aguardando revisão humana".)

## Preencher no LinkedIn via Chrome

- Abra `linkedin.com/feed`.
- Clique em "Começar publicação" / "Start a post".
- Digite o texto final no campo (digitação simulada, não colar via
  clipboard), incluindo a citação da fonte e as hashtags no final.
- Se o LinkedIn gerar automaticamente um card de link a partir de algum
  texto/URL da citação da fonte, remova o card (ele costuma linkar pra
  home do site, não pro artigo específico) — deixe só o texto.
- Se houver imagem (em `posts/queue/images/` ou em
  `~/Downloads/linkedin-posts/`): **não tente clicar no ícone de
  foto/anexar mais de uma vez.** Esse botão abre um diálogo nativo do SO
  que a automação do Chrome não consegue enxergar nem preencher — tentar
  de novo não resolve, só reabre o diálogo e trava a automação. Tente no
  máximo 1 vez para confirmar que não há um input acessível; se não
  houver, desista imediatamente e só informe o caminho do arquivo pro
  usuário anexar manualmente.
- **Pare aqui.** Não clique em "Publicar".

## Avisar o usuário

Mensagem final: "Post preenchido no LinkedIn, aba aberta. Revise e clique
em Publicar quando quiser." Inclua o caminho da imagem, se houver, pra
anexar manualmente.

## Cadência

Geração de rascunho (Modo A) pensada pra até 3x por semana (ex: segunda,
quarta, sexta) — também coberta pela rotina agendada na nuvem
(`claude.ai/code/routines`, trigger "LinkedIn post draft - SRE/DevOps/AIOps"),
que já roda nesses dias sozinha e deixa o rascunho pronto no repo. Não
execute o Modo A mais de uma vez no mesmo dia sem pedido explícito do
usuário. O Modo B (preencher) pode ser pedido a qualquer momento, sem
limite de frequência — é só preenchimento, não geração.
