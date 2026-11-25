---
name: padawan-perf-guard
description: "Fase 6 do Método Padawan Frame. Agente guardião econômico em tokens que audita e corrige a performance para mobile excelente e máquinas modestas (scroll frames, budgets de mídia/JS, prefers-reduced-motion, fallbacks leves de 3D/shaders, escaneabilidade). Gera .padawan/artifacts/06-perf-report.md."
user-invocable: false
---

## Objetivo

Fazer a página ficar rápida e fluida em um celular de entrada em 4G, sem perder a intenção visual: medir, comparar com os budgets, corrigir o que estoura e documentar.

## Gatilho e Breque de IA

- **Claude Code executa:** medições por comando, leitura dirigida de arquivos, correções e relatório.
- **Economia de tokens (regra desta skill):** nunca leia o projeto inteiro. Meça primeiro com comandos que retornam números, abra só os arquivos que estouraram budget e corrija em lotes pequenos. Saídas longas vão para arquivo em `.padawan/refs/perf/` e só o resumo entra no chat.
- **BREQUE:** nenhum de IA criativa. Se um asset visual precisar ser refeito mais leve (vídeo Higgsfield ou `.glb`), PARE e entregue o prompt/briefing de reexport ao Padawan, com os novos limites.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` (Fase 5 `CONCLUIDO`). Marque Fase 6 `EM_ANDAMENTO`. `mkdir -p .padawan/refs/perf`.
2. Budgets (do `CLAUDE.md`): JS inicial ≤ 170 KB gzip; LCP ≤ 2,5 s; CLS ≤ 0,1; INP ≤ 200 ms; hero ≤ 200 KB; frames mobile ≤ 2,5 MB por seção; `.glb` ≤ 1,5 MB; fontes ≤ 2 famílias e ≤ 4 arquivos.
3. **Medir build:** `npm run build > .padawan/refs/perf/build.txt 2>&1`; registre o tamanho do First Load JS (Next) ou dos bundles em `dist/` (Astro: `du -sh dist/_astro/*.js | sort -h | tail`).
4. **Medir mídia:** `find public -type f \( -name "*.jpg" -o -name "*.png" -o -name "*.webp" -o -name "*.avif" -o -name "*.mp4" -o -name "*.glb" \) -size +200k -exec du -h {} + | sort -h` e `du -sh public/media/*/frames-*`.
5. **Medir Lighthouse mobile** (com o servidor de produção rodando: `npm run start` ou `npx astro preview`):
   `npx -y lighthouse http://localhost:3000 --form-factor=mobile --throttling-method=simulate --only-categories=performance,accessibility,best-practices,seo --output=json --output-path=.padawan/refs/perf/lh-mobile.json --chrome-flags="--headless" --quiet`
   Extraia só os números: `node -e "const r=require('./.padawan/refs/perf/lh-mobile.json');const a=r.audits;console.log({perf:r.categories.performance.score,a11y:r.categories.accessibility.score,LCP:a['largest-contentful-paint'].displayValue,CLS:a['cumulative-layout-shift'].displayValue,TBT:a['total-blocking-time'].displayValue})"`
6. **Corrigir o que estourou**, nesta ordem de impacto:
   1. **Scroll frames:** reduzir fps/resolução/qualidade; pré-carregar só os 10 primeiros frames e o resto sob demanda; `IntersectionObserver` para não carregar fora da viewport; desenhar só quando o frame muda; `devicePixelRatio` limitado a 2.
   2. **3D/shaders:** carregar com `dynamic(..., { ssr: false })`/`client:visible` só quando a seção entra na viewport; `dpr={[1, 1.5]}` no Canvas; `frameloop="demand"` quando possível; em `navigator.hardwareConcurrency <= 4`, `navigator.deviceMemory <= 4` ou reduced motion, renderizar o fallback estático (AVIF/gradiente CSS).
   3. **JS:** remover libs não usadas (`npx -y depcheck`), importar GSAP só com plugins usados, `next/dynamic` para componentes abaixo da dobra, evitar `"use client"` em seções estáticas.
   4. **Imagens:** AVIF/WebP, `sizes` corretos, `priority`/`fetchpriority="high"` só no LCP, `loading="lazy"` no resto, `width`/`height` sempre (CLS).
   5. **Fontes:** subset latin, `display: swap`, preload só da fonte do `h1`.
   6. **Motion:** animar só `transform` e `opacity`; `will-change` apenas durante a animação; conferir que `prefers-reduced-motion` desliga scrub, parallax, 3D e shaders.
7. **Escaneabilidade (checagem rápida em 360 px):** cada seção cabe a mensagem principal em uma tela, texto ≥ 16 px, linhas ≤ 75 caracteres, CTA visível sem precisar de hover, alvos ≥ 44 px.
8. Repita as medições 3 e 5 após as correções e registre antes/depois.
9. Registre no artefato, rode o Gate e atualize o STATUS.

Modelo do artefato `06-perf-report.md`:

```
# 06 — Relatório de Performance
## Budgets (tabela Métrica | Budget | Antes | Depois | OK?)
## Mídia acima do limite (antes/depois)
## Correções aplicadas (arquivo → mudança → impacto)
## Fallbacks de 3D/shaders/scroll frames (condição de ativação)
## Escaneabilidade 360 px (checklist)
## Riscos aceitos pelo Padawan
## Validação
```

## Perguntas Provocativas

- Seu site funciona bem no celular mais barato de alguém da sua família? Você testou?
- Esse efeito vale os KB que custa? Quantos segundos de LCP ele adiciona?
- Se você tivesse que cortar uma única coisa para ganhar 1 segundo, o que seria?
- O fallback de reduced motion parece um site quebrado ou um site intencionalmente calmo?

## Critério de Conclusão (Gate)

- [ ] Lighthouse mobile: Performance ≥ 0,85 e Accessibility ≥ 0,95 (ou exceção registrada em "Riscos aceitos" com motivo).
- [ ] LCP ≤ 2,5 s, CLS ≤ 0,1 e TBT ≤ 200 ms na simulação mobile.
- [ ] JS inicial ≤ 170 KB gzip.
- [ ] Nenhuma imagem acima de 200 KB sem justificativa; frames mobile ≤ 2,5 MB por seção.
- [ ] Fallback leve ativo para 3D, shaders e scroll frames em reduced motion e hardware modesto.
- [ ] Tabela antes/depois preenchida.
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/06-perf-report.md` retorna vazio.
- [ ] Padawan validou.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/06-perf-report.md`
- Medições brutas: `.padawan/refs/perf/` (no `.gitignore`)
- `.padawan/STATUS.md`: Fase 6 → `CONCLUIDO`, `## Próximo comando` → `/padawan-deploy`.
- Termine exibindo exatamente:

```
/padawan-deploy
```
