---
name: padawan-build-next
description: "Fase 3 do Método Padawan Frame. Monta a base front-end logo após a engenharia reversa, com Next.js + Tailwind CSS + shadcn/ui (padrão) ou Astro (sites estáticos/conteúdo), aplicando os tokens e a estrutura de componentes antes de qualquer motion/3D. Gera .padawan/artifacts/03-build-base.md."
user-invocable: false
---

## Objetivo

Deixar o projeto rodando localmente com stack escolhida, tokens de design aplicados, estrutura de pastas previsível e as seções vazias montadas em ordem, para que a Fase 4 só pluge conteúdo e motion.

## Gatilho e Breque de IA

- **Claude Code executa:** toda a fase (instalação, configuração, tokens, componentes base, build local).
- **BREQUE:** nenhum. Se o Padawan não tiver Node.js 20+ instalado, PARE e oriente a instalação (`brew install node` ou nodejs.org) antes de continuar.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` (Fase 2 `CONCLUIDO`), `01-discovery.md` (Arquitetura de Seções) e `02-reverse-map.md` (Tokens propostos). Marque Fase 3 `EM_ANDAMENTO`.
2. Escolha a stack com o Padawan usando esta regra:
   - **Next.js (padrão):** formulários, integrações, áreas dinâmicas, motion pesado com React (React Bits, React Spring, R3F).
   - **Astro:** página de conteúdo/institucional quase estática, blog, máxima performance com pouco JS; ilhas React só onde houver interação.
3. Confira o ambiente: `node -v` (≥ 20) e `npm -v`. A pasta já é o repositório clonado; instale na raiz atual (`.`).
4. **Se Next.js:**
   1. `npx create-next-app@latest . --ts --tailwind --eslint --app --src-dir --import-alias "@/*" --use-npm` (se a pasta não estiver vazia por causa de `.padawan/`, `CLAUDE.md` ou `README.md`, mova-os temporariamente para `../_padawan_tmp/`, rode e devolva).
   2. `npx shadcn@latest init` (estilo padrão, cor base neutra) e adicione só o necessário: `npx shadcn@latest add button card input textarea sheet`.
   3. Estrutura: `src/app/` (rotas), `src/components/sections/` (uma seção por arquivo, na ordem da arquitetura), `src/components/ui/` (shadcn), `src/components/motion/` (vazio, para a Fase 4), `src/lib/`, `public/media/`.
5. **Se Astro:**
   1. `npm create astro@latest . -- --template minimal --typescript strict --install --no-git`
   2. `npx astro add tailwind` e, se houver ilhas interativas, `npx astro add react`.
   3. Estrutura: `src/pages/`, `src/layouts/BaseLayout.astro`, `src/components/sections/`, `src/components/motion/`, `public/media/`.
6. Aplique os tokens da Fase 2 como variáveis CSS em `src/app/globals.css` (Next) ou `src/styles/global.css` (Astro), no bloco `@theme` do Tailwind v4 (ou `tailwind.config` se a versão instalada for v3): `--color-*`, `--font-*`, `--radius-*`, `--shadow-*`, escala de espaçamento. Carregue fontes com `next/font` (Next) ou `@fontsource` (Astro), com `display: swap`.
7. Crie um componente por seção com o esqueleto semântico (`<section id="..." aria-labelledby="...">`, hat, h2, subtítulo, CTA) e textos provisórios curtos marcados como `[copy fase 4]`. Monte-os em ordem na página inicial.
8. Base de acessibilidade e performance: `<html lang="pt-BR">`, um único `h1`, foco visível, `prefers-reduced-motion` global em CSS, imagens via `next/image` ou `astro:assets`.
9. Rode `npm run build` e `npm run dev`; confira em 360 px e 1440 px. Corrija erros até o build passar.
10. Registre no artefato: stack e motivo, versões (`npx next --version` ou `npx astro --version`), árvore de pastas (`tree -L 3 src` ou `find src -maxdepth 3`), tokens aplicados, lista de seções, resultado do build e comando de dev.
11. Oriente commit no GitHub Desktop: `padawan: fase 3 concluída (base)`. Rode o Gate e atualize o STATUS.

## Perguntas Provocativas

- Essa página precisa mesmo de JavaScript em todas as seções? Onde Astro ganharia de Next aqui?
- Algum componente do shadcn está entrando só porque é fácil, e não porque a página precisa?
- Seus tokens têm nome de função (`--color-cta`) ou de aparência (`--color-blue`)? Qual envelhece melhor?
- Quantas famílias e pesos de fonte você está carregando? Qual dá para cortar?

## Critério de Conclusão (Gate)

- [ ] `npm run build` termina sem erro (colar as últimas linhas no artefato).
- [ ] Tokens da Fase 2 aplicados como variáveis CSS e usados nas seções.
- [ ] Uma seção por componente, na ordem da arquitetura da Fase 1.
- [ ] `lang="pt-BR"`, um único `h1`, regra global de `prefers-reduced-motion`.
- [ ] Os únicos textos provisórios são `[copy fase 4]` (não contam como pendência).
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/03-build-base.md` retorna vazio.
- [ ] Padawan abriu o `npm run dev` e validou.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/03-build-base.md`
- Código: base do projeto na raiz do repositório.
- `.padawan/STATUS.md`: Fase 3 → `CONCLUIDO`, `## Próximo comando` → `/padawan-section-motion`.
- Termine exibindo exatamente:

```
/padawan-section-motion
```
