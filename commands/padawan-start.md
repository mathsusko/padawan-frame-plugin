---
name: padawan-start
description: "Inicia um projeto do Método Padawan Frame na pasta atual, criando .padawan/STATUS.md, artefatos, prompts e o CLAUDE.md do projeto."
argument-hint: "[nome-do-projeto]"
allowed-tools: Bash, Read, Write, Edit, Glob
---

## Objetivo

Preparar o projeto `$ARGUMENTS` para o Método Padawan Frame: validar a pasta em `~/Documents/GitHub/`, criar a estrutura `.padawan/`, o painel `STATUS.md` e o `CLAUDE.md` do projeto, e apontar o primeiro comando.

## Gatilho e Breque de IA

- **Claude Code executa:** checagem de pasta e git, criação de arquivos, cópia de regras.
- **Breque:** nenhum. Se o repositório ainda não existe no GitHub, PARE e passe as instruções do Passo 2 para o Padawan fazer no GitHub/GitHub Desktop.

## Passo a Passo Operacional

1. Defina o nome: use `$ARGUMENTS`; se vazio, use o nome da pasta atual (`basename "$PWD"`). Confirme que está em `kebab-case` sem acentos.
2. Rode `pwd` e `git rev-parse --show-toplevel 2>/dev/null; git remote -v`. Se a pasta não for `~/Documents/GitHub/<nome>` ou não tiver remote `origin`, PARE e oriente:
   1. Criar o repositório `<nome>` em github.com (privado, sem template).
   2. No GitHub Desktop: File → Clone Repository → escolher `<nome>` → Local Path `~/Documents/GitHub/<nome>`.
   3. Abrir o terminal nessa pasta, rodar `claude` e repetir `/padawan-start <nome>`.
3. Se `.padawan/STATUS.md` já existir, NÃO sobrescreva: mostre a tabela atual e o próximo comando (mesmo comportamento de `/padawan-status`) e encerre.
4. Crie as pastas: `mkdir -p .padawan/artifacts .padawan/prompts`.
5. Crie `.padawan/STATUS.md` a partir do modelo `${CLAUDE_PLUGIN_ROOT}/templates/STATUS.md`, substituindo `{{NOME_DO_PROJETO}}`, `{{URL_DO_REPOSITORIO}}` (saída de `git remote get-url origin`) e `{{DATA_ISO}}` (`date +%Y-%m-%d`). Se a variável não resolver, localize o plugin com `find ~/.claude/plugins ~/Documents/GitHub/padawan-frame-plugin -path '*padawan-frame-plugin*' -name STATUS.md 2>/dev/null | head -1`.
6. Crie `CLAUDE.md` na raiz do projeto copiando `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md`. Se já existir um `CLAUDE.md`, acrescente o conteúdo ao final sob o título `# Método Padawan Frame` em vez de sobrescrever.
7. Crie `.padawan/prompts/README.md` com uma linha: `Prompts entregues a IAs externas (<fase>-<assunto>.md) e seus retornos (<fase>-<assunto>-retorno.md).`
8. Garanta que `.padawan/` NÃO está no `.gitignore` (o estado precisa ir para o GitHub).
9. Rode `grep -c "{{" .padawan/STATUS.md`; o resultado tem que ser `0`.
10. Oriente o commit no GitHub Desktop: `padawan: início do projeto`.

## Perguntas Provocativas

- Em uma frase, qual problema real esse site resolve para quem chega nele?
- Se o visitante só ler o título e o botão, o que ele precisa entender?

Registre as respostas no topo de `.padawan/STATUS.md`, logo abaixo de `Última atualização`, como `- **Problema:**` e `- **Mensagem-chave:**`.

## Critério de Conclusão (Gate)

- [ ] Pasta é `~/Documents/GitHub/<nome>` com remote `origin`.
- [ ] `.padawan/STATUS.md` existe com as 7 fases em `PENDENTE` e sem `{{`.
- [ ] `.padawan/artifacts/` e `.padawan/prompts/` existem.
- [ ] `CLAUDE.md` do projeto contém as regras do Método Padawan Frame.

## Saída Obrigatória

- Arquivos: `.padawan/STATUS.md`, `.padawan/prompts/README.md`, `CLAUDE.md`.
- `.padawan/STATUS.md`: todas as fases `PENDENTE`, seção `## Próximo comando` com `/padawan-discovery`.
- Termine exibindo exatamente:

```
/padawan-discovery
```
