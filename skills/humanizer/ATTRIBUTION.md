# Origem

`SKILL.md` neste diretório é uma cópia local, sem modificações, da skill
"Humanizer" de [blader/humanizer](https://github.com/blader/humanizer)
(licença MIT). Usada aqui como passo de revisão de texto antes de publicar
posts no LinkedIn — remove padrões típicos de escrita gerada por IA
(vocabulário genérico, listas em "regra de três", travessões em excesso,
tom promocional, etc.).

Para atualizar para uma versão mais nova do upstream:

```bash
gh api repos/blader/humanizer/contents/SKILL.md --jq '.content' | base64 -d > skills/humanizer/SKILL.md
```
