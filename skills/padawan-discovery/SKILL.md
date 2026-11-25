---
name: padawan-discovery
description: "Fase 1 do Método Padawan Frame. Conduz briefing, Design Thinking (Double Diamond), benchmarking de 3 referências e User Stories, delegando a parte criativa ao ChatGPT/Gemini e consolidando em .padawan/artifacts/01-discovery.md. Use quando o projeto está na Fase 1 ou o Padawan pede discovery, briefing ou benchmarking."
user-invocable: false
---

## Objetivo

Transformar o pedido do cliente em um entendimento verificável: quem é o público, qual problema a página resolve, como 3 concorrentes/referências fazem e quais User Stories a página precisa atender. Sem isso, as fases seguintes copiam estética sem estratégia.

## Gatilho e Breque de IA

- **Claude Code executa:** leitura de `.padawan/STATUS.md`, coleta do briefing em perguntas curtas, montagem dos prompts, consolidação do retorno no artefato, validação e atualização do STATUS.
- **BREQUE (obrigatório):** Double Diamond, benchmarking e User Stories são feitos no **ChatGPT ou Gemini** (de preferência com navegação web ligada). O Claude Code PARA após gerar os prompts e aguarda o Padawan colar o retorno. Não pesquise nem escreva personas por conta própria.

## Passo a Passo Operacional

1. Leia `.padawan/STATUS.md`. Se não existir, mande rodar `/padawan-start`. Marque a Fase 1 como `EM_ANDAMENTO`.
2. Colete o briefing com no máximo 8 perguntas, uma mensagem só:
   1. Empresa/marca e segmento.
   2. Oferta principal (produto/serviço e preço, se houver).
   3. Público-alvo (quem decide e quem usa).
   4. Objetivo da página (lead, venda, agendamento, autoridade).
   5. Ação principal esperada (CTA) e canal de conversão (WhatsApp, formulário, checkout).
   6. 3 URLs de referência ou concorrentes (se não tiver, peça o segmento e a cidade).
   7. Restrições: prazo, identidade visual existente, conteúdo/fotos disponíveis.
   8. Mood desejado em 3 palavras.
3. Salve o briefing bruto na seção `## 1. Briefing` do artefato.
4. Gere e salve em `.padawan/prompts/01-discovery.md` o prompt abaixo, preenchido com o briefing, e mostre-o em bloco de código:

   ```
   PAPEL: UX Researcher e estrategista de produto digital sênior.
   CONTEXTO: <briefing resumido em até 10 linhas>
   TAREFA:
   1) Double Diamond: Descobrir (5 dores e 5 desejos do público), Definir (1 frase "Como podemos..."), Desenvolver (3 abordagens de página), Entregar (abordagem escolhida e por quê).
   2) Benchmarking de 3 referências (<URL1>, <URL2>, <URL3>). Para cada uma: pontos fortes (3), falhas no mobile e de clareza (3), brecha de diferenciação (1 frase acionável).
   3) User Stories no formato Scrum "Como <persona>, quero <ação>, para <benefício>", 6 a 10 histórias, cada uma com 2 critérios de aceite e prioridade MoSCoW.
   4) Proposta de arquitetura de seções da landing (ordem e objetivo de cada seção).
   FORMATO DE SAÍDA: Markdown com os títulos "Double Diamond", "Benchmarking", "User Stories", "Arquitetura de Seções".
   RESTRIÇÕES: português do Brasil, frases curtas, foco mobile-first, sem jargão.
   ```

5. Registre em `## Aguardando IA externa` do STATUS: `| 1 | ChatGPT/Gemini | .padawan/prompts/01-discovery.md | (pendente) |`. PARE e aguarde.
6. Quando o Padawan colar o retorno: salve-o bruto em `.padawan/prompts/01-discovery-retorno.md` e preencha a coluna "Retorno colado em".
7. Consolide no artefato (seções 2 a 5 do modelo abaixo), cortando redundâncias. Não invente dados que não vieram do briefing ou do retorno.
8. Faça as Perguntas Provocativas, registre as respostas na seção `## 6. Decisões do Padawan`.
9. Rode o Gate. Se passar, marque `CONCLUIDO`, atualize o STATUS e exiba o próximo comando.

Modelo do artefato `01-discovery.md`:

```
# 01 — Discovery
## 1. Briefing
## 2. Double Diamond (Descobrir / Definir / Desenvolver / Entregar)
## 3. Benchmarking (tabela: Referência | Pontos fortes | Falhas mobile/clareza | Brecha)
## 4. User Stories (tabela: ID | História | Critérios de aceite | MoSCoW)
## 5. Arquitetura de Seções (ordem, objetivo, CTA)
## 6. Decisões do Padawan
## 7. Validação (data, quem validou)
```

## Perguntas Provocativas

- Qual das 3 brechas de diferenciação você aposta e qual você descarta? Por quê?
- Se o visitante sair em 5 segundos, qual User Story falhou?
- Qual seção da arquitetura você cortaria sem perder conversão?
- A referência mais bonita é também a mais clara no celular? O que você copia dela e o que não?

## Critério de Conclusão (Gate)

- [ ] `.padawan/artifacts/01-discovery.md` existe com as 7 seções preenchidas.
- [ ] Benchmarking com exatamente 3 referências, cada uma com pontos fortes, falhas mobile/clareza e brecha.
- [ ] No mínimo 6 User Stories com critérios de aceite e MoSCoW.
- [ ] Frase "Como podemos..." definida e abordagem escolhida justificada.
- [ ] Seção 6 com as respostas do Padawan.
- [ ] `grep -nE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/01-discovery.md` retorna vazio.
- [ ] Padawan confirmou "validado" no chat (registrar na seção 7).

## Saída Obrigatória

- Artefato: `.padawan/artifacts/01-discovery.md`
- Prompts: `.padawan/prompts/01-discovery.md` e `.padawan/prompts/01-discovery-retorno.md`
- `.padawan/STATUS.md`: Fase 1 → `CONCLUIDO`, gate marcado, linha de IA externa com retorno, `## Próximo comando` → `/padawan-reverse-html`.
- Termine exibindo exatamente:

```
/padawan-reverse-html
```
