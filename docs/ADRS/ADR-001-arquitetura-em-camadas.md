# ADR-001: Arquitetura em camadas com banco de dados relacional

- **Status:** Proposto (passa a Aceito quando o grupo aprovar o Pull Request)
- **Requisitos ligados:** RNF-01 (validar campos obrigatórios e números positivos) e RNF-02 (salvar os dados em banco relacional SQLite, com chaves estrangeiras ativas)

## Contexto

O UIMarmita tem dois CRUDs (alimentos e marmitas) ligados por uma tabela de itens, e calcula a proteína total de cada marmita. O sistema é pequeno, será feito por um grupo de iniciantes e precisa ser entregue em poucas semanas. Os dados têm relações claras (uma marmita tem vários itens e cada item aponta para um alimento), e o RNF-02 exige banco relacional. O RNF-01 exige validação dos campos antes de salvar qualquer coisa. Precisamos de uma organização de código simples, que facilite dividir o trabalho e testar as regras.

## Decisão

Adotar uma aplicação única (monolito) em Node.js com Express, organizada em três camadas, com dependência apenas de cima para baixo:

1. **Apresentação:** telas e rotas. Só recebe e mostra dados; não tem regra de negócio nem SQL.
2. **Serviço:** regras do sistema. Aqui ficam a validação (RNF-01) e o cálculo da proteína da marmita (soma de gramas / 100 x proteína por 100 g).
3. **Repositório:** acesso ao banco relacional SQLite (RNF-02), com tabelas `alimento`, `marmita` e `item_marmita` ligadas por chave estrangeira. Todo SQL fica só nessa camada.

O padrão de projeto pretendido é o **Repository**: cada tabela tem um repositório que esconde os detalhes do banco, e o serviço depende só dessa interface.

## Como a decisão atende os RNF

- **RNF-01:** a validação fica concentrada na camada de serviço, em um só lugar, e vale para qualquer tela que use o serviço. Dados inválidos são recusados antes de chegar ao banco.
- **RNF-02:** a camada de repositório é a única que conhece o SQLite, o que mantém os dados organizados em tabelas relacionadas e fáceis de consultar para o cálculo da proteína.

## Consequências

**Positivas**
- Cada pessoa do grupo pode trabalhar em uma camada sem atrapalhar as outras.
- As regras (validação e cálculo) podem ser testadas sem abrir tela nem banco real.
- Trocar o banco ou a interface mexe em uma camada só.

**Negativas**
- Uma mudança pequena, como incluir um novo campo no alimento, atravessa as três camadas e a tabela.
- Há risco de as camadas virarem só repasse de dados, com código repetido.
- Se alguém colocar SQL na tela ou regra no repositório, a vantagem da separação se perde.

## Desvantagem do estilo e como o grupo a mitiga

A principal desvantagem da arquitetura em camadas é o acoplamento entre elas: mudar uma coisa exige mexer em várias camadas, e a separação só funciona se todos respeitarem as regras. O grupo mitiga assim:

- Regra de dependência escrita: a apresentação só chama o serviço, o serviço só chama o repositório, e é proibido SQL fora do repositório.
- Revisão em todo Pull Request, conferindo essa regra (item da Definição de pronto).
- Testes automatizados do serviço com Jest e lint com ESLint no CI, para que mudanças em uma camada não quebrem as regras sem aviso.
- A regra de arquitetura entra nas instruções dos agentes (E10), para o agente também respeitar as camadas.

## Alternativas consideradas

- **Tudo em um arquivo só, sem camadas:** mais rápido no começo, mas mistura tela, regra e SQL e dificulta dividir o trabalho e testar.
- **Microsserviços:** é exagero para um sistema pequeno e aumentaria muito a complexidade para o prazo do grupo.
