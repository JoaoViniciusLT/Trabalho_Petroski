# Instruções para o agente (GitHub Copilot)

## Objetivo do sistema

**UIMarmita** é um sistema para planejar as marmitas da semana. O usuário cadastra alimentos (nome, categoria e proteína por 100 g), monta marmitas com a quantidade em gramas de cada alimento, e o sistema calcula a proteína total de cada marmita.

Antes de qualquer tarefa, leia:
- `specs/requisitos.md`: RF e RNF, com critérios de aceitação.
- `specs/backlog.md`: as histórias e a ordem do backlog.
- `docs/adr/ADR-001-arquitetura-em-camadas.md`: a decisão de arquitetura.
- `docs/plano-de-teste.md`: os critérios de aceitação de cada história.

## Arquitetura (regra do ADR-001)

**Arquitetura em camadas (ADR-001): apresentação → serviço → repositório → banco SQLite. A dependência vai só de cima para baixo. Nenhum acesso ao banco (SQL) fora da camada de repositórios, e nenhuma regra de negócio (como o cálculo da proteína) fora da camada de serviços.**

| Camada | Pasta | Pode chamar | Não pode |
|---|---|---|---|
| Apresentação (rotas e telas) | `src/routes/` e `public/` | serviços | acessar repositórios ou o banco; validar ou fazer contas |
| Serviços (regras) | `src/services/` | repositórios | conhecer `req`/`res` do Express; escrever SQL |
| Repositórios (dados) | `src/repositories/` | `src/db/` | conter regra de negócio |
| Banco | `src/db/` | SQLite | — |

- A camada de cima só chama a camada logo abaixo.
- A proteína total **nunca** é gravada no banco: ela é calculada na leitura (ver a armadilha em `specs/mini-spec.md`).
- Arredondar a proteína para 1 casa decimal **só no resultado final**.

## Padrão de projeto

**Facade:** cada serviço (`AlimentoService`, `MarmitaService`) é a fachada que a apresentação usa. A rota chama um único método do serviço, por exemplo `marmitaService.adicionarItem(marmitaId, alimentoId, gramas)`, e o serviço coordena a validação, os repositórios e o cálculo. Não crie atalhos da apresentação direto para o repositório. A camada de repositório tem um repositório por tabela (`AlimentoRepository`, `MarmitaRepository`, `ItemMarmitaRepository`), mas o padrão escolhido no ADR é o Facade.

## Comandos

| Ação | Comando |
|---|---|
| Instalar dependências | `npm ci` |
| Rodar o servidor | `npm start` |
| Rodar os testes | `npm test` |
| Testes com cobertura | `npm test -- --coverage` |
| Lint | `npm run lint` |

Antes de dizer que uma tarefa terminou, rode `npm run lint` e `npm test`. Os dois têm que passar.

## Convenções

- **Linguagem:** JavaScript (Node.js), módulos CommonJS, Express e SQLite.
- **Nomes:** código, variáveis e funções em português, sem acento (`proteinaTotal`, `diaSemana`). Classes em PascalCase (`MarmitaService`) e arquivos em camelCase (`marmitaService.js`).
- **Tabelas:** `alimento`, `marmita` e `item_marmita` (no singular, como no ADR-001), em snake_case.
- **Estilo:** o que o ESLint do projeto exige; 2 espaços, aspas simples e ponto e vírgula.
- **Testes:** Jest, em `tests/`, com o nome `<arquivo>.test.js`. Cada teste cita a história no `describe`, por exemplo `describe('H-06: adicionar alimento à marmita', ...)`.
- **Erros de validação:** HTTP 400 com `{ "erro": "<mensagem que diz qual campo está errado>" }` (RNF-01).
- **Commits:** `H-XX: descrição no imperativo`.

## Proibições

- Não colocar segredos no repositório (senhas, tokens, `.env`).
- Não alterar código de produção sem um teste que cubra a mudança.
- Não apagar, pular (`.skip`) ou enfraquecer testes existentes para fazer o CI passar.
- Não desativar regras do ESLint.
- Não escrever SQL fora de `src/repositories/`.
- Não adicionar dependências novas no `package.json` sem pedir ao grupo.
- Não alterar `specs/`, `docs/adr/` nem os critérios de aceitação: eles são escritos pelo grupo.
- Não fazer commit direto na `main` nem integrar PR.
- Não implementar nada que esteja fora do escopo (login, calorias, receitas, lista de compras).

## Agentes do projeto

- **`@implementador`** (`.github/agents/implementador.agent.md`): implementa uma história por vez, com TDD.
- **`@revisor`** (`.github/agents/revisor.agent.md`): dá um parecer sobre o PR, sem editar código.

Escolhemos dois agentes em vez de um para separar quem gera de quem valida e para limitar o "raio de dano": o revisor só tem permissão de leitura. Os artigos não provam que isso melhora a qualidade, e dois agentes gastam mais créditos; por isso o revisor roda uma vez por PR.

## Quem responde pelo código integrado

**O integrante que abriu o PR responde pelo código integrado, mesmo que o código tenha sido gerado por um agente de IA; quem aprovou a revisão responde junto.** O agente propõe; o grupo decide.
