# Plano de teste do UIMarmita

Plano de uma página (Sommerville, 2019, p. 207): o que testar, em que níveis, qual item segue TDD, como registrar os resultados e quando testar.

## 1. O que será testado

- **Regras de negócio** da camada de serviço: validação dos campos (RNF-01) e cálculo da proteína total (RF-04, RNF-04).
- **CRUD de alimentos e marmitas** (RF-01, RF-02) e **itens da marmita** (RF-03), incluindo a recusa de excluir alimento em uso (RF-05).
- **Persistência** no SQLite com chaves estrangeiras (RNF-02).
- **Desempenho** da listagem de marmitas (RNF-03).
- **Usabilidade** da tela de montar marmita (RNF-05), de forma manual.

Fora do plano: login, calorias e outros nutrientes, pois estão fora do escopo.

## 2. Níveis de teste

| Nível | O que cobre | Ferramenta | Automatizado |
|---|---|---|---|
| Unidade | Serviços (`AlimentoService`, `MarmitaService`) com repositórios falsos: validação e cálculo | Jest | Sim |
| Componentes | Serviço + repositório + SQLite em memória: CRUD, chaves estrangeiras, RF-05 | Jest | Sim |
| Sistema | Rotas HTTP de ponta a ponta (status 200/400, JSON) e tempo de resposta (RNF-03) | Jest + Supertest | Sim |
| Aceitação | Critérios das histórias conferidos pelo PO na tela; teste de usabilidade (RNF-05) | Manual, roteiro abaixo | Não |

Todos os testes automatizados rodam no CI (GitHub Actions) com `npm test` em todo Pull Request. A cobertura da camada de serviço deve ser ≥ 70% (RNF-06).

## 3. Item do backlog com teste escrito antes do código (TDD)

**Item 5 do backlog, H-07: ver a proteína total da marmita.** O cálculo tem entrada e saída bem definidas, então é o melhor candidato a TDD (Sommerville, 2019, p. 219):

1. Escrever os testes de `MarmitaService.calcularProteina` com os casos da seção 5 (49,0 g; marmita vazia = 0,0 g; arredondamento).
2. Rodar `npm test` e **confirmar que falham**, guardando a saída no PR.
3. Implementar o mínimo para passar, refatorar e rodar tudo de novo.

**Pior candidato a TDD:** o item 9 (README de execução) e o RNF-05 (usabilidade), porque não têm saída que um teste automatizado consiga verificar. Eles são conferidos manualmente.

## 4. Registro dos resultados

- **Automatizados:** o resultado de cada execução fica no log do GitHub Actions do PR. O link do log vai na descrição do PR.
- **Manuais e de aceitação:** registrados na tabela abaixo, uma linha por critério, preenchida pela Samanta (testes) ou pelo PO.

| Data | História | Critério / requisito testado | Resultado esperado | Resultado obtido | Aprovado? | Quem testou |
|---|---|---|---|---|---|---|
| — | — | — | — | — | — | — |

A tabela começa vazia porque ainda não há código; a primeira linha entra na Sprint 1.

## 5. Critérios de aceitação das 3 histórias mais prioritárias

Escritos pelo grupo, não pelo agente. "Auto" = automatizável (Jest no CI). Ordem do backlog: H-01, H-05 e H-06.

| História | # | Critério de aceitação | Tipo | Auto |
|---|---|---|---|---|
| **H-01** Cadastrar alimento (RF-01, RNF-01) | 1 | Cadastrar "Frango", categoria "Carne", 31 g/100 g faz o alimento aparecer na listagem com esses três dados. | Funcional | Sim |
| | 2 | Nome vazio, proteína negativa ou acima de 100 são recusados com HTTP 400 e mensagem que diz o campo errado, e nada é gravado no banco. | **Não funcional** (RNF-01) | Sim |
| | 3 | Depois de reiniciar o servidor, o alimento cadastrado continua na listagem. | **Não funcional** (RNF-02) | Sim |
| **H-05** Criar marmita (RF-02, RNF-01) | 1 | Criar "Almoço segunda" com dia "segunda-feira" faz a marmita aparecer na listagem, com 0,0 g de proteína. | Funcional | Sim |
| | 2 | Dia fora dos 7 dias da semana ou nome vazio é recusado com HTTP 400 e mensagem clara. | **Não funcional** (RNF-01) | Sim |
| **H-06** Adicionar alimento à marmita (RF-03, RF-04, RNF-01) | 1 | Adicionar 150 g de Frango (31 g/100 g) e 100 g de Arroz (2,5 g/100 g) faz a marmita mostrar **49,0 g** de proteína. | Funcional | Sim |
| | 2 | Adicionar de novo um alimento que já está na marmita soma as gramas no item existente, sem criar linha duplicada. | Funcional | Sim |
| | 3 | Com 50 marmitas de 5 itens cada, a listagem com os totais responde em menos de 500 ms (média de 10 chamadas). | **Não funcional** (RNF-03) | Sim |

Os critérios vêm antes do código para evitar testes que só confirmam o que o código já faz (Liu et al., 2025, p. 11).

## 6. Cronograma previsto

| Quando | O que | Responsável |
|---|---|---|
| Sprint 1, semana 1 | CI rodando lint e testes (item 1 do backlog); testes de unidade de H-01 e H-05 | João Vinicius, Gabriel |
| Sprint 1, semana 2 | TDD de H-07 e H-06; testes de componentes com SQLite em memória | Gabriel, Samanta |
| Fim da Sprint 1 | Aceitação manual das histórias da sprint, registrada na seção 4 | Matheus (PO), Samanta |
| Sprint 2 | Teste de desempenho (RNF-03), usabilidade (RNF-05) e regressão completa antes da entrega final | Samanta, João Vitor |
