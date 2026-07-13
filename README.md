# 📣 linkedin-post-agent

> **Projeto de estudo sobre uso do [Claude Code](https://claude.com/claude-code).**
> O objetivo principal não é o post em si, mas explorar até onde dá pra ir
> com skills customizadas, automação de navegador (`claude-in-chrome`),
> rotinas agendadas na nuvem e políticas de permissão — tudo dentro de um
> caso de uso real e contido.

Agente que ajuda a manter presença ativa no LinkedIn (~3x/semana), com
posts sobre **SRE, DevOps, Engenharia de Plataforma e IA aplicada a
operações (AIOps)**. Roda localmente via Claude Code e também tem uma
rotina agendada na nuvem que prepara os rascunhos sozinha.

---

## 🧠 Sobre este projeto

Este repositório documenta um experimento hands-on com Claude Code,
cobrindo:

- 🛠️ **Skills customizadas** (`.claude/skills/`) — como estruturar
  instruções reutilizáveis e encadear uma skill de terceiros
  (`humanizer`) dentro de outra.
- 🖱️ **Automação de navegador** via `claude-in-chrome` — preencher um
  formulário real (o composer do LinkedIn) sem nunca confirmar o envio.
- ☁️ **Rotinas agendadas na nuvem** (`claude.ai/code/routines`) — um
  ambiente isolado, sem acesso à máquina local, gerando conteúdo sozinho
  em um cron.
- 🔐 **Modelo de permissões** — o que dá pra automatizar com segurança
  (`.claude/settings.json`) vs. o que exige confirmação humana sempre
  (publicar, autorizar apps, deletar repositório).
- 🧩 **Limites reais da arquitetura** — por exemplo: a nuvem não consegue
  "acordar" o Chrome da sua máquina, e isso não é um bug, é a fronteira
  esperada entre local e remoto.

Não é um produto polido — é um laboratório. Erros, decisões revertidas e
ajustes de rota fazem parte do histórico de commits de propósito.

---

## ⚙️ Como funciona

1. 🔎 A skill `linkedin_post_draft` (`.claude/skills/linkedin_post_draft/SKILL.md`)
   levanta de 3 a 5 temas atuais candidatos a partir das fontes em
   `sources/sources.md` (Hacker News filtrado, SRE Weekly, The New Stack,
   awesome-ai-sre etc.), evitando repetir assunto das últimas 2 semanas
   (`logs/published.csv`), e **pergunta ao usuário qual dos candidatos vira
   o post** antes de escrever qualquer rascunho.
2. ✍️ Gera um rascunho conectando o tema escolhido com vivência real em AWS/EKS,
   Terraform, ArgoCD, GitLab CI, Vault etc., estruturado em parágrafos
   curtos e fechado com citação da fonte e até 3 hashtags.
3. 🧹 Passa o texto pela skill `humanizer`
   (`.claude/skills/humanizer/SKILL.md`, de
   [blader/humanizer](https://github.com/blader/humanizer)) para remover
   marcas típicas de texto gerado por IA.
4. 🖼️ Busca uma imagem de capa em banco de licença livre (Unsplash/Pexels)
   relacionada ao tema, quando não há imagem própria em `sources/`.
5. 💾 Salva o rascunho em `posts/queue/`.
6. 🌐 Abre o LinkedIn no Chrome, digita o post no campo de nova publicação
   e **para** — a confirmação de publicar é sempre manual. A imagem
   baixada fica em `~/Downloads/linkedin-posts/` pra anexar manualmente,
   já que o upload de arquivo abre um diálogo nativo do SO que a
   automação do Chrome não alcança.

> ⛔ **Nada nesta skill clica em "Publicar", "Conectar" ou "Seguir"
> sozinho.** O único passo automatizado no navegador é escrever o
> rascunho na caixa de post; você decide se publica e anexa a imagem.

> 🌍 **Este repositório é público**, e os rascunhos (`posts/queue/`,
> `logs/published.csv`) ficam versionados nele — decisão consciente
> (2026-07-10) pra manter tudo num lugar só, já que o conteúdo sempre
> passa por revisão antes de ir pro LinkedIn de qualquer forma.

---

## 🚀 Uso local

Com o Claude Code em modo Chrome, dentro desta pasta:

```bash
cd linkedin-post-agent
claude --chrome
```

Como as skills estão em `.claude/skills/`, o Claude Code já lista
`linkedin_post_draft` e `humanizer` como skills invocáveis nessa sessão
(escopo local ao projeto). `.claude/settings.json` já libera as
permissões necessárias pro fluxo (WebFetch/WebSearch dos domínios usados,
download de imagem livre, git, gh, tools de browser), então ele roda sem
prompts de aprovação a cada comando.

Dois modos, em linguagem natural:

| Modo | Como pedir | O que faz |
|---|---|---|
| 🆕 **Gerar rascunho** | "roda a skill do LinkedIn" (ou peça um tema específico) | Levanta candidatos, pergunta qual tema você quer, escreve, humaniza, busca imagem, salva e commita |
| 📤 **Preencher rascunho pronto** | "preenche o próximo rascunho no LinkedIn" / "posta o próximo rascunho" | `git pull`, pega a entrada mais antiga com `status=draft`, revisa contra o padrão da skill e preenche no navegador |

---

## ☁️ Rotina agendada (nuvem)

Existe uma rotina de cron (**"LinkedIn post draft - SRE/DevOps/AIOps"**,
seg/qua/sex às 12h horário de Fortaleza) rodando em
`claude.ai/code/routines` sobre este repositório
(`github.com/lucascosm3/linkedin-post-agent`). Ela roda isolada na nuvem,
então:

- ✅ **Consegue fazer sozinha**: escolher tema, redigir, humanizar, baixar
  imagem livre pra dentro do repo (`posts/queue/images/`), salvar o
  rascunho e dar `git commit` + `push`.
- ❌ **Não consegue**: abrir o Chrome e preencher o LinkedIn — isso
  depende da extensão `claude-in-chrome` conectada à sessão local do
  usuário, que não existe do lado da nuvem. Também não roda sozinha por
  cron local — precisa de uma sessão de Claude Code aberta na máquina.

**Fluxo típico:** a rotina prepara o rascunho automaticamente 3x/semana →
você abre o Claude Code quando quiser e pede *"preenche o próximo
rascunho no LinkedIn"* → revisa e publica manualmente.

> 🩺 **Troubleshooting:** se a rotina disparar (`last_fired_at` atualiza
> em `claude.ai/code/routines`) mas nenhum commit aparecer, o problema
> costuma ser autorização: confira se o app **"Claude" está instalado no
> GitHub** cobrindo este repositório, em `claude.ai` → Personalizar →
> Conectores → Integração com GitHub (desvincular e vincular de novo
> reabre o fluxo de autorização).

---

## 📁 Estrutura

```
linkedin-post-agent/
├── README.md
├── LICENSE
├── .gitignore
├── .claude/
│   ├── settings.json          # allowlist de permissões pro fluxo da skill
│   └── skills/
│       ├── linkedin_post_draft/
│       │   └── SKILL.md       # skill principal (orquestra tudo)
│       └── humanizer/
│           ├── SKILL.md       # skill de terceiros (blader/humanizer, MIT)
│           └── ATTRIBUTION.md
├── logs/
│   └── published.csv          # histórico de posts (draft/filled)
├── posts/
│   └── queue/                 # rascunhos gerados, um arquivo por post
│       └── images/            # imagens de capa baixadas (licença livre)
└── sources/
    ├── sources.md              # fontes confiáveis de temas
    └── voice_samples.md        # (opcional) amostras de escrita para calibrar tom
```

---

## 🎛️ Personalizando

- **📚 Fontes**: edite `sources/sources.md` pra adicionar/remover
  newsletters, blogs ou termos de busca no Hacker News.
- **🗣️ Tom de voz**: cole exemplos seus em `sources/voice_samples.md`.
- **⏰ Cadência**: a rotina em `claude.ai/code/routines` cobre o
  agendamento automático (veja seção acima). Pra mudar dias/horário,
  edite a rotina por lá ou peça pro Claude Code atualizá-la.

---

## 📄 Licença

Este projeto é distribuído sob a licença MIT — veja [LICENSE](LICENSE).

A skill em `.claude/skills/humanizer/` é uma cópia da skill "Humanizer" de
[blader/humanizer](https://github.com/blader/humanizer), também MIT. Veja
`.claude/skills/humanizer/ATTRIBUTION.md` para instruções de atualização.
