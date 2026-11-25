---
name: padawan-seo-keywords
description: "Executa a Fase 5 (SEO): keywords de fontes abertas com timeout e fallback local, headings e meta tags com Open Graph, canonical e JSON-LD. Gera .padawan/artifacts/05-seo-meta.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 5 do Método Padawan Frame seguindo a skill `padawan-seo-keywords` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/05-seo-meta.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** Fase 4 (`padawan-section-motion`) com status `CONCLUIDO` em `.padawan/STATUS.md`. Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-seo-keywords`, a escrita do artefato e a atualização do STATUS.
- **Breque:** Opcional: ChatGPT/Gemini para reescrever headings com as keywords obrigatórias. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/05-seo-meta.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 5 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-seo-keywords` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/05-seo-meta.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- Quem busca a sua keyword principal quer comprar agora ou só aprender?
- Use as demais perguntas da skill `padawan-seo-keywords` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-seo-keywords` marcados.
- [ ] `.padawan/artifacts/05-seo-meta.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/05-seo-meta.md`
- `.padawan/STATUS.md`: Fase 5 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-perf-guard`.
- Oriente o commit no GitHub Desktop: `padawan: fase 5 concluída (padawan-seo-keywords)`.
- Termine exibindo exatamente:

```
/padawan-perf-guard
```
