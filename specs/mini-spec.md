# Mini-spec: "Montar marmita"

## Ticket vago (como chegou)

> "Montar marmita."

Assim, o ticket não diz para quem é, quais dados entram, como saber se está pronto, nem o que acontece com valores inválidos. Um desenvolvedor (ou um agente) tende a interpretar do jeito mais simples (Sommerville, 2019, p. 90).

## Ticket reescrito

**Objetivo e para quem:** a pessoa que prepara as marmitas da semana quer escolher alimentos já cadastrados, informar quantos gramas de cada um vão na marmita e ver na hora a proteína total, sem fazer a conta à mão.

**Requisitos ligados:** RF-02, RF-03, RF-04, RNF-01, RNF-04.

### Critérios de aceitação (verificáveis)

1. Na tela de uma marmita, é possível escolher um alimento cadastrado e informar a quantidade em gramas; o item aparece na lista da marmita.
2. A quantidade aceita números inteiros de 1 a 2000. Valores fora disso, vazios ou não numéricos são recusados com a mensagem "Quantidade deve ser entre 1 e 2000 g" e nada é gravado.
3. Se o mesmo alimento for adicionado de novo à mesma marmita, as gramas são **somadas** ao item existente (não cria uma linha duplicada).
4. Depois de cada inclusão, alteração ou remoção, a proteína total da marmita é recalculada e exibida com 1 casa decimal.
5. Exemplo de conferência: 150 g de Frango (31 g/100 g) + 100 g de Arroz (2,5 g/100 g) = **49,0 g**.

### RNF mensurável

- **RNF-03:** a listagem com o total recalculado responde em menos de 500 ms com 50 marmitas de 5 itens cada.

### Contexto de arquitetura

- Arquitetura em camadas (ver ADR-001): **apresentação → serviço → repositório → SQLite**, com o padrão Facade nos serviços.
- A conta da proteína fica **só** em `MarmitaService` (camada de serviço). A apresentação não faz conta e não acessa o banco.
- O acesso à tabela `item_marmita` fica só em `ItemMarmitaRepository`.
- Tabelas: `alimento (id, nome, categoria, proteina_100g)`, `marmita (id, nome, dia_semana)`, `item_marmita (id, marmita_id, alimento_id, gramas)`, com chaves estrangeiras.

### Armadilha

**Não guardar a proteína total no banco.** É tentador criar uma coluna `proteina_total` em `marmita`, mas se alguém editar a proteína de um alimento (RF-01), todas as marmitas que o usam ficam com o valor antigo. O total deve ser **calculado na leitura**, a partir dos itens e dos valores atuais dos alimentos.

Armadilha secundária: somar números com casas decimais em JavaScript dá erro de ponto flutuante (`0.1 + 0.2 = 0.30000000000000004`). Arredondar **só no final**, para 1 casa decimal.

### Fora do escopo deste ticket

- Copiar uma marmita para outro dia.
- Calorias e outros nutrientes.
