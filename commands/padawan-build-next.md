---
name: padawan-build-next
description: "Executa a Fase 3 (Base front-end): Next.js + Tailwind + shadcn/ui ou Astro, com tokens e seções em ordem. Gera .padawan/artifacts/03-build-base.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 3 do Método Padawan Frame seguindo a skill `padawan-build-next` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/03-build-base.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** Fase 2 (`padawan-reverse-html`) com status `CONCLUIDO` em `.padawan/STATUS.md`. Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-build-next`, a escrita do artefato e a atualização do STATUS.
- **Breque:** Nenhum breque de IA; para apenas se faltar Node.js 20+. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/03-build-base.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 3 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-build-next` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/03-build-base.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- Esta página precisa de JavaScript em todas as seções?
- Use as demais perguntas da skill `padawan-build-next` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-build-next` marcados.
- [ ] `.padawan/artifacts/03-build-base.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/03-build-base.md`
- `.padawan/STATUS.md`: Fase 3 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-section-motion`.
- Oriente o commit no GitHub Desktop: `padawan: fase 3 concluída (padawan-build-next)`.
- Termine exibindo exatamente:

```
/padawan-section-motion
```
