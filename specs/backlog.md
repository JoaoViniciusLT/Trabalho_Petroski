# Histórias e backlog

## Histórias de usuário

Cada integrante escreveu duas histórias. Formato: "Como …, quero …, para …".

| ID | Autor | História | Fluxo principal | Fluxo alternativo | Requisitos |
|---|---|---|---|---|---|
| H-01 | Matheus | Como pessoa que monta marmitas, quero cadastrar um alimento com nome, categoria e proteína por 100 g, para usá-lo nas minhas marmitas. | Preenche o formulário e salva; o alimento aparece na lista. | Campo vazio ou proteína inválida: o sistema mostra o erro e não salva. | RF-01, RNF-01 |
| H-02 | Matheus | Como pessoa que monta marmitas, quero ver a lista de alimentos cadastrados, para escolher o que vou usar. | Abre a tela de alimentos e vê nome, categoria e proteína. | Sem alimentos: mostra "Nenhum alimento cadastrado". | RF-01 |
| H-03 | João Vitor | Como pessoa que monta marmitas, quero editar um alimento, para corrigir um valor de proteína errado. | Altera os dados e salva; a lista e o total das marmitas mudam. | Valor inválido: mostra o erro e mantém o valor antigo. | RF-01, RF-04, RNF-01 |
| H-04 | João Vitor | Como pessoa que monta marmitas, quero excluir um alimento que não uso mais, para manter a lista limpa. | Exclui; o alimento some da lista. | Alimento usado em marmita: a exclusão é recusada com aviso. | RF-01, RF-05 |
| H-05 | João Vinicius | Como pessoa que monta marmitas, quero criar uma marmita com nome e dia da semana, para organizar a semana. | Informa nome e dia e salva; a marmita aparece na lista. | Dia inválido ou nome vazio: mostra o erro. | RF-02, RNF-01 |
| H-06 | João Vinicius | Como pessoa que monta marmitas, quero adicionar alimentos com a quantidade em gramas a uma marmita, para montar a refeição. | Escolhe o alimento, informa as gramas, e o item aparece na marmita. | Alimento repetido: soma as gramas; gramas inválidas: erro. | RF-03, RNF-01 |
| H-07 | Gabriel | Como pessoa que monta marmitas, quero ver a proteína total de cada marmita, para saber se ela atinge minha meta. | Abre a marmita e vê o total com 1 casa decimal. | Marmita sem itens: mostra 0,0 g. | RF-04, RNF-03, RNF-04 |
| H-08 | Gabriel | Como pessoa que monta marmitas, quero alterar a quantidade ou remover um alimento da marmita, para ajustar a refeição. | Muda as gramas ou remove o item; o total é recalculado. | Gramas inválidas: mantém o valor antigo e mostra o erro. | RF-03, RF-04 |
| H-09 | Samanta | Como pessoa que monta marmitas, quero ver as marmitas da semana agrupadas por dia, para planejar o que vou comer. | Lista mostra as marmitas de segunda a domingo, cada uma com seu total. | Dia sem marmita: mostra "sem marmita". | RF-02, RF-04, RNF-03 |
| H-10 | Samanta | Como pessoa que monta marmitas, quero editar ou excluir uma marmita, para corrigir o planejamento. | Edita nome/dia ou exclui; ao excluir, os itens dela também saem. | — | RF-02 |

## Backlog priorizado

Ordem definida pelo Product Owner (Matheus). Sem estimativa de horas nesta etapa.

| # | Descrição | Tipo | Sprint 1 |
|---|---|---|---|
| 1 | Montar a estrutura do projeto em camadas (Express, SQLite, Jest, ESLint) e o CI no GitHub Actions, conforme o ADR-001. | Arquitetura | ✅ |
| 2 | H-01: cadastrar alimento. | História | ✅ |
| 3 | H-05: criar marmita. | História | ✅ |
| 4 | H-06: adicionar alimento à marmita. | História | ✅ |
| 5 | H-07: ver a proteína total da marmita. | História | ✅ |
| 6 | H-02, H-03 e H-04: listar, editar e excluir alimentos. | História | ✅ |
| 7 | H-08 e H-10: ajustar itens e editar/excluir marmitas. | História | |
| 8 | H-09: visão da semana por dia. | História | |
| 9 | Escrever o README de execução (instalar, rodar, testar) e atualizar o diagrama de arquitetura. | Documentação | |

**Sprint 1 (2 semanas):** itens 1 a 6. O que não terminar volta ao backlog.

### Como sabemos que as histórias cobrem os requisitos essenciais?

Cada RF e RNF aparece em pelo menos uma história (coluna "Requisitos"). O núcleo do sistema (cadastrar alimento → criar marmita → adicionar item → ver total) está todo na Sprint 1.
