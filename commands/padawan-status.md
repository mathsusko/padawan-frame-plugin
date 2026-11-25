---
name: padawan-status
description: "Mostra o painel do Método Padawan Frame, revalida os artefatos contra o STATUS.md e exibe exatamente o próximo comando."
allowed-tools: Bash, Read, Edit, Glob, Grep
---

## Objetivo

Responder em poucos segundos "onde estou e o que faço agora", corrigindo divergências entre `.padawan/STATUS.md` e os arquivos reais em `.padawan/artifacts/`.

## Gatilho e Breque de IA

- **Claude Code executa:** leitura de `.padawan/`, checagem de pendências, correção do STATUS.
- **Breque:** nenhum. Se houver linha na tabela "Aguardando IA externa" sem retorno, lembre o Padawan de colar o retorno e mostre o caminho do prompt em `.padawan/prompts/`.

## Passo a Passo Operacional

1. Se `.padawan/STATUS.md` não existir, responda apenas: rode `/padawan-start <nome-do-projeto>`.
2. Leia a tabela de fases.
3. Para cada fase, rode:
   `test -s .padawan/artifacts/0N-*.md && grep -cE "TODO|TBD|\?\?\?|\{\{" .padawan/artifacts/0N-*.md`
4. Corrija divergências:
   - `CONCLUIDO` sem artefato ou com pendência → volte para `EM_ANDAMENTO` e liste as linhas pendentes.
   - Artefato existente com status `PENDENTE` → mude para `EM_ANDAMENTO`.
5. Determine o próximo comando: a primeira fase não `CONCLUIDO` na ordem 1 → 7. Se todas estiverem concluídas, o próximo comando é `/padawan-deploy` para um novo ciclo de release.
6. Atualize `Última atualização` e `## Próximo comando` se algo mudou.
7. Mostre a tabela compacta, as pendências (no máximo 5 linhas) e os prompts externos aguardando retorno.

## Perguntas Provocativas

Não se aplica. Este comando é só leitura e correção de estado.

## Critério de Conclusão (Gate)

- [ ] Todo `CONCLUIDO` tem artefato sem pendência.
- [ ] `## Próximo comando` coincide com a primeira fase não concluída.

## Saída Obrigatória

- Nenhum artefato novo. Atualiza `.padawan/STATUS.md` apenas se havia divergência.
- Termine exibindo exatamente o próximo comando em bloco de código, por exemplo:

```
/padawan-reverse-html
```
