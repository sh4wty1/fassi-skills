# Pendências

Anotado em 2026-10-06. Nada aqui foi resolvido ainda.

## 1. `help-me` não vem na instalação por npx

**Sintoma:** depois de instalar pelo npx, o `help-me` não estava disponível. Pelo plugin ele aparece como `/fassi-skills:help-me`.

**Causa (confirmada):** o comando do README instala uma skill só:

```bash
npx skills@latest add sh4wty1/fassi-skills --skill setup-fassi-skills -g
```

`~/.claude/skills/` tem `setup-fassi-skills` e não tem `help-me`.

**Caminhos possíveis:**
- o README instalar as duas (`--skill setup-fassi-skills --skill help-me`, ou sem `--skill`, se o instalador pegar todas);
- o `setup-fassi-skills` instalar o `help-me` quando ele faltar.

## 2. O setup não recomendou nenhuma das skills novas

**Sintoma:** depois de adicionar as 87 skills do TLC e atualizar, rodar o setup de novo só disse que as recomendadas já estavam instaladas. A expectativa era ver algumas das novas.

**Causa: não confirmada.** Hipóteses, da mais provável para a menos:

1. **Cópia antiga.** `/setup-fassi-skills` abre a cópia global em `~/.claude/skills/`, instalada pelo npx. O `/plugin marketplace update` atualiza só a do plugin (`/fassi-skills:setup-fassi-skills`). Em 2026-10-06 a global ainda tinha o manifest de 9 skills. Conferir: contar as skills em `~/.claude/skills/setup-fassi-skills/manifest.json`.
2. **Regra conservadora.** A recomendação é "skills cujo `when_to_use` se encaixa no projeto". Neste repo, que não tem código de aplicação, isso deu só `harness-eval` e `skill-architect`, mesmo com o manifest de 96.
3. **Falta de regra para uma segunda rodada.** O SKILL.md não manda mostrar o que entrou no registry desde a última vez, nem os "quase encaixes" que valeria conhecer.

**Caminhos possíveis:**
- resolver a duplicidade plugin/npx: documentar um único jeito de instalar, ou o setup avisar quando o manifest ao lado dele está atrás do do GitHub;
- o setup separar "recomendadas" de "vale conhecer" (encaixe parcial), em vez de um conjunto só;
- numa segunda rodada, destacar as skills do manifest que ainda não estão instaladas e que são novas.

## 3. Criar uma skill de padrões de commits

**Ideia:** uma skill minha, em `skills/<nome>/SKILL.md`, que oriente a escrita das mensagens de commit.

**Base:** https://github.com/iuricode/padroes-de-commits (ainda não lido; conferir a licença antes de reaproveitar texto).

**A decidir:** nome da skill, se ela só orienta ou também escreve o commit, e se entra no manifest ou só na tabela "Skills written by me" do README.

## 4. pentestwithai/skills — não adicionado

O repo https://github.com/pentestwithai/skills foi avaliado e **não entrou** no registry, por dois motivos:

- **Sem licença.** Nenhum arquivo LICENSE; o registry registra a licença de cada fonte e não aponta para conteúdo sem licença conhecida.
- **Não é uma skill de audit.** O único `SKILL.md` é um agente ofensivo autônomo (exploração até root), não uma análise defensiva. Para audit de segurança o TLC já cobre o lado defensivo: `security-best-practices`, `security-threat-model`, `security-ownership-map`.

Se ainda quiser algo dessa linha, o caminho é escrever uma skill própria de análise whitebox (a base seria o `GEMINI.md` do repo, não o `SKILL.md`), e aí a licença passa a ser a nossa. Decidir depois.

## Como retomar

1. Conferir a hipótese 2.1 antes de mudar qualquer coisa: se for só cópia antiga, o item 2 encolhe para um problema de atualização.
2. `/fassi-skills:help-me` com cada item, para escolher o caminho.
3. `/skill-architect` para redesenhar o passo de recomendação do setup, se o item 2 pedir mudança no SKILL.md, e para desenhar a skill do item 3.
4. `/harness-eval` depois das mudanças, para auditar as duas skills.

Arquivos envolvidos: `README.md` (seção Install), `skills/setup-fassi-skills/SKILL.md` (passos 1 e 2), `skills/help-me/SKILL.md`.
