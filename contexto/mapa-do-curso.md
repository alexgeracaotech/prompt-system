# Mapa do Curso

Índice das quarenta e oito aulas do curso de desenvolvimento web. Relaciona, para cada aula, o tema e os conceitos tratados.

Constitui a fonte da ordem do conteúdo. Os conceitos declarados nas aulas anteriores à aula em curso são de conhecimento do interlocutor. Os conceitos declarados nas aulas posteriores são desconhecidos e não podem ser empregados, mencionados ou pressupostos.

O marcador `(r)` indica conceito tratado em nível de reconhecimento. A ausência de marcador indica nível ativo.

---

## Estrutura

| Etapa | Aulas | Projeto |
|---|---|---|
| Fundamentos | 1 a 8 | Simba |
| Ferramental | 9 e 10 | Simba |
| `v0.1.0` — Arquitetura base e entrega contínua | 11 a 17 | Gefina |
| `v0.2.0` — Persistência e consulta de dados | 18 a 23 | Gefina |
| `v0.3.0` — Operações de escrita e camada de acesso a dados | 24 a 30 | Gefina |
| `v0.4.0` — Segunda entidade e sistema de design | 31 a 36 | Gefina |
| `v0.5.0` — Autenticação e controle de acesso | 37 a 42 | Gefina |
| `v1.0.0` — Consulta avançada e integração contínua | 43 a 46 | Gefina |
| Encerramento | 47 e 48 | Gefina |

---

## Etapa Fundamentos

- **Projeto:** Simba

  O Simba é um simulador bancário operado por caixas de diálogo do navegador. Identifica quem o utiliza, apresenta um menu de operações, consulta saldo, registra depósitos e saques, e mantém o histórico das movimentações realizadas na sessão.
  
  O domínio bancário foi escolhido por três razões. É familiar a qualquer pessoa, o que dispensa a explicação do problema antes da explicação da solução. Suas operações exigem exatamente os fundamentos que a etapa constrói: decisão para validar valores, repetição para manter o menu ativo, agrupamento para organizar as operações. E o resultado é verificável sem critério subjetivo — o saldo está correto ou não está.
  
  O Simba não persiste dados, não autentica quem o utiliza e não se comunica com sistema algum. As caixas de diálogo substituem a interface porque construir interface exigiria conhecimento ainda indisponível, e o objeto desta etapa é o raciocínio de programação, não a apresentação.
  
  O Simba constitui o primeiro item do repositório de quem percorre o curso, e é publicado ao final da etapa seguinte.
- **Ambiente:** Playground do TypeScript
- **Comandos de sistema:** não

| # | Tema | Conceitos |
|---|---|---|
| 1 | Lógica de programação e ambiente de execução | lógica, algoritmo, lógica de programação, sequência de passos, entrada, processamento, saída, noção de programa, instrução, ponto e vírgula, origem do JavaScript, motor JavaScript, execução restrita a JavaScript no navegador, propósito do TypeScript, conversão para JavaScript, Playground, painel de JavaScript gerado, modo `strict` como configuração, `alert` como saída, `prompt` como entrada, comentário de linha, comentário de bloco |
| 2 | Variáveis, identificadores e tipos primitivos | `const`, `let`, reatribuição, regras de identificadores, camelCase, PascalCase, snake_case, SCREAMING_SNAKE_CASE, `string`, `number`, `boolean`, `null`, `undefined`, `typeof`, tipagem estática e dinâmica, inferência de tipo, anotação explícita, leitura de união de tipos, tipo inferido de `prompt` |
| 3 | Operadores e composição de texto | concatenação com `+`, sequências de escape, `\n`, operadores aritméticos, precedência, parênteses, atribuição composta, operadores relacionais, `===`, `!==`, `==` (r), `&&`, `\|\|`, `!`, expressão booleana |
| 4 | Estruturas de decisão | `if`, `else`, `else if`, bloco, aninhamento, truthy e falsy, valor padrão com `\|\|`, estreitamento de tipo, `switch`, `case`, `break` em `switch`, `default` |
| 5 | Estruturas de repetição e conversão de tipo | `while`, variável de controle, laço infinito e interrupção, `do...while`, `break` em laço, `continue`, `Number()`, `NaN`, comparação com `NaN`, `confirm` como entrada |
| 6 | Funções, escopo e tipagem de assinatura | declaração de função, parâmetro, argumento, invocação, `alert`, `prompt` e `confirm` como funções, `return`, retorno como interrupção, parâmetro com valor padrão, expressão de função, escopo léxico, escopo de bloco, escopo de função, `var` (r), variável global e seu risco, tipagem de parâmetro, tipagem de retorno, `void` |
| 7 | Objetos literais e agrupamento | objeto literal, propriedade, acesso por ponto, acesso por colchete, alteração de propriedade, acréscimo de propriedade, anotação de forma embutida, método, sintaxe abreviada de método, valor primitivo e valor de referência, encapsulamento automático de primitivos, `Number.isNaN` (r), `window` como objeto global, objeto como agrupamento de dado, objeto como agrupamento de comportamento |
| 8 | Arrays, índices e percurso | array literal, índice, base zero, `length` como propriedade, leitura por índice, índice inexistente e `undefined`, atribuição por índice, acréscimo em `array[array.length]`, array de objetos, anotação de tipo de array por extenso, array como valor de referência, `for`, inicialização, condição, incremento, contador, percurso por índice, percurso condicional, acumulação numérica, acumulação de texto, construção de novo array dentro do laço |

---

## Etapa Ferramental

- **Projeto:** Simba
- **Ambiente:** Editor local
- **Comandos de sistema:** sim

| # | Tema | Conceitos |
|---|---|---|
| 9 | Linha de comando e ambiente de desenvolvimento | terminal e shell, PowerShell, Bash, Zsh (r), prompt de comando, comando, opção e argumento, sistema de arquivos, diretório de trabalho, caminho absoluto, caminho relativo, `.`, `..`, `pwd`, `ls`, `cd`, `mkdir`, extensão de arquivo, instalação do VS Code, pasta como projeto, explorador de arquivos, terminal integrado, `code .`, paleta de comandos (r) |
| 10 | Controle de versão e repositório remoto | problema resolvido pelo controle de versão, histórico de alterações, instalação do Git, `git config`, `user.name`, `user.email`, `git init`, arquivo oculto, diretório `.git`, diretório de trabalho, área de preparo, repositório local, `git status`, `git add`, `git commit`, mensagem descritiva, `git log` (r), `git diff` (r), conta no GitHub, repositório remoto, criação de repositório vazio, autenticação por token de acesso pessoal, `git remote add`, `origin`, `git push`, endereço compartilhável do Playground |

---

## `v0.1.0` — Arquitetura base e entrega contínua

- **Projeto:** Gefina

  O Gefina é um sistema de gestão de contas a receber. Registra os clientes de uma organização e as faturas emitidas contra esses clientes, e apresenta a visão consolidada da situação financeira que desses registros decorre. Destina-se ao uso interno de uma equipe: cada integrante mantém sua própria carteira de clientes, e todos visualizam os registros de toda a organização.
  
  O sistema compreende autenticação com sessão e proteção de rotas, cadastro e manutenção de clientes e de faturas, listagens com busca, ordenação e paginação, painel de indicadores consolidados, e administração das contas de acesso.
  
  O domínio foi escolhido porque impõe as exigências que um sistema em operação impõe, e cada exigência corresponde a uma competência a formar. A necessidade de identificar quem opera conduz à autenticação. A distinção entre o que cada integrante pode fazer conduz à autorização. O volume de registros conduz à recuperação parcial de dados. A dependência entre clientes e faturas conduz às restrições de integridade. Nenhuma dessas competências é exercitada em domínio artificial.
  
  O Gefina não possui cadastro público de usuários, recuperação de senha, envio de arquivo de imagem nem representação gráfica de séries temporais. As exclusões constam da especificação do produto, acompanhadas da razão de cada uma.
  
  O Gefina é construído em seis entregas sucessivas, cada uma concluída com uma versão publicada e funcional. Constitui o segundo item do repositório de quem percorre o curso, e a entrega final.
  
  A descrição completa do sistema — domínio, modelo de dados, permissões, regras de negócio, contrato da API, interface e dados iniciais — consta da especificação do produto, que é a fonte autoritativa de toda decisão a seu respeito.
- **Ambiente:** Editor local
- **Comandos de sistema:** sim, exceto aula 11
- **Abertura de ciclo:** aula 11
- **Encerramento de ciclo:** aula 17

| # | Tema | Conceitos |
|---|---|---|
| 11 | Levantamento de requisitos e modelagem conceitual | necessidade de negócio, contas a receber, requisito funcional, requisito não funcional, escopo e exclusão de escopo, entidade, atributo, relacionamento, cardinalidade, regra de negócio, dado armazenado e dado derivado, critério de aceite, comportamento observável, Markdown, título, ênfase, lista, bloco de código, link, README, milestone, issue, versionamento semântico, `MAJOR`, `MINOR`, `PATCH`, release |
| 12 | Arquitetura cliente-servidor e protocolo HTTP | cliente e servidor, requisição e resposta, URL, esquema, host, porta, caminho, método HTTP, `GET`, `POST`, `PUT`, `DELETE`, cabeçalho, corpo, código de status, famílias 2xx, 4xx e 5xx, JSON, `Content-Type`, API, REST (r), painel de rede do navegador |
| 13 | Node.js, módulos e servidor HTTP | Node.js, execução fora do navegador, instalação, `node`, ausência de `window` e de `alert`, `console.log`, módulo, `import`, `export`, `export default`, `import type`, npm, `package.json`, `npm init`, dependência, `npm install`, `node_modules`, `.gitignore`, script npm, execução direta de TypeScript, `--watch`, `tsc --noEmit`, `node:http`, `createServer`, `listen`, roteamento manual, `JSON.stringify` |
| 14 | Express e estrutura da API | Express, instalação, `app`, roteamento por método e caminho, `req`, `res`, `res.json`, parâmetro de rota, middleware, `express.json`, ordem de execução, `Router`, separação de rotas em módulo, dados em memória, template literal, `type`, `interface`, propriedade opcional, união de literais |
| 15 | HTML e React no navegador | HTML, marcação, elemento, tag, atributo, aninhamento, `<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`, `<meta charset>`, `<title>`, `<div>`, `<h1>`, `<p>`, `<ul>`, `<li>`, `<table>`, `<script>`, DOM, manipulação imperativa do DOM, React por CDN, Babel no navegador, JSX, componente, propriedade de componente, `map` na renderização de lista, chave de lista, função de seta |
| 16 | Ferramenta de build e consumo de API | limitação da compilação no navegador, Vite, `npm create vite`, servidor de desenvolvimento, substituição de módulo em tempo real (r), estrutura de pastas `api` e `web`, `fetch`, promessa, `async`, `await`, `try`/`catch`, `useState`, `useEffect`, array de dependências, estado de carregamento, estado de erro |
| 17 | Origem única e publicação contínua | política de mesma origem, proxy de desenvolvimento, caminho relativo, `npm run build`, `dist`, arquivos estáticos no Express, ordem de middlewares, rota de reserva, roteamento no cliente, variável de ambiente, `process.env`, Render, comando de build, comando de início, publicação a partir da `main`, hibernação de serviço gratuito, tag de release |

---

## `v0.2.0` — Persistência e consulta de dados

- **Projeto:** Gefina
- **Ambiente:** Editor local
- **Comandos de sistema:** sim
- **Abertura de ciclo:** aula 18
- **Encerramento de ciclo:** aula 23

| # | Tema | Conceitos |
|---|---|---|
| 18 | Modelagem relacional e banco de dados | banco de dados relacional, tabela, coluna, linha, tipo de dado, `INTEGER`, `TEXT`, `DATE`, `TIMESTAMP`, chave primária, chave estrangeira, integridade referencial, restrição `NOT NULL`, restrição `UNIQUE`, normalização (r), modelo lógico do Gefina, valor monetário em centavos, ponto flutuante e erro de arredondamento, PostgreSQL, Neon, projeto, branch de banco, ambiente de desenvolvimento e de produção, string de conexão |
| 19 | Definição e manipulação de dados em SQL | SQL, DDL e DML, `CREATE TABLE`, `SERIAL`, `PRIMARY KEY`, `REFERENCES`, `INSERT INTO`, `VALUES`, `RETURNING`, `SELECT`, projeção de colunas, `WHERE`, operadores de comparação em SQL, `AND`, `OR`, `ORDER BY`, `ASC`, `DESC`, `LIMIT`, `UPDATE`, `DELETE`, cláusula ausente e alteração em massa |
| 20 | Relacionamentos e agregação | `JOIN`, `INNER JOIN`, condição de junção, qualificação de coluna, alias de tabela, alias de coluna, `LEFT JOIN` (r), `COUNT`, `SUM`, `GROUP BY` (r), agregação sobre conjunto, script de seed, dados fictícios, idempotência do seed |
| 21 | Acesso ao banco pela aplicação | driver de banco, `pg`, `Pool`, conexão, consulta parametrizada, `$1`, injeção de SQL, resultado sem tipo, `rows`, mapeamento de coluna para propriedade, `snake_case` e `camelCase`, camada de serviço, separação entre rota e acesso a dados, nomeação por sufixo de camada, variável de ambiente, `.env`, `.env.example`, `--env-file`, segredo em repositório público, rotação de credencial |
| 22 | Listagem de faturas com dados reais | substituição dos dados em memória, listagem com `JOIN`, consulta por identificador, resposta de registro inexistente, tratamento de erro na rota, `try`/`catch` no servidor, `Intl.NumberFormat`, formatação de moeda em reais, conversão de centavos para reais, `Date`, `Intl.DateTimeFormat`, formato brasileiro de data, `for...of`, `push` |
| 23 | Painel inicial e primeira agregação | requisito de visão consolidada, consulta de agregação para totais, endpoint de resumo, componente de cartão, composição de componentes, propriedades tipadas, rota de navegação no cliente, `reduce` (r), publicação da release |

---

## `v0.3.0` — Operações de escrita e camada de acesso a dados

- **Projeto:** Gefina
- **Ambiente:** Editor local
- **Comandos de sistema:** sim
- **Abertura de ciclo:** aula 24
- **Encerramento de ciclo:** aula 30

| # | Tema | Conceitos |
|---|---|---|
| 24 | Fundamentos de CSS e estrutura de layout | folha de estilo, `<link>`, seletor de tipo, classe, `id` (r), especificidade, cascata, herança, unidades `px`, `rem`, `%`, cor hexadecimal, propriedade customizada, `var()`, `:root`, tokens de cor e espaçamento, tipografia, fonte web, `@font-face` (r), `font-family`, `font-size`, `font-weight`, numeral tabular, `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`, `<button>`, `<a>` |
| 25 | Modelo de caixa e disposição de elementos | box model, `content`, `padding`, `border`, `margin`, `box-sizing`, colapso de margem (r), `display`, `block`, `inline`, `inline-block`, `flex`, eixo principal e eixo transversal, `flex-direction`, `justify-content`, `align-items`, `gap`, `flex-grow`, `position`, `relative`, `absolute`, `fixed`, estilização de tabela, estilização de barra lateral, pseudo-classe, `:hover`, `:focus-visible` |
| 26 | Rotas de escrita e criação de registros | `POST`, corpo da requisição, `express.json` em uso, `INSERT ... RETURNING`, status `201`, `Location` (r), `PUT`, atualização parcial e total, `DELETE`, status `204`, idempotência (r), tratamento de erro por status, `400`, `404`, `500`, middleware de erro, REST Client, arquivo `.http` |
| 27 | Formulários e diálogo modal | `<form>`, `<label>`, `<input>`, tipos de campo, `<select>`, `<option>`, `<textarea>` (r), atributo `name`, atributo `id` e associação de rótulo, campo controlado, `value`, `onChange`, evento sintético, `preventDefault`, `onSubmit`, `<dialog>`, `showModal`, `close`, foco e retorno de foco, estado de conteúdo do modal, confirmação de exclusão no mesmo modal, atualização da listagem após envio |
| 28 | Validação de dados e verificação em tempo de execução | tipo em tempo de compilação e valor em tempo de execução, validação manual, verificação de presença, verificação de tipo, expressão regular, literal, classe de caracteres, quantificador, âncora, `test`, limite da validação artesanal, Zod, esquema, `parse`, `safeParse`, inferência de tipo a partir do esquema, mensagem de erro traduzida, validação no servidor e no cliente |
| 29 | Testes automatizados da API | propósito do teste automatizado, teste de comportamento e teste de implementação, Vitest, `describe`, `it`, `expect`, asserção, execução da suíte, Supertest, requisição sem porta, preparo e limpeza de estado, `beforeEach`, teste de criação, teste de atualização, teste de remoção, teste de dado inválido, teste como rede de segurança |
| 30 | Mapeamento objeto-relacional | limitação do acesso por driver, tipagem manual de resultado, ORM, Prisma, `schema.prisma`, modelo, mapeamento de nome de coluna, migração, histórico versionado de esquema, `migrate dev`, cliente gerado, tipagem derivada do esquema, `findMany`, `findUnique`, `create`, `update`, `delete`, relação declarada, `include`, `$queryRaw`, agregação preservada em SQL, suíte inalterada após substituição, publicação da release |

---

## `v0.4.0` — Segunda entidade e sistema de design

- **Projeto:** Gefina
- **Ambiente:** Editor local
- **Comandos de sistema:** sim
- **Abertura de ciclo:** aula 31
- **Encerramento de ciclo:** aula 36

| # | Tema | Conceitos |
|---|---|---|
| 31 | Testes de componente | camada de teste de interface, DOM simulado, jsdom, React Testing Library, `render`, consulta por papel, consulta por rótulo, `getByRole`, `getByLabelText`, `findBy` e assincronia, `userEvent`, simulação de digitação, simulação de clique, asserção sobre a tela, teste independente de classe CSS, simulação de requisição, teste de formulário, teste de validação, teste de confirmação de exclusão |
| 32 | Framework de estilização utilitária | limitação da folha de estilo em escala, nomeação de classe, escopo global do CSS, repetição de valor, Tailwind, classe utilitária, instalação e integração com Vite, `@import`, `@theme`, mapeamento de tokens para o tema, escala de espaçamento, escala tipográfica, correspondência entre propriedade CSS e utilitário, estado por prefixo, `hover:`, `focus-visible:`, composição de utilitários |
| 33 | Reescrita da interface e componentes reutilizáveis | remoção da folha de estilo, conversão das telas existentes, componente de botão, componente de campo, componente de tabela, componente de modal, propriedade de variação, `children`, composição, consistência visual, suíte inalterada após reescrita |
| 34 | Segunda entidade e generalização do padrão | modelagem de clientes, migração de esquema, rotas de leitura de clientes, rotas de escrita de clientes, rota de opções, ordem de registro de rotas, reaproveitamento da camada de serviço, reaproveitamento de componentes, rota de navegação, consolidação do padrão de CRUD |
| 35 | Imagem, texto e apresentação de identidade | `<img>`, atributos `src` e `alt`, acessibilidade de imagem, imagem opcional, campo de URL, tratamento de falha de carregamento, `onError`, métodos de string, `trim`, `split`, `slice`, `join`, `toUpperCase`, geração de iniciais, componente de avatar, cor fixa da paleta, `??` e valor ausente, `find` (r) |
| 36 | Restrições de integridade e refinamento | restrição `UNIQUE` em produção, violação de unicidade, código de erro do banco, status `409`, tradução de erro técnico em mensagem ao usuário, recusa de remoção de cliente com faturas, estado vazio sem critérios, estado vazio com critérios, estado de erro de carregamento, revisão de acessibilidade, `aria-label` (r), verificação dos critérios de aceite, publicação da release |

---

## `v0.5.0` — Autenticação e controle de acesso

- **Projeto:** Gefina
- **Ambiente:** Editor local
- **Comandos de sistema:** sim
- **Abertura de ciclo:** aula 37
- **Encerramento de ciclo:** aula 42

| # | Tema | Conceitos |
|---|---|---|
| 37 | Identidade no modelo de dados e migração em base populada | requisito de identidade, entidade de usuário, papel, cadeia de posse, coluna obrigatória em tabela com dados, estratégia de migração em três passos, coluna opcional, preenchimento retroativo, aplicação da obrigatoriedade, `ALTER TABLE`, migração de dados, ordem de aplicação entre ambientes, atualização do seed, relação usuário-cliente no esquema, restrição de remoção |
| 38 | Armazenamento seguro de credenciais | senha em texto puro e suas consequências, vazamento de base, função de derivação de chave, hash e irreversibilidade, `node:crypto`, `randomBytes`, sal, unicidade do resultado, `scrypt`, custo computacional, armazenamento de sal e resultado, comparação em tempo constante, `timingSafeEqual`, ataque por tempo, princípio de não implementar algoritmo criptográfico |
| 39 | Sessão e autenticação | autenticação, credencial, ausência de estado no HTTP, cookie, `Set-Cookie`, atributos `HttpOnly`, `Secure`, `SameSite`, `Max-Age`, identificador opaco, tabela de sessões, expiração, `cookie-parser`, criação de sessão, encerramento de sessão, remoção do registro, política de mesma origem, CORS, cabeçalho `Access-Control-Allow-Origin`, `credentials`, origem única e ausência de CORS |
| 40 | Proteção de rotas e estado de autenticação no cliente | middleware de autenticação, status `401`, usuário corrente na requisição, rota de identidade, tela de acesso, formulário de credenciais, mensagem única de credencial inválida, contexto do React, `createContext`, `useContext`, provedor, estado global de autenticação, rota protegida no cliente, redirecionamento, persistência entre recarregamentos, identificação do usuário autenticado |
| 41 | Autorização por papel e por posse | autenticação e autorização, papel administrativo, autorização por papel, status `403`, autorização por posse, verificação de propriedade do registro, restrição de emissão de fatura a cliente próprio, restrição de listagem aos registros próprios, interface como conveniência e servidor como garantia, ocultação condicional de elemento, verificação por teste automatizado |
| 42 | Administração de usuários | requisito de gestão de acesso, rotas restritas ao papel administrativo, cadastro de usuário, alteração de usuário, senha facultativa na alteração, remoção de usuário, bloqueio de remoção da própria conta, violação de integridade referencial na remoção, mensagem ao usuário, restrição de listagem por usuário, somente leitura em faturas e clientes para o papel administrativo, navegação condicional, publicação da release |

---

## `v1.0.0` — Consulta avançada e integração contínua

- **Projeto:** Gefina
- **Ambiente:** Editor local
- **Comandos de sistema:** sim
- **Abertura de ciclo:** aula 43
- **Encerramento de ciclo:** aula 46

| # | Tema | Conceitos |
|---|---|---|
| 43 | Recuperação parcial e busca | limitação da listagem completa, volume de dados e desempenho, índice de banco de dados (r), paginação, `LIMIT`, `OFFSET`, cálculo de deslocamento, total de registros, total de páginas, parâmetro de consulta, query string, `req.query`, estado de página no cliente, controle de navegação entre páginas, busca textual, `ILIKE`, curinga `%`, busca por nome e por endereço eletrônico, parâmetro opcional, composição de condições, preservação de critérios entre páginas |
| 44 | Ordenação dinâmica e segurança de consulta | ordenação por coluna variável, impossibilidade de parametrizar identificador, concatenação de identificador e injeção de SQL, lista de permissão, validação de critério e de direção, valor padrão, `ORDER BY` dinâmico, seletor de ordenação, composição com paginação e busca, restrição aos registros próprios, restrição por usuário na visão administrativa, filtro por situação da fatura |
| 45 | Painel consolidado e integração contínua | requisito de visão gerencial, agregação condicional, `SUM ... FILTER`, `COUNT ... FILTER`, consulta única para múltiplas métricas, situação derivada de fatura, vencimento e atraso, comparação com data corrente, cartões de indicador, listagem de faturas recentes, listagem de faturas em atraso, integração contínua, GitHub Actions, workflow, gatilho por evento, execução em pull request, verificação de tipo, verificação de formatação, execução da suíte, falha impedindo integração |
| 46 | Adaptação a múltiplos tamanhos de tela e entrega | limitação do layout fixo, `<meta name="viewport">`, consulta de mídia, abordagem mobile-first, prefixos responsivos do Tailwind, adaptação da barra lateral, adaptação de tabela em telas estreitas, revisão de contraste, verificação dos critérios de aceite de todas as releases, atualização do README, `v1.0.0`, tag de release, publicação final |

---

## Etapa Encerramento

- **Projeto:** Gefina
- **Ambiente:** Editor local
- **Comandos de sistema:** sim, apenas aula 47

| # | Tema | Conceitos |
|---|---|---|
| 47 | Nomes de domínio e configuração de DNS | endereço IP, resolução de nomes, DNS, servidor de nomes, hierarquia de domínios, domínio de topo, domínio de segundo nível, subdomínio, registrador, registro de domínio, registro `A`, registro `CNAME`, `TTL`, propagação, domínio personalizado na plataforma, verificação de titularidade, certificado digital, autoridade certificadora, HTTPS, emissão automática de certificado, renovação |
| 48 | Retrospectiva, portfólio e continuidade | arco das seis releases, decisão de arquitetura e sua motivação, dívida técnica, limite consciente de escopo, README como documentação de projeto, descrição de repositório, apresentação de projeto em processo seletivo, leitura do histórico de commits como narrativa, consolidação das notas de mercado, caminhos de estudo subsequentes |

---

## Momentos de construção manual precedente à ferramenta

Relação dos pontos em que o curso constrói manualmente antes de adotar a ferramenta correspondente.

| Aula | Construção manual | Ferramenta adotada |
|---|---|---|
| 13 e 14 | servidor com `node:http` e roteamento manual | Express |
| 15 | manipulação imperativa do DOM | React |
| 15 e 16 | React por CDN com Babel no navegador | Vite |
| 17 | rota de reserva e roteamento no cliente | React Router |
| 21 a 30 | SQL cru por driver | Prisma |
| 24, 25, 32 e 33 | CSS puro | Tailwind |
| 28 | validação manual com expressão regular | Zod |
| 38 | sal, derivação e comparação em tempo constante | bcrypt e argon2, nomeados |

---

## Notas de mercado previstas

Itens de emprego corrente na atividade profissional que o curso não adota, e a aula em que são apresentados.

| Aula | Item |
|---|---|
| 10 | Commitlint, Husky e Commitizen |
| 12 | Insomnia e Postman |
| 13 | `tsx` e `ts-node` |
| 16 | TanStack Query |
| 26 | Insomnia e Postman |
| 27 | React Hook Form |
| 29 | Jest, Playwright e Cypress |
| 30 | Docker |
| 32 | ESLint e Prettier |
| 39 | JWT |
| 38 | bcrypt e argon2 |
| 43 | índice de banco de dados |
| 45 | perfis de execução e plano de consulta |
