---
name: padawan-reverse-html
description: "Fase 2 do Método Padawan Frame. Faz engenharia reversa de HTML das referências de trás para frente (estrutura e wireframe, componentes e variações, shaders/scroll/transições) e decide com o Padawan o que manter ou descartar. Gera .padawan/artifacts/02-reverse-map.md."
user-invocable: false
---

## Objetivo

Entender como as referências escolhidas na Fase 1 foram construídas, camada por camada, e converter isso em um mapa de decisões para o nosso projeto: o que reaproveitar como padrão, o que adaptar ao mood e o que descartar por não servir à intenção ou ao mobile.

## Gatilho e Breque de IA

- **Claude Code executa:** baixar e ler HTML/CSS/JS das referências, medir espaçamentos e grids, identificar bibliotecas de motion, montar o mapa e atualizar o STATUS. Esta é uma fase essencialmente do Claude Code.
- **BREQUE:** só se o Padawan quiser uma leitura estética/emocional da referência (ex.: "que sensação essa página passa?"). Nesse caso gere um prompt curto para ChatGPT/Gemini com print da referência e aguarde.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md`; confirme Fase 1 `CONCLUIDO`. Leia a seção Benchmarking de `01-discovery.md` para obter as 3 URLs. Marque Fase 2 `EM_ANDAMENTO`.
2. Para cada referência, baixe o HTML com timeout curto e salve fora do repositório final:
   `mkdir -p .padawan/refs && curl -sL --max-time 15 -A "Mozilla/5.0" <URL> -o .padawan/refs/ref-N.html`
   Se falhar (SPA, bloqueio), peça ao Padawan para salvar a página (Cmd+S, "Página completa") em `.padawan/refs/` ou usar o DevTools → Elements → Copy outerHTML. Acrescente `.padawan/refs/` ao `.gitignore`.
3. **Camada 1 — Estrutura e wireframe cru** (leia de trás para frente: do layout para o detalhe):
   - Grid e container: largura máxima, colunas, gutters.
   - Margins, paddings e gaps recorrentes (converta para escala de 4 px / classes Tailwind).
   - Hierarquia tipográfica: famílias, tamanhos por breakpoint (h1, h2, corpo, legenda), pesos, line-height, tracking.
   - Ordem e altura relativa das seções. Desenhe um wireframe ASCII mobile de cada referência.
4. **Camada 2 — Componentes e variações conceituais:** liste botões, cards, navbar, badges, inputs, carrosséis. Para cada um, registre a variação e o mood que transmite: sólido, gradiente, glass (backdrop-blur), glow (box-shadow colorido), radius (0 = sério/editorial, 8–12 = neutro/SaaS, full = amigável).
5. **Camada 3 — Shaders, scroll e transições:** procure no código `gsap`, `ScrollTrigger`, `lenis`, `locomotive`, `three`, `@react-three`, `canvas`, `webgl`, `framer-motion`, `react-spring`, `<video`, sequências de imagens (`frame_0001`), `@keyframes`, `IntersectionObserver`. Descreva cada efeito: gatilho, duração aproximada, propriedade animada e custo no mobile (baixo/médio/alto).
6. Monte a tabela de decisão: `Elemento | Referência | Manter | Adaptar | Descartar | Motivo ligado à intenção`.
7. Faça as Perguntas Provocativas e registre as respostas na tabela e em `## Decisões do Padawan`.
8. Extraia os tokens propostos (cores, fontes, escala de espaçamento, radius, sombras) em uma seção `## Tokens propostos` pronta para a Fase 3.
9. Rode o Gate, atualize o STATUS e exiba o próximo comando.

Modelo do artefato `02-reverse-map.md`:

```
# 02 — Mapa de Engenharia Reversa
## Referências analisadas (URL, data, método de captura)
## Camada 1 — Estrutura e wireframe (por referência, com wireframe ASCII mobile)
## Camada 2 — Componentes e variações (tabela Componente | Variação | Mood | Radius)
## Camada 3 — Shaders, scroll e transições (tabela Efeito | Lib | Gatilho | Custo mobile)
## Tabela de decisão (Manter / Adaptar / Descartar)
## Tokens propostos
## Decisões do Padawan
## Validação
```

## Perguntas Provocativas

- Esse efeito existe para contar a história da marca ou só para impressionar? Ele sobrevive em um celular de entrada?
- Qual radius combina com o mood que definimos na Fase 1? O que um radius errado comunicaria?
- Se você remover todo o motion dessa referência, a hierarquia ainda funciona? O que isso diz sobre a estrutura?
- Glass e glow ajudam a leitura do CTA ou competem com ele?
- Qual componente da referência é a "assinatura" dela e por isso NÃO devemos copiar?

## Critério de Conclusão (Gate)

- [ ] As 3 referências analisadas nas 3 camadas (ou motivo registrado se alguma não pôde ser capturada).
- [ ] Wireframe ASCII mobile de pelo menos uma referência e da proposta do projeto.
- [ ] Tabela de decisão com pelo menos 10 elementos e todos com motivo.
- [ ] Seção `Tokens propostos` completa (cores com hex, fontes, escala de espaço, radius, sombras).
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/02-reverse-map.md` retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/02-reverse-map.md`
- Capturas brutas: `.padawan/refs/` (no `.gitignore`)
- `.padawan/STATUS.md`: Fase 2 → `CONCLUIDO`, `## Próximo comando` → `/padawan-build-next`.
- Termine exibindo exatamente:

```
/padawan-build-next
```
