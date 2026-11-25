# padawan-frame-plugin

Plugin do Claude Code da **Seeke Studio** que implementa o **Método Padawan Frame**: um caminho de 7 fases para um Padawan sair do briefing até uma landing page publicada, com motion de qualidade, SEO técnico, performance mobile e conformidade básica. O plugin guarda o progresso em `.padawan/STATUS.md`, exige um artefato validado por fase e sabe a hora de parar e entregar um prompt pronto para ChatGPT, Gemini, Higgsfield ou um agente com Blender MCP, para não gastar tokens de código em tarefas criativas.

## Instalação única (uma vez por máquina)

O plugin fica na sua máquina em `~/Documents/GitHub/padawan-frame-plugin` (clonado pelo GitHub Desktop). Ele já traz um `marketplace.json`, então você instala uma vez e ele fica disponível em todos os projetos.

Dentro do Claude Code, em qualquer pasta:

```
/plugin marketplace add ~/Documents/GitHub/padawan-frame-plugin
/plugin install padawan-frame-plugin@seeke-studio
```

Reinicie o Claude Code. Confira com `/plugin` (aba Installed) ou digitando `/padawan` para ver os comandos.

Para testar uma alteração no plugin sem reinstalar, rode o Claude Code apontando para a pasta:

```
claude --plugin-dir ~/Documents/GitHub/padawan-frame-plugin
```

Quando você puxar uma versão nova pelo GitHub Desktop, atualize com `/plugin marketplace update seeke-studio`.

## Fluxo de um projeto novo

1. No GitHub, crie o repositório do projeto (ex.: `clinica-sorriso-lp`).
2. No GitHub Desktop, clone em `~/Documents/GitHub/clinica-sorriso-lp`.
3. Abra o terminal na pasta e rode `claude`.
4. Rode `/padawan-start clinica-sorriso-lp`. O comando cria `.padawan/`, `STATUS.md`, o `CLAUDE.md` do projeto e mostra o próximo comando.
5. Siga as fases. Ao fim de cada uma, faça commit e push no GitHub Desktop. Assim o progresso (`.padawan/`) fica sincronizado e qualquer pessoa da equipe continua de onde parou.

| Fase | Comando | O que entrega |
|------|---------|---------------|
| 1 | `/padawan-discovery` | Briefing, Double Diamond, benchmarking de 3 referências e User Stories |
| 2 | `/padawan-reverse-html` | Mapa de engenharia reversa: estrutura, componentes, shaders/scroll |
| 3 | `/padawan-build-next` | Base Next.js (Tailwind + shadcn/ui) ou Astro com tokens |
| 4 | `/padawan-section-motion` | Decupagem por seção e integração de motion/3D |
| 5 | `/padawan-seo-keywords` | Keywords, headings e meta tags com JSON-LD |
| 6 | `/padawan-perf-guard` | Relatório de performance mobile e budgets |
| 7 | `/padawan-deploy` | Testes desafiadores, conformidade, build e deploy |

`/padawan-status` mostra a qualquer momento onde você está e qual o próximo comando.

## Ferramentas externas

O plugin não substitui essas ferramentas, ele prepara o prompt e espera o retorno:

- **ChatGPT ou Gemini:** UX Discovery, benchmarking, Scrum, copy curta de scan, prompts de frame inicial e final.
- **Higgsfield:** vídeo entre frame A e frame B, que vira sequência de scroll frames.
- **Blender MCP (agente especializado):** cenas 3D exportadas em `.glb`.
- **GitHub Desktop:** commit e push ao fim de cada fase.
- **Vercel ou Netlify:** deploy (Fase 7).

Quando o Claude Code parar com um prompt, copie, cole na ferramenta indicada, traga a resposta de volta e cole no chat. O prompt e o retorno ficam salvos em `.padawan/prompts/`.

## Estrutura do plugin

```
.claude-plugin/plugin.json       manifesto
.claude-plugin/marketplace.json  marketplace local "seeke-studio"
CLAUDE.md                        regras globais (copiadas para cada projeto)
templates/STATUS.md              modelo do painel de estado
commands/                        comandos /padawan-*
skills/                          uma skill por fase
```

## Licença

MIT © Seeke Studio. Os textos de conformidade gerados na Fase 7 são pontos de partida e não substituem revisão jurídica formal.
