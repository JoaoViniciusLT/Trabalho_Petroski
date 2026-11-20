# Requisitos do sistema Marmita

Cada requisito tem um identificador e um critério de aceitação verificável.
Os requisitos não são independentes: o RF-04 depende dos RF-01 a RF-03.

## Requisitos funcionais (RF)

| ID | Requisito | Critério de aceitação |
|---|---|---|
| RF-01 | Cadastrar, listar, editar e excluir alimentos (nome, categoria e proteína por 100 g). | Um alimento cadastrado com nome "Frango", categoria "Carne" e 31 g de proteína por 100 g aparece na listagem com esses dados. Depois de editado, a listagem mostra o valor novo; depois de excluído, ele não aparece mais. |
| RF-02 | Cadastrar, listar, editar e excluir marmitas (nome e dia da semana). | Uma marmita "Almoço segunda", com dia "segunda-feira", aparece na listagem. O dia só aceita os 7 dias da semana. |
| RF-03 | Adicionar alimentos a uma marmita com a quantidade em gramas, alterar essa quantidade e remover o item. | Ao adicionar 150 g de Frango à marmita, o item aparece na marmita com 150 g. Ao alterar para 200 g, o item mostra 200 g; ao removê-lo, o item some e o alimento continua cadastrado. |
| RF-04 | Calcular e exibir a proteína total de cada marmita. | Uma marmita com 150 g de Frango (31 g/100 g) e 100 g de Arroz (2,5 g/100 g) mostra **49,0 g** de proteína (46,5 + 2,5). Uma marmita sem itens mostra 0,0 g. |
| RF-05 | Impedir a exclusão de um alimento que está em alguma marmita. | Ao tentar excluir o Frango, que está em uma marmita, o sistema recusa e mostra a mensagem "Alimento usado em 1 marmita; remova-o das marmitas antes de excluir". |

## Requisitos não funcionais (RNF)

| ID | Tipo | Requisito | Critério de aceitação (mensurável) |
|---|---|---|---|
| RNF-01 | Produto (confiabilidade) | Validar campos obrigatórios e números positivos. | 100% dos casos inválidos do plano de teste (nome vazio, proteína negativa ou acima de 100, gramas ≤ 0 ou acima de 2000) são rejeitados com HTTP 400 e uma mensagem que diz qual campo está errado, sem gravar nada no banco. |
| RNF-02 | Organizacional (implementação) | Salvar os dados em banco de dados relacional (SQLite), com chaves estrangeiras ativas. | Depois de reiniciar o servidor, os dados cadastrados continuam lá. Inserir um item com `alimento_id` inexistente é recusado pelo banco. |
| RNF-03 | Produto (desempenho) | Listar marmitas com a proteína total calculada de forma rápida. | Com 200 alimentos e 50 marmitas de 5 itens cada no banco, `GET /marmitas` responde em **menos de 500 ms** (média de 10 chamadas, medida no teste automatizado). |
| RNF-04 | Produto (precisão) | Exibir a proteína com uma casa decimal, sem erro de arredondamento. | Para os 10 casos de cálculo do plano de teste, o valor exibido é igual ao calculado à mão, arredondado para 1 casa decimal (diferença de 0,0 g). |
| RNF-05 | Produto (usabilidade) | Interface simples, que funcione no computador. | Funciona no Chrome e no Firefox atuais, em tela de 1366×768, sem barra de rolagem horizontal. Uma pessoa que nunca usou o sistema monta uma marmita com 3 alimentos em até 2 minutos. |
| RNF-06 | Organizacional (manutenção) | Manter testes automatizados na regra de negócio. | Cobertura de linhas da camada de serviços ≥ 70% (relatório do `jest --coverage` no CI). |
