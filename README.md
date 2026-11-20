# UIMarmita

Sistema simples para planejar as refeições da semana. O usuário cadastra os alimentos (nome, categoria e proteína por 100 g) e monta suas marmitas escolhendo os alimentos e a quantidade em gramas de cada um. O sistema calcula automaticamente a proteína total de cada marmita.

**Linguagem:** Node.js (Express + SQLite; testes com Jest; lint com ESLint).

## Tema e escopo

- **Para quem:** pessoas que preparam marmitas para a semana e querem acompanhar quanta proteína estão comendo.
- **Qual problema:** calcular à mão a proteína de cada marmita é trabalhoso e sujeito a erro; o sistema faz a conta (soma de `gramas / 100 × proteína por 100 g`).
- **O que não entra:** login e múltiplos usuários, receitas, outros nutrientes (calorias, carboidratos, gorduras) e lista de compras.

O sistema é formado por dois CRUDs (alimentos e marmitas) ligados por uma tabela de itens, em um banco de dados relacional.

## Equipe

| Nome completo | GitHub | Papel | O que cobre bem | Por que esse papel |
|---|---|---|---|---|
| João Vinicius Lameu de Oliveira | [@JoaoViniciusLT](https://github.com/JoaoViniciusLT) | Arquiteto técnico | programar | Criou o repositório e a base do projeto; toma as decisões técnicas do ADR. |
| João Vitor Pereira Kramek | [@Kramekjvp](https://github.com/Kramekjvp) | Scrum Master | programar, documentar | Organiza o quadro e as entregas, cobra prazos e remove obstáculos do grupo. |
| Matheus Henrique de Paiva Pinto | [@MatheusPaiva-ai](https://github.com/MatheusPaiva-ai) | Product Owner | negociar, documentar | Fala pelo usuário da marmita, prioriza o backlog e aceita as histórias. |
| Gabriel Barreto | [@B3rretog](https://github.com/B3rretog) | Desenvolvedor | programar, testar | Implementa as histórias e escreve os testes antes do código (TDD). |
| Samanta Barth | [@samanta-barth](https://github.com/samanta-barth) | Desenvolvedora / testes | testar, documentar | Mantém o plano de teste e confere os critérios de aceitação. |

As quatro competências (negociar, programar, testar e documentar) estão cobertas. "Negociar" tem só uma pessoa: o Scrum Master substitui o PO quando ele faltar.

## Definição de pronto

Uma tarefa só está pronta quando:

1. Os critérios de aceitação da história foram verificados e marcados na tabela do [plano de teste](docs/plano-de-teste.md).
2. Os testes automatizados (`npm test`) e o lint (`npm run lint`) passam no CI, sem erros.
3. A funcionalidade nova tem teste escrito antes do código, e esse teste falhou antes da implementação.
4. Outro integrante, que não é o autor, revisou e aprovou o Pull Request.
5. O trabalho foi integrado na `main` por Pull Request, sem quebrar o build.
6. A documentação afetada (spec, ADR ou README) foi atualizada no mesmo PR.
7. Todo uso de IA na tarefa foi registrado em `docs/registro-ia.md` (o que foi pedido, se foi aceito ou corrigido, e quem usou).

## Entrega parcial

O índice de todos os entregáveis está em [ENTREGAPARCIAL01.md](ENTREGAPARCIAL01.md).
