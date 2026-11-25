# CLAUDE.md — Regras globais do Método Padawan Frame (Seeke Studio)

Este arquivo vale para todo projeto criado com o `padawan-frame-plugin`. O comando `/padawan-start` copia este conteúdo para o `CLAUDE.md` da raiz do projeto, porque o Claude Code não carrega o `CLAUDE.md` de um plugin automaticamente.

## 1. Onde todo projeto vive

- Todo projeto nasce em `~/Documents/GitHub/[nome-do-projeto]`, criado pelo GitHub (repositório) e clonado pelo GitHub Desktop.
- Nome do projeto em `kebab-case`, sem acentos (ex.: `clinica-sorriso-lp`).
- Commits e push são feitos pelo Padawan no GitHub Desktop ao fim de cada fase concluída. Mensagem sugerida: `padawan: fase N concluída (<skill>)`.
- O Claude Code nunca faz `git push --force` nem apaga histórico.

## 2. Protocolo de estado `.padawan/`

```
.padawan/
├── STATUS.md          # fonte única da verdade do progresso
├── artifacts/         # um artefato por fase (01 a 07)
└── prompts/           # prompts entregues para IAs externas e seus retornos
```

Regras determinísticas:

1. Antes de qualquer ação, leia `.padawan/STATUS.md`. Se não existir, pare e mande rodar `/padawan-start`.
2. A tabela usa exatamente as colunas `Fase | Skill | Status (PENDENTE | EM_ANDAMENTO | CONCLUIDO) | Artefato | Próximo Comando`.
3. Só existem três status: `PENDENTE`, `EM_ANDAMENTO`, `CONCLUIDO`. Nada de "quase", "parcial" ou emoji.
4. Uma fase só vira `CONCLUIDO` quando: o artefato existe em `.padawan/artifacts/`; não contém `TODO`, `TBD`, `???` nem `{{`; o gate da skill foi checado item por item; e o Padawan confirmou.
5. Fases seguem a ordem 1 → 7. Para iniciar a fase N, a fase N-1 precisa estar `CONCLUIDO`. Exceção: o Padawan pede explicitamente para pular, e isso fica registrado no artefato da fase pulada como `PULADA_PELO_PADAWAN: <motivo>`, ainda com status `PENDENTE`.
6. Ao concluir uma fase: atualize a linha na tabela, marque o checklist de gates, atualize `Última atualização` e a seção `## Próximo comando`, e termine a resposta exibindo exatamente o próximo comando em bloco de código.
7. Validação rápida de pendências (rode antes de concluir):
   `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/0N-*.md`
   Saída vazia = pode concluir.

## 3. Jogo de cintura entre IAs (economia de tokens)

O Claude Code é caro para pensar texto criativo e barato para ler código. Divida assim:

| Tarefa | Quem faz |
|--------|----------|
| UX Discovery, Double Diamond, entrevistas, personas | ChatGPT ou Gemini |
| Benchmarking de referências (análise visual/estratégica) | ChatGPT ou Gemini (com navegação) |
| User Stories, backlog e cerimônias Scrum | ChatGPT ou Gemini |
| Copy curta de scan (hat, título, subtítulo, CTA) | ChatGPT ou Gemini |
| Prompts visuais: frame inicial, frame final, vídeo Higgsfield | ChatGPT ou Gemini → Higgsfield |
| Cena 3D (Blender MCP) | Agente especializado externo com Blender MCP |
| Leitura de repositório, engenharia reversa de HTML | Claude Code |
| Código, tokens, componentes, motion, build | Claude Code |
| SEO técnico, meta tags, JSON-LD | Claude Code |
| Auditoria de performance, acessibilidade e deploy | Claude Code |

**Breque de IA:** quando a tarefa cair na coluna externa, o Claude Code PARA, salva o prompt pronto em `.padawan/prompts/<fase>-<assunto>.md`, mostra o prompt ao Padawan em um bloco de código, marca a fase como `EM_ANDAMENTO`, registra na tabela "Aguardando IA externa" do STATUS e aguarda o retorno. Não escreve código nem texto criativo enquanto espera.

Formato padrão de prompt externo:

```
PAPEL: <especialista>
CONTEXTO: <projeto, público, objetivo, links>
TAREFA: <o que entregar>
FORMATO DE SAÍDA: <markdown com seções X, Y, Z>
RESTRIÇÕES: português do Brasil, mobile-first, frases curtas, sem jargão.
```

## 4. Mobile-first e scan

- Projete e codifique a partir de 360 px de largura; desktop é melhoria progressiva (`sm:`, `md:`, `lg:`).
- Leitura por scan: hat (até 4 palavras), título (até 8 palavras), subtítulo (até 20 palavras), um CTA principal por seção.
- Alvos de toque com no mínimo 44×44 px. Texto base com no mínimo 16 px. Contraste AA (4.5:1 texto, 3:1 elementos grandes).
- Todo motion respeita `prefers-reduced-motion` e tem fallback estático.
- Budgets padrão (página inicial, mobile): JS ≤ 170 KB gzip, LCP ≤ 2,5 s em 4G, CLS ≤ 0,1, INP ≤ 200 ms, imagem hero ≤ 200 KB, sequência de scroll frames ≤ 2,5 MB no mobile.

## 5. Postura com o Padawan

- O Padawan está aprendendo critério. Antes de decidir por ele, faça as Perguntas Provocativas da skill e registre as respostas no artefato.
- Explique o "porquê" em uma frase; não dê aula longa.
- Respostas curtas, em português do Brasil, com o próximo passo sempre explícito.

## 6. Ordem das 7 fases

| Fase | Skill / Comando | Artefato |
|------|-----------------|----------|
| 1 | `/padawan-discovery` | `.padawan/artifacts/01-discovery.md` |
| 2 | `/padawan-reverse-html` | `.padawan/artifacts/02-reverse-map.md` |
| 3 | `/padawan-build-next` | `.padawan/artifacts/03-build-base.md` |
| 4 | `/padawan-section-motion` | `.padawan/artifacts/04-section-motion.md` |
| 5 | `/padawan-seo-keywords` | `.padawan/artifacts/05-seo-meta.md` |
| 6 | `/padawan-perf-guard` | `.padawan/artifacts/06-perf-report.md` |
| 7 | `/padawan-deploy` | `.padawan/artifacts/07-deploy-log.md` |

Use `/padawan-status` a qualquer momento para ver onde o projeto está e qual é o próximo comando.
