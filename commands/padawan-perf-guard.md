---
name: padawan-perf-guard
description: "Executa a Fase 6 (Guardião de performance): budgets de mídia/JS, scroll frames, reduced motion, fallbacks de 3D/shaders e escaneabilidade no mobile. Gera .padawan/artifacts/06-perf-report.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 6 do Método Padawan Frame seguindo a skill `padawan-perf-guard` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/06-perf-report.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** Fase 5 (`padawan-seo-keywords`) com status `CONCLUIDO` em `.padawan/STATUS.md`. Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-perf-guard`, a escrita do artefato e a atualização do STATUS.
- **Breque:** Nenhum breque criativo; para e entrega briefing de reexport se um vídeo ou .glb precisar ser refeito mais leve. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/06-perf-report.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 6 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-perf-guard` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/06-perf-report.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- Se você tivesse que cortar uma única coisa para ganhar 1 segundo, o que seria?
- Use as demais perguntas da skill `padawan-perf-guard` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-perf-guard` marcados.
- [ ] `.padawan/artifacts/06-perf-report.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/06-perf-report.md`
- `.padawan/STATUS.md`: Fase 6 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-deploy`.
- Oriente o commit no GitHub Desktop: `padawan: fase 6 concluída (padawan-perf-guard)`.
- Termine exibindo exatamente:

```
/padawan-deploy
```
