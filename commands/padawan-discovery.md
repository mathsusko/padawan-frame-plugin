---
name: padawan-discovery
description: "Executa a Fase 1 (Discovery): briefing, Double Diamond, benchmarking de 3 referências e User Stories. Gera .padawan/artifacts/01-discovery.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 1 do Método Padawan Frame seguindo a skill `padawan-discovery` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/01-discovery.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** `.padawan/STATUS.md` existente (criado por `/padawan-start`). Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-discovery`, a escrita do artefato e a atualização do STATUS.
- **Breque:** ChatGPT/Gemini para Double Diamond, benchmarking e User Stories. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/01-discovery.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 1 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-discovery` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/01-discovery.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- Qual problema do público esta página resolve que os concorrentes ignoram?
- Use as demais perguntas da skill `padawan-discovery` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-discovery` marcados.
- [ ] `.padawan/artifacts/01-discovery.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/01-discovery.md`
- `.padawan/STATUS.md`: Fase 1 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-reverse-html`.
- Oriente o commit no GitHub Desktop: `padawan: fase 1 concluída (padawan-discovery)`.
- Termine exibindo exatamente:

```
/padawan-reverse-html
```
