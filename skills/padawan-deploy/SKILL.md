---
name: padawan-deploy
description: "Fase 7 do Método Padawan Frame. Roda testes desafiadores (3G, viewport mobile, teclado, contraste, formulários, scroll rápido), checa conformidade e acessibilidade (LGPD, políticas, cookies, dados da empresa, direitos autorais, alt), faz build de produção e deploy. Gera .padawan/artifacts/07-deploy-log.md."
user-invocable: false
---

## Objetivo

Publicar a página só depois de ela sobreviver a testes de uso real e cumprir o mínimo de conformidade legal e de acessibilidade, deixando registrado o que foi testado, o que foi corrigido e onde está no ar.

## Gatilho e Breque de IA

- **Claude Code executa:** testes automatizáveis, auditoria de acessibilidade, checagem de páginas legais, build, deploy via CLI e log.
- **BREQUE 1 — Textos legais:** rascunhos de Política de Privacidade, Termos de Uso e Política de Reembolso podem ser gerados no **ChatGPT/Gemini** com os dados reais da empresa. PARE, entregue o prompt e aguarde. Aviso obrigatório: os textos são ponto de partida e **não substituem revisão jurídica formal**.
- **BREQUE 2 — Deploy:** criar conta, conectar o repositório na Vercel/Netlify e configurar domínio é ação do Padawan. PARE e passe o passo a passo quando for necessário login.
- O deploy em produção só acontece depois que o Padawan disser explicitamente "pode publicar".

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` (Fase 6 `CONCLUIDO`). Marque Fase 7 `EM_ANDAMENTO`. Suba o servidor de produção local (`npm run build && npm run start`, ou `npx astro preview`).
2. **Testes desafiadores** (registre resultado e correção de cada um):
   1. **Rede 3G:** DevTools → Network → "Slow 3G" (ou Lighthouse com `--throttling.cpuSlowdownMultiplier=4`). Conteúdo principal e CTA aparecem antes do motion terminar de carregar?
   2. **Viewport mobile:** 360×640, 390×844 e 768×1024; sem scroll horizontal (`document.documentElement.scrollWidth <= innerWidth`).
   3. **Teclado:** navegar só com Tab/Shift+Tab/Enter/Esc; foco sempre visível, ordem lógica, menu e modais fecham com Esc, sem armadilha de foco.
   4. **Contraste:** `npx -y @axe-core/cli http://localhost:3000 --tags wcag2a,wcag2aa --exit` e corrigir violações de contraste e nomes acessíveis.
   5. **Formulários:** campo vazio, e-mail inválido, telefone com e sem DDD, envio duplo, mensagem de sucesso e de erro anunciadas (`aria-live`), labels visíveis.
   6. **Scroll rápido:** rolar a página inteira em 2 segundos, ida e volta; scroll frames e pins não podem travar, piscar ou deixar seção em branco.
3. **Conformidade e acessibilidade** (checklist; cada item com caminho do arquivo ou URL):
   - LGPD em formulários: finalidade da coleta informada, checkbox de consentimento não pré-marcado quando houver uso para marketing, link para a Política de Privacidade junto ao botão, coleta só dos dados necessários.
   - Páginas `/politica-de-privacidade`, `/termos-de-uso` e `/politica-de-reembolso` (esta quando houver venda) acessíveis no rodapé.
   - Aviso de cookies/tracking: se houver Google Analytics, Meta Pixel, Tag Manager ou similar, banner com aceitar/recusar e scripts carregados só após consentimento.
   - Dados da empresa no rodapé: razão social, CNPJ, endereço (se aplicável) e contato.
   - Direitos autorais: `© <ano> <empresa>` e licenças de fontes, imagens, vídeos e modelos 3D verificadas (registre a origem de cada asset).
   - `alt` em todas as imagens informativas e `alt=""` nas decorativas: `grep -rnE "<img|<Image" src | grep -v "alt="` deve retornar vazio.
4. **BREQUE 1:** se faltar algum texto legal, salve em `.padawan/prompts/07-legal.md` e mostre:

   ```
   PAPEL: Redator especializado em documentos de conformidade para sites brasileiros (LGPD, CDC).
   CONTEXTO: Empresa <razão social>, CNPJ <...>, contato <...>, site <url>. Coleta: <campos do formulário>. Ferramentas de tracking: <lista>. Venda online: <sim/não, prazo de arrependimento>.
   TAREFA: Rascunhar Política de Privacidade, Termos de Uso e Política de Reembolso.
   FORMATO DE SAÍDA: Markdown, um documento por título, com data de vigência.
   RESTRIÇÕES: português do Brasil, linguagem clara, citar LGPD (Lei 13.709/2018) e CDC (Lei 8.078/1990) quando aplicável, incluir aviso de que o texto deve ser revisado por advogado.
   ```

   PARE e aguarde. Com o retorno, crie as páginas e mantenha o aviso de revisão jurídica no artefato.
5. **Build de produção:** `npm run build` limpo, sem warnings de erro; `npm audit --omit=dev --audit-level=high` registrado.
6. **Deploy** (após "pode publicar"):
   - Vercel (padrão para Next.js): `npx vercel login` (Padawan), `npx vercel link`, `npx vercel --prod`. Alternativa sem CLI: importar o repositório em vercel.com/new; cada push na `main` pelo GitHub Desktop publica sozinho.
   - Netlify (bom para Astro): `npx netlify-cli login`, `npx netlify-cli init`, `npx netlify-cli deploy --prod --dir=dist`.
   - Configure a variável do domínio final e atualize `canonical`, `og:url`, `metadataBase`/`site` se a URL mudou.
7. **Pós-deploy:** abrir a URL em 390 px, rodar `curl -sI <url>` (status 200, HTTPS), conferir `/sitemap.xml` e `/robots.txt`, rodar Lighthouse mobile na URL pública e testar o JSON-LD no Rich Results Test.
8. Oriente commit e push no GitHub Desktop: `padawan: fase 7 concluída (deploy <url>)`.
9. Registre no artefato, rode o Gate e atualize o STATUS.

Modelo do artefato `07-deploy-log.md`:

```
# 07 — Log de Deploy
## Testes desafiadores (tabela Teste | Resultado | Correção | Status)
## Conformidade e acessibilidade (checklist com caminhos)
## Aviso jurídico (textos não substituem revisão jurídica formal)
## Build de produção (saída resumida, npm audit)
## Deploy (plataforma, URL, data/hora, commit)
## Pós-deploy (status HTTP, Lighthouse público, sitemap, robots, Rich Results)
## Pendências pós-lançamento aceitas pelo Padawan
## Validação
```

## Perguntas Provocativas

- Uma pessoa que só usa teclado ou leitor de tela consegue completar a ação principal? Você tentou?
- Seu formulário pede algum dado que você não usa? Por que pedir?
- Se o cliente receber uma notificação da ANPD amanhã, o que nesta página o protege e o que o expõe?
- Todas as imagens, fontes e vídeos têm licença para uso comercial? De onde vieram?
- O site sobe sozinho a cada push, ou o deploy depende de alguém lembrar de rodar um comando?

## Critério de Conclusão (Gate)

- [ ] Os 6 testes desafiadores executados, cada um com resultado e status OK.
- [ ] axe sem violações de nível A/AA (ou exceção justificada).
- [ ] Checklist de conformidade completo, com páginas legais publicadas e aviso de revisão jurídica registrado.
- [ ] Nenhuma `<img>`/`<Image>` sem `alt`.
- [ ] Build de produção limpo.
- [ ] URL pública responde 200 via HTTPS; sitemap e robots acessíveis.
- [ ] Padawan disse "pode publicar" antes do deploy (registrar data/hora).
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/07-deploy-log.md` retorna vazio.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/07-deploy-log.md`
- Prompts: `.padawan/prompts/07-legal.md` e `07-legal-retorno.md` (se usados)
- `.padawan/STATUS.md`: Fase 7 → `CONCLUIDO`, todos os gates marcados, URL de produção no cabeçalho, `## Próximo comando` → `/padawan-status`.
- Termine exibindo exatamente:

```
/padawan-status
```
