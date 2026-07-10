# linkedin-post-agent

Projeto para ajudar a manter presença ativa no LinkedIn (~3x/semana), com
posts sobre SRE, DevOps, Engenharia de Plataforma e IA aplicada a
operações (AIOps). Roda localmente via Claude Code e também tem uma
rotina agendada na nuvem que prepara os rascunhos sozinha.

## Como funciona

1. A skill `linkedin_post_draft` (`.claude/skills/linkedin_post_draft/SKILL.md`)
   escolhe um tema atual a partir das fontes em `sources/sources.md`
   (Hacker News filtrado, SRE Weekly, The New Stack, awesome-ai-sre etc.),
   evitando repetir assunto das últimas 2 semanas (`logs/published.csv`).
2. Gera um rascunho de post conectando o tema com sua vivência real em
   AWS/EKS, Terraform, ArgoCD, GitLab CI, Vault etc., estruturado em
   parágrafos curtos e fechado com citação da fonte e até 3 hashtags.
3. Passa o texto pela skill `humanizer`
   (`.claude/skills/humanizer/SKILL.md`, de
   [blader/humanizer](https://github.com/blader/humanizer)) para remover
   marcas típicas de texto gerado por IA.
4. Busca uma imagem de capa em banco de licença livre (Unsplash/Pexels)
   relacionada ao tema, quando não há imagem própria em `sources/`.
5. Salva o rascunho em `posts/queue/`.
6. Abre o LinkedIn no Chrome, digita o post no campo de nova publicação e
   **para** — a confirmação de publicar é sempre manual. A imagem baixada
   fica em `~/Downloads/linkedin-posts/` pra anexar manualmente, já que o
   upload de arquivo abre um diálogo nativo do SO que a automação do
   Chrome não alcança.

**Nada nesta skill clica em "Publicar", "Conectar" ou "Seguir" sozinho.**
O único passo automatizado no navegador é escrever o rascunho na caixa de
post; você decide se publica e anexa a imagem.

> **Este repositório é público**, e os rascunhos (`posts/queue/`,
> `logs/published.csv`) ficam versionados nele — decisão consciente
> (2026-07-10) pra manter tudo num lugar só, já que o conteúdo sempre
> passa por revisão antes de ir pro LinkedIn de qualquer forma.

## Uso local

Com o Claude Code em modo Chrome, dentro desta pasta:

```
cd linkedin-post-agent
claude --chrome
```

Como as skills estão em `.claude/skills/`, o Claude Code já lista
`linkedin_post_draft` e `humanizer` como skills invocáveis nessa sessão
(escopo local ao projeto). `.claude/settings.json` já libera as
permissões necessárias pro fluxo (WebFetch/WebSearch dos domínios usados,
download de imagem livre, git, gh, tools de browser), então o fluxo roda
sem prompts de aprovação.

Dois modos, em linguagem natural:

- **Gerar um rascunho novo**: "roda a skill do LinkedIn" ou peça um tema
  específico.
- **Preencher um rascunho já pronto** (gerado por você ou pela rotina na
  nuvem): "preenche o próximo rascunho no LinkedIn" / "posta o próximo
  rascunho". Puxa o `git pull`, pega a entrada mais antiga com
  `status=draft` em `logs/published.csv`, revisa contra o padrão da skill
  e preenche.

## Rotina agendada (nuvem)

Existe uma rotina de cron ("LinkedIn post draft - SRE/DevOps/AIOps",
seg/qua/sex às 12h horário de Fortaleza) rodando em
`claude.ai/code/routines` sobre este repositório
(github.com/lucascosm3/linkedin-post-agent). Ela roda isolada na nuvem,
então:

- **Consegue fazer sozinha**: escolher tema, redigir, humanizar, baixar
  imagem livre pra dentro do repo (`posts/queue/images/`), salvar o
  rascunho e dar `git commit` + `push`.
- **Não consegue**: abrir o Chrome e preencher o LinkedIn — isso depende
  da extensão `claude-in-chrome` conectada à sessão local do usuário, que
  não existe do lado da nuvem. Também não roda sozinha por cron local —
  precisa de uma sessão de Claude Code aberta na sua máquina.

Fluxo típico: a rotina prepara o rascunho automaticamente 3x/semana → você
abre o Claude Code quando quiser e pede "preenche o próximo rascunho no
LinkedIn" → revisa e publica manualmente.

Se a rotina disparar (`last_fired_at` atualiza em `claude.ai/code/routines`)
mas nenhum commit aparecer, o problema costuma ser autorização: confira
se o app **"Claude" está instalado no GitHub** cobrindo este repositório,
em `claude.ai` → Personalizar → Conectores → Integração com GitHub
(desvincular e vincular de novo reabre o fluxo de autorização do GitHub).

## Estrutura

```
linkedin-post-agent/
├── README.md
├── LICENSE
├── .gitignore
├── .claude/
│   ├── settings.json         # allowlist de permissões pro fluxo da skill
│   └── skills/
│       ├── linkedin_post_draft/
│       │   └── SKILL.md        # skill principal (orquestra tudo)
│       └── humanizer/
│           ├── SKILL.md         # skill de terceiros (blader/humanizer, MIT)
│           └── ATTRIBUTION.md
├── logs/
│   └── published.csv        # histórico de posts (rascunho/publicado)
├── posts/
│   └── queue/                # rascunhos gerados, um arquivo por post
│       └── images/            # imagens de capa baixadas (livres de licença)
└── sources/
    ├── sources.md             # fontes confiáveis de temas
    └── voice_samples.md       # (opcional) amostras da sua escrita para calibrar tom
```

## Personalizando

- **Fontes**: edite `sources/sources.md` para adicionar/remover newsletters,
  blogs ou termos de busca no Hacker News.
- **Tom de voz**: cole exemplos seus em `sources/voice_samples.md`.
- **Cadência**: a rotina em `claude.ai/code/routines` cobre o agendamento
  automático (veja seção acima). Pra mudar dias/horário, edite a rotina
  por lá ou peça pro Claude Code atualizá-la.

## Licença

Este projeto é distribuído sob a licença MIT — veja [LICENSE](LICENSE).

A skill em `.claude/skills/humanizer/` é uma cópia da skill "Humanizer" de
[blader/humanizer](https://github.com/blader/humanizer), também MIT. Veja
`.claude/skills/humanizer/ATTRIBUTION.md` para instruções de atualização.
