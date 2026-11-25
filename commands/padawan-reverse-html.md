---
name: padawan-reverse-html
description: "Executa a Fase 2 (Engenharia reversa de HTML): estrutura, componentes e shaders/scroll das referências, com decisão de manter ou descartar. Gera .padawan/artifacts/02-reverse-map.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 2 do Método Padawan Frame seguindo a skill `padawan-reverse-html` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/02-reverse-map.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** Fase 1 (`padawan-discovery`) com status `CONCLUIDO` em `.padawan/STATUS.md`. Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-reverse-html`, a escrita do artefato e a atualização do STATUS.
- **Breque:** Somente para leitura estética opcional da referência no ChatGPT/Gemini. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/02-reverse-map.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 2 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-reverse-html` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/02-reverse-map.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- O que dessa referência é estrutura que serve ao seu projeto e o que é só assinatura dela?
- Use as demais perguntas da skill `padawan-reverse-html` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-reverse-html` marcados.
- [ ] `.padawan/artifacts/02-reverse-map.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/02-reverse-map.md`
- `.padawan/STATUS.md`: Fase 2 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-build-next`.
- Oriente o commit no GitHub Desktop: `padawan: fase 2 concluída (padawan-reverse-html)`.
- Termine exibindo exatamente:

```
/padawan-build-next
```
