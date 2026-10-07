# Instalar skills recomendadas neste projeto

**Por quê:** rodar o `/setup-fassi-skills` no próprio repositório, depois de adicionar o catálogo do TLC, para ter aqui as skills que servem a um registry de skills.
**O quê:** `harness-eval` e `skill-architect` instaladas para o Claude Code. Nenhum workflow recomendado, porque o repo não tem features de código.
**Como:** `npx --yes @tech-leads-club/agent-skills install -s <skill> -a claude-code`, uma por vez, a partir da raiz. Criou `.claude/skills/harness-eval/`, `.claude/skills/skill-architect/` e `.agents/.skill-lock.json`.
**Verificação:** os dois `SKILL.md` existem em `.claude/skills/`.
**Pendências:** `.claude/` e `.agents/` estão sem rastreio no git; decidir entre ignorar ou commitar. A cópia global do `setup-fassi-skills` em `~/.claude/skills/` continua na versão antiga (9 skills) até rodar `npx skills@latest update -g`.
