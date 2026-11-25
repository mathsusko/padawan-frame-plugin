---
name: padawan-deploy
description: "Executa a Fase 7 (Testes, conformidade e deploy): 3G, mobile, teclado, contraste, formulários, scroll rápido, LGPD e políticas, build e deploy. Gera .padawan/artifacts/07-deploy-log.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 7 do Método Padawan Frame seguindo a skill `padawan-deploy` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/07-deploy-log.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** Fase 6 (`padawan-perf-guard`) com status `CONCLUIDO` em `.padawan/STATUS.md`. Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-deploy`, a escrita do artefato e a atualização do STATUS.
- **Breque:** ChatGPT/Gemini para rascunho de textos legais (não substitui revisão jurídica formal); Padawan para login na Vercel/Netlify e para autorizar a publicação. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/07-deploy-log.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 7 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-deploy` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/07-deploy-log.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- Uma pessoa que só usa teclado consegue completar a ação principal?
- Use as demais perguntas da skill `padawan-deploy` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-deploy` marcados.
- [ ] `.padawan/artifacts/07-deploy-log.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/07-deploy-log.md`
- `.padawan/STATUS.md`: Fase 7 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-status`.
- Oriente o commit no GitHub Desktop: `padawan: fase 7 concluída (padawan-deploy)`.
- Termine exibindo exatamente:

```
/padawan-status
```
