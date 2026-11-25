---
name: padawan-section-motion
description: "Fase 4 do Método Padawan Frame. Dupla acoplada de Decupagem de Seção (hat, título, subtítulo, CTA, background) e Direção Visual & Integração de Motion (Scroll Frame com Higgsfield, carousel, GSAP/ScrollTrigger, Three.js, Blender MCP, React Bits, React Spring, Shaders MCP), com breques para prompts visuais no ChatGPT/Gemini. Gera .padawan/artifacts/04-section-motion.md."
user-invocable: false
---

## Objetivo

Para cada seção da página: decupar o conteúdo de scan, decidir a direção visual e o motion que reforçam a mensagem, obter os insumos visuais com IAs externas e integrar no código com fallback leve. Uma seção por vez, sempre com a dupla decupagem → motion.

## Gatilho e Breque de IA

- **Claude Code executa:** leitura das seções existentes, ficha de decupagem, escolha técnica de motion, integração em código, extração de frames, fallback e atualização do STATUS.
- **BREQUE 1 — Copy de scan:** hat, título, subtítulo e CTA são escritos no **ChatGPT/Gemini**. PARE, entregue o prompt e aguarde.
- **BREQUE 2 — Prompts visuais:** frame inicial (A), frame final (B) e vídeo de transição são gerados por **ChatGPT/Gemini → Higgsfield**. PARE, entregue os prompts e aguarde os arquivos.
- **BREQUE 3 — 3D:** cena Blender é feita por um **agente especializado com Blender MCP**. PARE, entregue o briefing 3D e aguarde o `.glb`.
- **BREQUE 4 — Shader autoral:** se o Padawan quiser um shader feito sob medida via Shaders MCP, PARE e entregue o briefing ao agente que tem o MCP.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` (Fase 3 `CONCLUIDO`), a Arquitetura de Seções (`01-discovery.md`), a Camada 3 de `02-reverse-map.md` e a lista de componentes em `src/components/sections/`. Marque Fase 4 `EM_ANDAMENTO`.
2. **Decupagem (para todas as seções de uma vez):** monte a tabela `Seção | Objetivo | Hat (≤4 palavras) | Título (≤8) | Subtítulo (≤20) | CTA | Background | User Story atendida`.
3. **BREQUE 1:** salve em `.padawan/prompts/04-copy.md` e mostre:

   ```
   PAPEL: Copywriter de conversão especializado em leitura por scan no celular.
   CONTEXTO: <marca, público, frase "Como podemos...", brecha de diferenciação, tabela de decupagem>
   TAREFA: Para cada seção, escreva 2 opções de hat (≤4 palavras), título (≤8), subtítulo (≤20) e CTA (verbo + benefício, ≤4 palavras).
   FORMATO DE SAÍDA: tabela Markdown por seção.
   RESTRIÇÕES: português do Brasil, sem clichês ("soluções inovadoras"), sem ponto de exclamação, voz da marca: <3 palavras de mood>.
   ```

   Registre em "Aguardando IA externa" e PARE.
4. Com o retorno (salvo em `.padawan/prompts/04-copy-retorno.md`), o Padawan escolhe uma opção por seção. Substitua todos os `[copy fase 4]` no código.
5. **Direção visual por seção:** escolha uma técnica pela matriz e registre o motivo:

   | Técnica | Use quando | Custo mobile | Fallback obrigatório |
   |---------|-----------|--------------|----------------------|
   | Scroll Frame (Higgsfield) | Transformação/antes-depois, produto revelado | Alto | Poster estático (frame B) |
   | Carousel foto/card | Portfólio, depoimentos, planos | Baixo | Lista com scroll-snap nativo |
   | GSAP + ScrollTrigger | Reveal, pin, parallax, contadores | Médio | Conteúdo visível sem animação |
   | Three.js / R3F | Produto 3D interativo | Alto | Imagem renderizada do `.glb` |
   | Blender MCP | Cena 3D autoral para Three.js ou vídeo | Alto | Render PNG/AVIF |
   | React Bits | Microefeitos de texto/fundo prontos | Baixo/Médio | Texto estático |
   | React Spring | Interações físicas (hover, drag) | Baixo | Estado final sem física |
   | Shaders (Shaders MCP / GLSL) | Background vivo, gradiente animado | Médio/Alto | Gradiente CSS |

   Regra: no máximo 1 técnica de custo Alto acima da dobra e no máximo 2 na página inteira.
6. **BREQUE 2 (Scroll Frame):** salve em `.padawan/prompts/04-visual-<secao>.md` e mostre três prompts:
   - **Frame A (inicial):** sujeito, enquadramento, lente, luz, paleta (hex dos tokens), fundo, proporção 9:16 (mobile) e 16:9 (desktop), "sem texto na imagem".
   - **Frame B (final):** mesmo enquadramento e luz do Frame A, apenas a transformação desejada muda.
   - **Vídeo Higgsfield A → B:** duração 4–6 s, movimento de câmera (ex.: dolly-in lento), sem cortes, 24 fps, mesma proporção, usar Frame A como primeiro quadro e Frame B como último.
   PARE e aguarde o vídeo em `public/media/<secao>/source.mp4`.
7. **Frames:** com o vídeo recebido, extraia e otimize:
   `mkdir -p public/media/<secao>/frames-m public/media/<secao>/frames-d`
   `ffmpeg -i public/media/<secao>/source.mp4 -vf "fps=12,scale=720:-2" -c:v libwebp -quality 70 public/media/<secao>/frames-m/f_%04d.webp`
   `ffmpeg -i public/media/<secao>/source.mp4 -vf "fps=24,scale=1600:-2" -c:v libwebp -quality 75 public/media/<secao>/frames-d/f_%04d.webp`
   Mobile: ≤ 60 frames e ≤ 2,5 MB no total (`du -sh`). Se passar, reduza fps ou qualidade.
8. **BREQUE 3 (3D):** salve em `.padawan/prompts/04-3d-<secao>.md` um briefing para o agente Blender MCP: objeto, estilo, materiais (PBR, cores dos tokens), iluminação, câmera, limite de 50 mil triângulos, texturas ≤ 1024 px, export `.glb` com Draco, mais um render AVIF do ângulo principal para fallback. PARE e aguarde em `public/media/<secao>/`.
9. **Integração (Claude Code):**
   - Next.js: componentes em `src/components/motion/` com `"use client"`; GSAP via `@gsap/react` (`useGSAP`) e `gsap/ScrollTrigger` registrado uma vez; Three.js via `@react-three/fiber` + `@react-three/drei` carregado com `next/dynamic` e `ssr: false`; React Spring via `@react-spring/web`; React Bits copiando só o componente usado.
   - Astro: ilhas com `client:visible`; GSAP em `<script>` do componente.
   - Scroll Frame: `<canvas>` com pré-carregamento progressivo dos frames, desenho via `requestAnimationFrame`, ScrollTrigger com `scrub` e `pin`, e fonte trocada para `frames-m` abaixo de 768 px.
   - Toda técnica checa `window.matchMedia('(prefers-reduced-motion: reduce)')` e entrega o fallback da matriz.
   - Instale só o que for usado (ex.: `npm i gsap @gsap/react`).
10. Rode `npm run build`, confira em 360 px e 1440 px com e sem reduced motion (DevTools → Rendering → Emulate CSS prefers-reduced-motion).
11. Registre tudo no artefato, rode o Gate e atualize o STATUS.

Modelo do artefato `04-section-motion.md`:

```
# 04 — Seções e Motion
## Decupagem (tabela por seção, copy final escolhida)
## Direção visual por seção (técnica, motivo, custo, fallback)
## Insumos externos (prompt → arquivo recebido → caminho em public/media)
## Integração técnica (componentes criados, libs e versões)
## Testes (360 px, 1440 px, reduced motion, build)
## Decisões do Padawan
## Validação
```

## Perguntas Provocativas

- O que o visitante entende nessa seção se o motion não carregar? Se a resposta for "nada", a estrutura está errada.
- Esse scroll frame conta uma transformação real do cliente ou é só um efeito bonito?
- Qual CTA da página é o principal? Algum motion está roubando atenção dele?
- Seu Frame A e Frame B têm a mesma luz e enquadramento? Se não, o vídeo vai "pular".
- Você colocaria essa animação se o cliente pagasse pelo consumo de bateria do visitante?

## Critério de Conclusão (Gate)

- [ ] Todas as seções da arquitetura com decupagem completa e copy final.
- [ ] `grep -rn "\[copy fase 4\]" src` retorna vazio.
- [ ] Cada seção com técnica, motivo e fallback registrados; regra de no máximo 2 técnicas de custo Alto respeitada.
- [ ] Insumos externos recebidos e salvos nos caminhos registrados (ou técnica trocada por uma sem insumo, com motivo).
- [ ] Sequência de frames mobile ≤ 2,5 MB por seção.
- [ ] `prefers-reduced-motion` testado e fallback funcionando.
- [ ] `npm run build` sem erro.
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/04-section-motion.md` retorna vazio.
- [ ] Padawan validou no navegador.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/04-section-motion.md`
- Prompts: `.padawan/prompts/04-copy.md`, `04-visual-<secao>.md`, `04-3d-<secao>.md` e respectivos `-retorno.md`
- Mídia: `public/media/<secao>/`
- `.padawan/STATUS.md`: Fase 4 → `CONCLUIDO`, linhas de IA externa com retorno, `## Próximo comando` → `/padawan-seo-keywords`.
- Termine exibindo exatamente:

```
/padawan-seo-keywords
```
