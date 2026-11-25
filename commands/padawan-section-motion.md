---
name: padawan-section-motion
description: "Executa a Fase 4 (Decupagem de seção + Direção visual e motion): copy de scan, Scroll Frame Higgsfield, carousel, GSAP, Three.js, Blender MCP, React Bits, React Spring e shaders. Gera .padawan/artifacts/04-section-motion.md."
argument-hint: "[observações opcionais]"
---

## Objetivo

Rodar a Fase 4 do Método Padawan Frame seguindo a skill `padawan-section-motion` à risca, do início ao gate, e deixar o artefato `.padawan/artifacts/04-section-motion.md` validado. Observações do Padawan para esta execução: $ARGUMENTS

## Gatilho e Breque de IA

- **Pré-condição:** Fase 3 (`padawan-build-next`) com status `CONCLUIDO` em `.padawan/STATUS.md`. Se não estiver, PARE, mostre o status atual e exiba o comando da fase pendente.
- **Claude Code executa:** os passos técnicos da skill `padawan-section-motion`, a escrita do artefato e a atualização do STATUS.
- **Breque:** ChatGPT/Gemini para copy de scan e prompts de frame A, frame B e vídeo Higgsfield; agente com Blender MCP para 3D; agente com Shaders MCP para shader autoral. Ao chegar no breque, salve o prompt em `.padawan/prompts/`, mostre-o em bloco de código, registre em "Aguardando IA externa" e aguarde o retorno.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md` e confira a pré-condição.
2. Se o artefato `.padawan/artifacts/04-section-motion.md` já existir, retome de onde parou em vez de recomeçar; se a fase estiver `CONCLUIDO`, pergunte se o Padawan quer revisar.
3. Marque a Fase 4 como `EM_ANDAMENTO`.
4. Carregue e siga a skill `padawan-section-motion` (Passo a Passo Operacional completo, incluindo os breques).
5. Ao final, rode `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/04-section-motion.md` e o Critério de Conclusão da skill item por item.
6. Peça a validação explícita do Padawan.

## Perguntas Provocativas

- O que o visitante entende nesta seção se o motion não carregar?
- Use as demais perguntas da skill `padawan-section-motion` e registre as respostas no artefato.

## Critério de Conclusão (Gate)

- [ ] Todos os itens do gate da skill `padawan-section-motion` marcados.
- [ ] `.padawan/artifacts/04-section-motion.md` existe e o grep de pendências retorna vazio.
- [ ] Padawan validou no chat.

## Saída Obrigatória

- Artefato: `.padawan/artifacts/04-section-motion.md`
- `.padawan/STATUS.md`: Fase 4 → `CONCLUIDO`, checklist de gate marcado, `Última atualização` com a data de hoje, `## Próximo comando` → `/padawan-seo-keywords`.
- Oriente o commit no GitHub Desktop: `padawan: fase 4 concluída (padawan-section-motion)`.
- Termine exibindo exatamente:

```
/padawan-seo-keywords
```
