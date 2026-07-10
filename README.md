# linkedin-post-agent

Projeto local para ajudar a manter presença ativa no LinkedIn (~3x/semana),
com posts sobre SRE, DevOps, Engenharia de Plataforma e IA aplicada a
operações (AIOps).

## Como funciona

1. A skill `skills/linkedin_post_draft.md` escolhe um tema atual a partir
   das fontes em `sources/sources.md` (Hacker News filtrado, SRE Weekly,
   The New Stack, awesome-ai-sre etc.).
2. Gera um rascunho de post conectando o tema com sua vivência real em
   AWS/EKS, Terraform, ArgoCD, GitLab CI, Vault, etc.
3. Passa o texto pela skill `skills/humanizer/SKILL.md`
   ([blader/humanizer](https://github.com/blader/humanizer)) para remover
   marcas típicas de texto gerado por IA.
4. Salva o rascunho em `posts/queue/`.
5. Abre o LinkedIn no Chrome, digita o post no campo de nova publicação e
   **para** — a confirmação de publicar é sempre manual.

**Nada nesta skill clica em "Publicar", "Conectar" ou "Seguir" sozinho.**
O único passo automatizado é escrever o rascunho na caixa de post; você
decide se publica.

## Uso

Com o Claude Code em modo Chrome:

```
claude --chrome
```

E peça para executar a skill `linkedin_post_draft`, opcionalmente indicando
um tema específico. Recomendado rodar até 3x por semana (ex: seg/qua/sex).

## Estrutura

```
linkedin-post-agent/
├── README.md
├── .gitignore
├── logs/
│   └── published.csv        # histórico de posts (rascunho/publicado)
├── posts/
│   └── queue/                # rascunhos gerados, um arquivo por post
├── sources/
│   ├── sources.md             # fontes confiáveis de temas
│   └── voice_samples.md       # (opcional) amostras da sua escrita para calibrar tom
└── skills/
    ├── linkedin_post_draft.md # skill principal (orquestra tudo)
    └── humanizer/
        ├── SKILL.md            # skill de terceiros (blader/humanizer, MIT)
        └── ATTRIBUTION.md
```

## Personalizando

- **Fontes**: edite `sources/sources.md` para adicionar/remover newsletters,
  blogs ou termos de busca no Hacker News.
- **Tom de voz**: cole exemplos seus em `sources/voice_samples.md`.
- **Cadência**: ajuste a frequência de execução (cron, lembrete manual, etc.)
  fora deste projeto — a skill não se auto-agenda.

## Licença

Este projeto é distribuído sob a licença MIT — veja [LICENSE](LICENSE).

A skill em `skills/humanizer/` é uma cópia da skill "Humanizer" de
[blader/humanizer](https://github.com/blader/humanizer), também MIT. Veja
`skills/humanizer/ATTRIBUTION.md` para instruções de atualização.
