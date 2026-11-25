---
name: padawan-seo-keywords
description: "Fase 5 do Método Padawan Frame. Consulta fontes abertas de keywords com timeout curto e fallback local, filtra por intenção, define headings e gera meta tags (title, description, Open Graph, canonical, JSON-LD) no código. Gera .padawan/artifacts/05-seo-meta.md."
user-invocable: false
---

## Objetivo

Garantir que a página seja encontrada pelas buscas certas: keywords com intenção compatível com o objetivo da página, hierarquia de headings coerente e metadados técnicos completos e válidos.

## Gatilho e Breque de IA

- **Claude Code executa:** toda a fase (consultas HTTP, filtragem, headings, metadados no código, validação). SEO técnico é trabalho do Claude Code.
- **BREQUE:** opcional. Se o Padawan quiser reescrever headings com mais apelo, gere um prompt curto para ChatGPT/Gemini com as keywords obrigatórias e o limite de caracteres, e aguarde.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` (Fase 4 `CONCLUIDO`), o briefing (`01-discovery.md`) e a copy final (`04-section-motion.md`). Marque Fase 5 `EM_ANDAMENTO`.
2. Defina 3 a 5 termos semente a partir de oferta + público + cidade/região (se negócio local).
3. Consulte fontes abertas, cada uma com `--max-time 5`, e salve o bruto em `.padawan/refs/seo/`:
   - Google Autocomplete: `curl -s --max-time 5 "https://suggestqueries.google.com/complete/search?client=firefox&hl=pt-BR&gl=br&q=<termo-url-encoded>"`
   - DuckDuckGo Autocomplete: `curl -s --max-time 5 "https://duckduckgo.com/ac/?q=<termo>&kl=br-pt"`
   - Datamuse (relacionadas, útil para termos em inglês): `curl -s --max-time 5 "https://api.datamuse.com/words?ml=<termo>&max=20"`
   - Wikipedia (entidades e sinônimos): `curl -s --max-time 5 "https://pt.wikipedia.org/w/api.php?action=opensearch&search=<termo>&limit=10&format=json"`
4. **Fallback local** (se todas falharem ou retornarem vazio): gere variações combinando semente × modificadores de intenção: transacional (`preço`, `orçamento`, `contratar`, `agendar`, `comprar`), comercial (`melhor`, `vs`, `avaliação`), local (`perto de mim`, `em <cidade>`, `<bairro>`), informacional (`como`, `o que é`, `quanto custa`). Registre no artefato que o fallback foi usado.
5. Deduplique e classifique cada keyword por intenção (transacional, comercial, local, informacional). Mantenha as que batem com o objetivo da página; uma landing de conversão prioriza transacional + local.
6. Escolha: 1 keyword principal, 3 a 5 secundárias, 5 a 10 long tails para FAQ/subtítulos.
7. **Headings:** um `h1` com a keyword principal (natural, ≤ 60 caracteres), `h2` por seção com secundárias quando fizer sentido, sem pular níveis. Atualize o código mantendo a copy de scan aprovada (ajustes mínimos, sinalizados ao Padawan).
8. **Metadados no código:**
   - Next.js: `export const metadata` em `src/app/layout.tsx`/`page.tsx` com `title` (50–60 caracteres), `description` (140–160), `alternates.canonical`, `openGraph` (title, description, url, siteName, locale `pt_BR`, type, images 1200×630), `twitter` (`summary_large_image`), `robots`. Crie `src/app/sitemap.ts` e `src/app/robots.ts`.
   - Astro: tags em `BaseLayout.astro` (`<title>`, `<meta name="description">`, `<link rel="canonical">`, `og:*`, `twitter:*`) e `@astrojs/sitemap` (`npx astro add sitemap`) com `site` no `astro.config`.
   - JSON-LD em `<script type="application/ld+json">`: `Organization` ou `LocalBusiness` (nome, url, logo, telefone, endereço, horário, sameAs) + `WebSite`; `FAQPage` se houver FAQ; `Product`/`Service` se aplicável. Use apenas dados reais do briefing.
   - Imagem OG 1200×630 em `public/og.jpg` (se não existir, use o Frame B ou peça ao Padawan).
9. Valide: `npm run build`, depois confira o HTML gerado com `curl -s localhost:3000 | grep -E "<title>|description|canonical|og:|ld\+json"` (porta 4321 no Astro). Valide o JSON-LD com `node -e` fazendo `JSON.parse` do bloco extraído, e oriente o Padawan a testar no Rich Results Test do Google após o deploy.
10. Registre no artefato, rode o Gate e atualize o STATUS.

Modelo do artefato `05-seo-meta.md`:

```
# 05 — SEO e Meta
## Sementes e fontes consultadas (status de cada fonte, fallback usado?)
## Keywords (tabela Keyword | Intenção | Uso: principal/secundária/long tail | Onde aparece)
## Headings (árvore h1 > h2 > h3)
## Metadados (title, description, canonical, OG, Twitter, robots, sitemap)
## JSON-LD (bloco final)
## Validação técnica (saídas dos comandos)
## Decisões do Padawan
## Validação
```

## Perguntas Provocativas

- Quem busca essa keyword quer comprar agora ou só aprender? Sua página atende essa pessoa?
- Seu `h1` soa como gente falando ou como robô de SEO?
- Se o seu title aparecer no Google ao lado de 9 concorrentes, por que alguém clicaria no seu?
- Algum dado do JSON-LD foi inventado? Dado falso em schema pode virar penalidade.

## Critério de Conclusão (Gate)

- [ ] Status de cada fonte registrado (OK, timeout ou vazio) e fallback documentado quando usado.
- [ ] 1 principal, 3–5 secundárias e 5–10 long tails classificadas por intenção.
- [ ] Exatamente um `h1` na página e níveis de heading sem saltos.
- [ ] Title 50–60 e description 140–160 caracteres; canonical, OG (com imagem 1200×630), Twitter e robots presentes no HTML gerado.
- [ ] JSON-LD válido (`JSON.parse` sem erro) e com dados reais.
- [ ] Sitemap e robots gerados.
- [ ] `npm run build` sem erro.
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/05-seo-meta.md` retorna vazio.
- [ ] Padawan validou.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/05-seo-meta.md`
- Brutos: `.padawan/refs/seo/` (no `.gitignore`)
- Código: metadados, JSON-LD, sitemap e robots.
- `.padawan/STATUS.md`: Fase 5 → `CONCLUIDO`, `## Próximo comando` → `/padawan-perf-guard`.
- Termine exibindo exatamente:

```
/padawan-perf-guard
```
