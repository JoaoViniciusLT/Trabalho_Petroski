# Convenção de versionamento

## As 4 regras

### 1. Repositório

- Fica no GitHub: `JoaoViniciusLT/Trabalho_Petroski`.
- Os 5 integrantes são colaboradores com permissão de escrita, e o professor tem acesso de leitura.
- A `main` é a linha principal (*mainline*): ela sempre tem uma versão que funciona.
- Nenhum segredo (senha, token, `.env`) vai para o repositório; o `.gitignore` bloqueia `.env` e `node_modules/`.

### 2. Branches

- Uma branch por tarefa do backlog, criada a partir da `main` atualizada.
- Nome: `tipo/ID-descricao-curta`, em minúsculas e com hífens:
  - `feature/H-01-cadastrar-alimento`
  - `fix/H-06-gramas-negativas`
  - `docs/readme-execucao`
  - `arq/estrutura-camadas`
- Ninguém faz commit direto na `main`.
- Mensagem de commit no imperativo, citando a história: `H-01: adiciona validação de proteína`.

### 3. Fusão (merge)

- Só entra na `main` por **Pull Request**.
- O PR precisa de **1 aprovação de outro integrante** (não o autor) e do CI verde.
- Quem integra (clica em *merge*) é o **autor do PR**, depois da aprovação. O Arquiteto técnico (João Vinicius) pode bloquear um PR que fuja do ADR.
- Usamos *squash and merge*, e a branch é apagada depois da fusão.

### 4. Teste e lint antes de integrar

- Antes de abrir o PR, rodar localmente: `npm run lint` e `npm test`.
- O CI (GitHub Actions) roda em todo PR para a `main`: `npm ci`, `npm run lint` e `npm test`.
- PR com CI vermelho não pode ser integrado.

## Se duas pessoas alterarem o mesmo trecho (conflito)

1. Quem for integrar por último atualiza a sua branch com a `main` (`git pull origin main`).
2. Resolve o conflito conversando com quem fez a outra alteração, e não apaga o código do colega sem falar com ele.
3. Roda `npm run lint` e `npm test` de novo e envia.
4. Para evitar conflitos: tarefas pequenas, uma pessoa por história e PRs integrados rápido.

## Se o build quebrar na `main`

"Nunca quebre a construção" (Sommerville, 2019, p. 678).

1. Quem perceber avisa no grupo na hora.
2. O autor do último PR integrado corrige com prioridade máxima. Se não conseguir em até 1 hora, faz `git revert` do PR, e a `main` volta a funcionar.
3. Ninguém integra outro PR enquanto a `main` estiver quebrada.

## Quem responde pelo código integrado

**O integrante que abriu o PR responde pelo código integrado, mesmo que o código tenha sido gerado por um agente de IA; quem aprovou a revisão responde junto.**
