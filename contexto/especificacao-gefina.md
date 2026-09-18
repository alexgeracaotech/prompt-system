# Especificação do Produto — Gefina

Documento de referência do sistema. Consultado pelos arquivos de aula sempre que uma decisão de produto precisa ser observada.

Descreve o que o sistema é e como se comporta. Não descreve como é construído.

A consulta observa a vedação de antecipação: este documento descreve o sistema completo, e apenas o que a aula em curso comporta é empregado.

---

## 1. Visão e domínio

### 1.1 Propósito

O Gefina é um sistema de gestão de contas a receber. Registra os clientes de uma organização e as faturas emitidas contra esses clientes, e apresenta uma visão consolidada da situação financeira decorrente desses registros.

O sistema destina-se ao uso interno de uma equipe. Cada integrante mantém sua própria carteira de clientes e emite faturas apenas para os clientes que lhe pertencem, mas todos visualizam os registros de toda a organização. A visibilidade é coletiva; a responsabilidade é individual.

### 1.2 Domínio

Contas a receber designa o conjunto de valores que uma organização tem direito a receber de terceiros em decorrência de bens fornecidos ou serviços prestados. Cada valor a receber é documentado por uma fatura, emitida contra um cliente, com data de emissão e data de vencimento definidas.

Uma fatura permanece pendente desde a emissão até o recebimento do valor correspondente, quando passa à situação de paga. Faturas pendentes cuja data de vencimento já transcorreu encontram-se em atraso e demandam ação de cobrança.

O acompanhamento dessas informações responde a três perguntas recorrentes da operação: quanto já foi recebido, quanto há a receber, e o que está vencido.

### 1.3 Glossário

A tabela estabelece a correspondência entre o vocabulário apresentado ao usuário e o vocabulário empregado no código, conforme a política declarada na seção 8.

| Interface | Código | Definição |
|---|---|---|
| Usuário | `user` | Integrante da organização com acesso ao sistema |
| Administrador | `admin` | Papel com atribuições de gestão de acesso e visão somente leitura |
| Cliente | `customer` | Pessoa ou organização a quem se emitem faturas |
| Fatura | `invoice` | Documento que registra um valor a receber |
| Valor | `amount` | Quantia da fatura, armazenada em centavos |
| Situação | `status` | Condição da fatura quanto ao recebimento |
| Emissão | `issue date` | Data em que a fatura foi emitida |
| Vencimento | `due date` | Data limite para o recebimento |
| Paga | `paid` | Situação da fatura cujo valor foi recebido |
| Pendente | `pending` | Situação da fatura cujo valor não foi recebido |
| Atrasada | — | Rótulo de exibição da fatura pendente com vencimento transcorrido |
| Sessão | `session` | Registro que sustenta o acesso autenticado |
| Papel | `role` | Conjunto de atribuições associado a um usuário |

O rótulo Atrasada não possui correspondente no código por não constituir valor armazenado. Sua natureza é definida na seção 4.1.

### 1.4 Escopo

O sistema contempla:

- autenticação por endereço de correio eletrônico e senha, com sessão e proteção de rotas;
- cadastro, alteração e remoção de clientes;
- emissão, alteração e remoção de faturas;
- listagem paginada de clientes e de faturas, com busca textual e ordenação;
- restrição da visão às próprias carteiras, mediante acionamento explícito;
- painel inicial com indicadores consolidados, faturas recentes e faturas em atraso;
- administração de contas de acesso, restrita ao papel administrativo;
- adaptação da interface a telas de larguras distintas.

### 1.5 Exclusões de escopo

As funcionalidades a seguir não integram o sistema. A relação é declarada para que a ausência seja compreendida como decisão e não como omissão.

| Exclusão | Motivo |
|---|---|
| Cadastro público de usuários | O acesso é concedido pelo administrador |
| Recuperação de senha | Demanda serviço de envio de mensagens |
| Envio de arquivo de imagem | A imagem do cliente é informada por endereço |
| Registro da data de pagamento | A situação de recebimento é suficiente ao propósito |
| Distinção entre pagamento pontual e em atraso | Apenas o recebimento é relevante |
| Representação gráfica de séries temporais | Os indicadores numéricos atendem ao propósito |
| Moedas distintas do real | A operação é nacional |
| Tema claro | O sistema adota tema escuro único |
| Notificação de vencimento | Demanda serviço de envio de mensagens |
| Exportação de dados | Não decorre de necessidade da operação |
| Desativação de usuário em lugar da remoção | A remoção com restrição de integridade atende ao propósito |
| Registro de auditoria | Excede a necessidade de um sistema de equipe reduzida |

---

## 2. Modelo de dados

### 2.1 Entidades e relacionamentos

O sistema comporta quatro entidades. Três representam o domínio; a quarta sustenta o acesso autenticado.

```
users ──< sessions
  │
  └──< customers ──< invoices
```

| Relacionamento | Cardinalidade | Natureza |
|---|---|---|
| Usuário e sessão | Um para muitos | Um usuário mantém zero ou mais sessões ativas |
| Usuário e cliente | Um para muitos | Um usuário detém zero ou mais clientes |
| Cliente e fatura | Um para muitos | Um cliente possui zero ou mais faturas |

### 2.2 Cadeia de posse

A posse dos registros decorre da cadeia entre as entidades. Um cliente pertence ao usuário que o cadastrou. Uma fatura pertence ao cliente contra o qual foi emitida e, por consequência, ao usuário detentor desse cliente.

A fatura não registra proprietário próprio. A ausência dessa coluna é deliberada: como a emissão é restrita aos clientes do próprio usuário, o detentor da fatura é sempre o detentor do cliente, e armazenar essa informação constituiria duplicação de dado derivável.

A expressão registros próprios designa, para clientes, aqueles cuja coluna `user_id` corresponde ao usuário autenticado, e, para faturas, aquelas cujo cliente satisfaz essa condição.

### 2.3 Entidade `users`

| Coluna | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | `SERIAL` | Chave primária | Identificador |
| `name` | `TEXT` | `NOT NULL` | Nome do usuário, apresentado na interface |
| `email` | `TEXT` | `NOT NULL`, `UNIQUE` | Credencial de acesso |
| `password_hash` | `TEXT` | `NOT NULL` | Resultado da derivação da senha, com o sal correspondente |
| `role` | `TEXT` | `NOT NULL`, padrão `user` | Papel do usuário |
| `created_at` | `TIMESTAMP` | `NOT NULL`, padrão instante corrente | Momento do cadastro |

A coluna `role` admite os valores `user` e `admin`.

A coluna `password_hash` não armazena a senha. Seu conteúdo é o sal e o resultado da função de derivação, conforme a seção 4.6.

### 2.4 Entidade `sessions`

| Coluna | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | `TEXT` | Chave primária | Identificador opaco, gerado aleatoriamente |
| `user_id` | `INTEGER` | `NOT NULL`, referencia `users(id)` | Usuário titular da sessão |
| `expires_at` | `TIMESTAMP` | `NOT NULL` | Instante de expiração |
| `created_at` | `TIMESTAMP` | `NOT NULL`, padrão instante corrente | Momento da criação |

O identificador não deriva de dado do usuário nem carrega informação. Sua única função é localizar o registro correspondente.

A remoção de um usuário remove suas sessões em cascata. Sessão não constitui registro de valor histórico, e sua permanência após a remoção do titular não teria propósito.

### 2.5 Entidade `customers`

| Coluna | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | `SERIAL` | Chave primária | Identificador |
| `user_id` | `INTEGER` | `NOT NULL`, referencia `users(id)` | Usuário detentor |
| `name` | `TEXT` | `NOT NULL` | Nome do cliente |
| `email` | `TEXT` | `NOT NULL`, `UNIQUE` | Endereço de correio eletrônico |
| `image_url` | `TEXT` | — | Endereço da imagem de identificação |
| `created_at` | `TIMESTAMP` | `NOT NULL`, padrão instante corrente | Momento do cadastro |

A unicidade de `email` é global e não restrita ao usuário detentor. Um mesmo cliente não pode ser cadastrado por dois usuários distintos. A consequência é que o cadastro de cliente já registrado por outro integrante da equipe resulta em recusa, conforme a seção 4.4.

A coluna `image_url` admite ausência de valor. O comportamento correspondente é definido na seção 6.9.

### 2.6 Entidade `invoices`

| Coluna | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | `SERIAL` | Chave primária | Identificador |
| `customer_id` | `INTEGER` | `NOT NULL`, referencia `customers(id)` | Cliente contra o qual a fatura foi emitida |
| `amount` | `INTEGER` | `NOT NULL` | Valor em centavos |
| `status` | `TEXT` | `NOT NULL`, padrão `pending` | Situação quanto ao recebimento |
| `issue_date` | `DATE` | `NOT NULL` | Data de emissão |
| `due_date` | `DATE` | `NOT NULL` | Data de vencimento |
| `created_at` | `TIMESTAMP` | `NOT NULL`, padrão instante corrente | Momento do registro |

A coluna `status` admite os valores `pending` e `paid`.

A coluna `amount` armazena centavos como número inteiro. A representação de valores monetários em ponto flutuante produz erro de arredondamento, uma vez que frações decimais não possuem representação exata em base binária. A conversão para exibição é definida na seção 6.9.

A coluna `created_at` distingue-se de `issue_date`. A primeira registra o instante em que o dado ingressou no sistema; a segunda, a data que o documento declara. As duas podem divergir, e a ordenação por registro recente emprega `created_at`.

### 2.7 Comportamento na remoção

| Operação | Comportamento | Motivo |
|---|---|---|
| Remoção de usuário sem clientes | Permitida | Não há registro dependente |
| Remoção de usuário com clientes | Recusada | Preservação da integridade referencial |
| Remoção de cliente sem faturas | Permitida | Não há registro dependente |
| Remoção de cliente com faturas | Recusada | Preservação da integridade referencial |
| Remoção de fatura | Permitida | Não há registro dependente |
| Remoção de usuário e suas sessões | Cascata | Sessão não possui valor independente |

A recusa não constitui limitação técnica. Decorre de decisão: a remoção em cascata suprimiria silenciosamente registros financeiros, e a supressão de registro financeiro por efeito colateral de outra operação é inadmissível. A recusa obriga a decisão explícita sobre os registros dependentes.

### 2.8 Índices

Os índices do sistema decorrem das restrições de unicidade e das relações declaradas. As colunas `customers.user_id`, `invoices.customer_id` e `sessions.user_id` recebem índice por constituírem chave estrangeira. As colunas com restrição de unicidade recebem índice automaticamente.

O sistema não declara índice cuja criação decorra de decisão deliberada de otimização.

---

## 3. Papéis e permissões

### 3.1 Papéis

O sistema define dois papéis.

**Usuário** constitui o papel padrão. Destina-se aos integrantes da equipe que mantêm carteira de clientes e emitem faturas.

**Administrador** destina-se à gestão das contas de acesso e ao acompanhamento da operação. O papel não mantém carteira própria e não produz registros de domínio.

O papel é atribuído no cadastro e não é alterável pela interface. A promoção de um usuário a administrador não integra o escopo do sistema.

### 3.2 Matriz de permissões

| Operação | Usuário | Administrador |
|---|---|---|
| Visualizar painel inicial | Sim | Sim |
| Visualizar clientes | Todos | Todos |
| Visualizar faturas | Todas | Todas |
| Cadastrar cliente | Sim | Não |
| Alterar cliente | Próprios | Não |
| Remover cliente | Próprios | Não |
| Emitir fatura | Para clientes próprios | Não |
| Alterar fatura | De clientes próprios | Não |
| Remover fatura | De clientes próprios | Não |
| Restringir listagem aos registros próprios | Sim | Não se aplica |
| Restringir listagem por usuário | Não | Sim |
| Acessar administração de usuários | Não | Sim |
| Cadastrar usuário | Não | Sim |
| Alterar usuário | Não | Sim |
| Remover usuário | Não | Sim |
| Remover a própria conta | Não | Não |

### 3.3 Naturezas da autorização

As restrições do sistema decorrem de dois mecanismos distintos.

**Autorização por papel** condiciona a operação ao papel do usuário, independentemente do registro envolvido. A administração de usuários é acessível ao administrador e inacessível ao usuário comum, quaisquer que sejam os dados. A recusa por esse mecanismo independe do estado do banco.

**Autorização por posse** condiciona a operação à relação entre o usuário autenticado e o registro. A alteração de um cliente é permitida ao seu detentor e recusada aos demais. A recusa por esse mecanismo exige a consulta prévia do registro, uma vez que a posse não é conhecida antes de sua leitura.

A distinção possui consequência prática: a verificação por papel ocorre antes do acesso ao banco; a verificação por posse, necessariamente depois.

### 3.4 Restrições complementares

**Emissão de fatura.** A fatura é emitida contra cliente detido pelo usuário autenticado. A verificação incide sobre o cliente informado, não sobre a fatura. A recusa produz a resposta de operação não autorizada.

**Alteração e remoção de fatura.** A permissão decorre do detentor do cliente vinculado, conforme a cadeia de posse.

**Remoção da própria conta.** O administrador não remove a conta com que se encontra autenticado. A restrição previne a supressão do único acesso administrativo.

**Restrição de listagem.** O usuário aciona explicitamente a restrição aos registros próprios. Na ausência do acionamento, a listagem apresenta os registros de toda a organização. O administrador dispõe de mecanismo equivalente, com seleção do usuário.

### 3.5 Superfície de verificação

A restrição de permissão é verificada no servidor, em todas as operações restritas.

A interface reflete as permissões pela ocultação dos elementos correspondentes às operações indisponíveis. A ocultação constitui conveniência e não mecanismo de segurança: um cliente que não seja a interface pode submeter qualquer requisição, e a recusa depende exclusivamente da verificação no servidor.

O sistema mantém, portanto, verificação em duas camadas com propósitos distintos. A interface evita que o usuário acione operação que resultará em recusa. O servidor garante que a recusa ocorra.

### 3.6 Respostas de recusa

| Situação | Resposta |
|---|---|
| Requisição sem sessão válida | Não autenticado |
| Sessão válida, papel insuficiente | Não autorizado |
| Sessão válida, registro de outro detentor | Não autorizado |
| Registro inexistente | Não encontrado |

A distinção entre não autenticado e não autorizado é significativa: a primeira indica ausência de identificação e admite correção pelo acesso ao sistema; a segunda indica identificação insuficiente para a operação e não admite correção pelo usuário.

---

## 4. Regras de negócio

### 4.1 Situação da fatura

A fatura assume duas situações armazenadas: pendente e paga. A transição entre elas decorre da alteração do registro pelo usuário e não observa restrição de ordem — uma fatura paga pode retornar à situação pendente, hipótese que atende a correção de lançamento equivocado.

A condição de atraso não constitui situação armazenada. Uma fatura encontra-se em atraso quando satisfaz simultaneamente duas condições:

- a situação registrada é pendente;
- a data de vencimento é anterior à data corrente.

A condição é avaliada no momento da consulta. Uma fatura ingressa em atraso pela passagem do tempo, sem que operação alguma incida sobre ela.

A exibição observa três rótulos mutuamente exclusivos, determinados na ordem: fatura com situação paga exibe Paga; fatura pendente com vencimento transcorrido exibe Atrasada; fatura pendente com vencimento futuro ou correspondente à data corrente exibe Pendente.

A fatura paga após o vencimento exibe Paga. O sistema não distingue recebimento pontual de recebimento em atraso, conforme a exclusão de escopo declarada na seção 1.5.

### 4.2 Valor da fatura

O valor é armazenado em centavos, como número inteiro, e deve ser superior a zero. Fatura de valor nulo ou negativo não é admitida.

A conversão observa duas direções. Na entrada, o valor informado em reais é convertido em centavos por multiplicação por cem e arredondamento. Na exibição, o valor armazenado é convertido em reais por divisão por cem e formatado conforme a seção 6.9.

### 4.3 Datas da fatura

A data de emissão e a data de vencimento são obrigatórias.

A data de vencimento não é anterior à data de emissão. A coincidência entre ambas é admitida e corresponde a fatura com vencimento imediato.

Nenhuma das datas observa restrição em relação à data corrente. A emissão retroativa é admitida, uma vez que o sistema registra documentos que podem preceder seu lançamento.

### 4.4 Cliente

O nome é obrigatório e não admite conteúdo composto exclusivamente por espaços.

O endereço de correio eletrônico é obrigatório, observa formato válido e é único no sistema. A tentativa de cadastro com endereço já registrado é recusada, ainda que o registro existente pertença a outro usuário. A mensagem apresentada informa a duplicidade sem revelar o detentor do registro existente.

O endereço da imagem é facultativo. Quando informado, observa formato de endereço válido. O comportamento na ausência de valor ou na falha de carregamento é definido na seção 6.9.

O cliente é vinculado ao usuário autenticado no momento do cadastro. O vínculo não é alterável.

### 4.5 Usuário

O nome é obrigatório e não admite conteúdo composto exclusivamente por espaços.

O endereço de correio eletrônico é obrigatório, observa formato válido e é único no sistema.

A senha é obrigatória no cadastro e observa comprimento mínimo de oito caracteres. Na alteração do usuário, o campo é facultativo: a ausência de valor preserva a senha vigente.

O papel é atribuído no cadastro. O sistema não altera o papel de usuário existente.

### 4.6 Armazenamento de credenciais

A senha não é armazenada. O sistema armazena o resultado de função de derivação de chave aplicada à senha, acompanhado do sal empregado.

O sal é gerado aleatoriamente para cada senha, com dezesseis bytes, e armazenado junto ao resultado da derivação. A geração individual assegura que senhas idênticas produzam resultados distintos, impedindo a identificação de coincidências pela leitura da base.

A verificação aplica a mesma derivação à senha informada, utilizando o sal armazenado, e compara os resultados por rotina de tempo constante. A comparação convencional interrompe-se na primeira divergência, e a variação do tempo de resposta revelaria a extensão da coincidência.

### 4.7 Sessão

A autenticação bem-sucedida cria um registro de sessão com identificador aleatório de trinta e dois bytes e expiração de sete dias.

O identificador é transmitido ao cliente por cookie inacessível a script, restrito a conexão segura e limitado a requisições de mesma origem.

Cada requisição a rota protegida localiza a sessão pelo identificador recebido. A ausência do registro, ou sua expiração, resulta em recusa por ausência de autenticação.

O encerramento remove o registro de sessão e o cookie correspondente. A remoção do registro invalida a sessão de modo definitivo, independentemente da posse do identificador.

### 4.8 Recuperação de registros

**Paginação.** As listagens de clientes e de faturas apresentam dez registros por página. A resposta acompanha o total de registros que satisfazem os critérios vigentes, de modo a permitir o cálculo do total de páginas.

**Busca.** A busca incide sobre o nome e o endereço de correio eletrônico do cliente, em ambas as listagens. A comparação desconsidera diferença entre maiúsculas e minúsculas e localiza a ocorrência do termo em qualquer posição do texto.

**Ordenação.** As listagens admitem ordenação por critérios definidos, em ordem crescente ou decrescente. O critério recebido é confrontado com relação fechada de valores admitidos; valor não constante da relação resulta na aplicação do critério padrão.

| Listagem | Critérios | Padrão |
|---|---|---|
| Clientes | Nome, total pendente, total pago | Nome crescente |
| Faturas | Cliente, valor, vencimento | Vencimento decrescente |

**Composição.** Restrição de posse, busca, ordenação e paginação compõem-se entre si. A alteração de qualquer critério retorna a listagem à primeira página; a navegação entre páginas preserva os demais critérios.

### 4.9 Indicadores do painel

O painel apresenta quatro indicadores, calculados sobre a totalidade dos registros da organização:

| Indicador | Cálculo |
|---|---|
| Total recebido | Soma dos valores das faturas com situação paga |
| Total pendente | Soma dos valores das faturas com situação pendente |
| Faturas | Quantidade de faturas registradas |
| Clientes | Quantidade de clientes registrados |

O painel apresenta ainda duas listagens: as cinco faturas registradas mais recentemente, ordenadas pelo momento de registro em ordem decrescente; e as cinco faturas em atraso com vencimento mais antigo, ordenadas pelo vencimento em ordem crescente.

O total pendente compreende as faturas em atraso, uma vez que o atraso não constitui situação distinta da pendência.

---

## 5. Contrato da API

### 5.1 Convenções gerais

A API é servida sob o prefixo `/api`. O cliente a referencia por caminho relativo, uma vez que ambos compartilham a mesma origem.

Requisições e respostas empregam JSON. Os nomes de campo observam a grafia `camelCase`, distinta da grafia `snake_case` adotada nas colunas do banco; a conversão ocorre na camada de acesso a dados.

Datas são representadas no formato `AAAA-MM-DD` quando correspondem a data, e no formato ISO 8601 completo quando correspondem a instante. A formatação para exibição ocorre no cliente.

Valores monetários são transmitidos em centavos, como número inteiro.

Salvo indicação em contrário, todas as rotas exigem sessão válida.

### 5.2 Formato das respostas

Respostas de listagem observam envelope com os dados e a informação de paginação:

```json
{
  "data": [],
  "page": 1,
  "pageSize": 10,
  "total": 137
}
```

Respostas de registro individual retornam o objeto diretamente, sem envelope.

### 5.3 Códigos de status

| Código | Emprego |
|---|---|
| `200` | Consulta ou alteração bem-sucedida |
| `201` | Criação bem-sucedida; o corpo contém o registro criado |
| `204` | Remoção bem-sucedida; sem corpo |
| `400` | Dados inválidos |
| `401` | Ausência de sessão válida |
| `403` | Sessão válida com permissão insuficiente |
| `404` | Registro inexistente |
| `409` | Conflito com o estado atual dos dados |
| `500` | Falha não prevista |

O código `409` aplica-se à violação de unicidade e à recusa de remoção por existência de registros dependentes.

### 5.4 Formato de erro

```json
{
  "error": {
    "message": "Não foi possível cadastrar o cliente.",
    "fields": {
      "email": "Este endereço já está cadastrado."
    }
  }
}
```

A mensagem destina-se à apresentação ao usuário e é redigida em português. O objeto de campos é presente apenas quando a falha decorre de validação e permite associar cada mensagem ao campo correspondente do formulário.

A resposta de erro não expõe mensagem originada do banco de dados nem rastreamento de pilha.

### 5.5 Sessão

A sessão é tratada como recurso. A autenticação consiste na criação de uma sessão; o encerramento, em sua remoção.

| Método | Caminho | Sessão | Descrição |
|---|---|---|---|
| `POST` | `/api/sessions` | Dispensada | Autentica e cria sessão |
| `GET` | `/api/sessions/current` | Exigida | Retorna o usuário autenticado |
| `DELETE` | `/api/sessions/current` | Exigida | Encerra a sessão |

Corpo da requisição de criação:

```json
{ "email": "usuario@gefina.com.br", "password": "..." }
```

Corpo da resposta:

```json
{ "id": 2, "name": "Ana Bessa", "email": "usuario@gefina.com.br", "role": "user" }
```

A criação bem-sucedida define o cookie de sessão. A credencial inválida produz `401` com mensagem única, que não distingue endereço inexistente de senha incorreta.

### 5.6 Clientes

| Método | Caminho | Permissão |
|---|---|---|
| `GET` | `/api/customers` | Qualquer papel |
| `GET` | `/api/customers/options` | Papel usuário |
| `GET` | `/api/customers/:id` | Qualquer papel |
| `POST` | `/api/customers` | Papel usuário |
| `PUT` | `/api/customers/:id` | Detentor |
| `DELETE` | `/api/customers/:id` | Detentor |

A rota de opções retorna os clientes do usuário autenticado, com identificador e nome apenas, destinada ao preenchimento do campo de seleção do formulário de fatura. Não observa paginação.

Sua declaração precede a declaração da rota com parâmetro, uma vez que a correspondência de rotas no Express observa a ordem de registro e o segmento `options` seria capturado como identificador.

Representação do cliente:

```json
{
  "id": 3,
  "name": "Construtora Meridiano",
  "email": "contato@meridiano.com.br",
  "imageUrl": null,
  "createdAt": "2026-03-14T13:22:05.000Z",
  "owner": { "id": 2, "name": "Ana Bessa" },
  "totals": { "pending": 482000, "paid": 1250000 }
}
```

Os totais correspondem à soma dos valores das faturas do cliente em cada situação e sustentam os critérios de ordenação correspondentes.

Corpo de criação e alteração:

```json
{ "name": "...", "email": "...", "imageUrl": null }
```

### 5.7 Faturas

| Método | Caminho | Permissão |
|---|---|---|
| `GET` | `/api/invoices` | Qualquer papel |
| `GET` | `/api/invoices/:id` | Qualquer papel |
| `POST` | `/api/invoices` | Papel usuário, cliente próprio |
| `PUT` | `/api/invoices/:id` | Detentor do cliente |
| `DELETE` | `/api/invoices/:id` | Detentor do cliente |

Representação da fatura:

```json
{
  "id": 128,
  "amount": 125000,
  "status": "pending",
  "situation": "overdue",
  "issueDate": "2026-06-01",
  "dueDate": "2026-06-15",
  "createdAt": "2026-06-01T09:14:00.000Z",
  "customer": {
    "id": 3,
    "name": "Construtora Meridiano",
    "email": "contato@meridiano.com.br",
    "imageUrl": null
  },
  "owner": { "id": 2, "name": "Ana Bessa" }
}
```

A representação distingue dois campos de natureza diversa. O campo `status` corresponde ao valor armazenado e admite `pending` e `paid`; é o campo manipulado pelo formulário de alteração. O campo `situation` corresponde ao valor derivado e admite `pending`, `paid` e `overdue`; é o campo empregado na exibição do rótulo. O segundo é calculado a cada consulta e não possui coluna correspondente.

Corpo de criação e alteração:

```json
{
  "customerId": 3,
  "amount": 125000,
  "status": "pending",
  "issueDate": "2026-06-01",
  "dueDate": "2026-06-15"
}
```

### 5.8 Usuários

Todas as rotas exigem o papel administrativo.

| Método | Caminho |
|---|---|
| `GET` | `/api/users` |
| `GET` | `/api/users/:id` |
| `POST` | `/api/users` |
| `PUT` | `/api/users/:id` |
| `DELETE` | `/api/users/:id` |

Representação do usuário:

```json
{
  "id": 2,
  "name": "Ana Bessa",
  "email": "ana@gefina.com.br",
  "role": "user",
  "createdAt": "2026-01-10T11:00:00.000Z",
  "customerCount": 8
}
```

A representação não inclui o resultado da derivação da senha, em nenhuma resposta da API.

A quantidade de clientes informa o administrador sobre a possibilidade de remoção do usuário.

Corpo de criação:

```json
{ "name": "...", "email": "...", "password": "...", "role": "user" }
```

Na alteração, o campo de senha é facultativo; sua ausência preserva a credencial vigente.

### 5.9 Painel

| Método | Caminho | Permissão |
|---|---|---|
| `GET` | `/api/dashboard` | Qualquer papel |

Corpo da resposta:

```json
{
  "totals": {
    "paid": 8420000,
    "pending": 3150000,
    "invoiceCount": 180,
    "customerCount": 24
  },
  "recentInvoices": [],
  "overdueInvoices": []
}
```

As listagens contêm cinco faturas cada, na representação da seção 5.7. A primeira apresenta as faturas de registro mais recente; a segunda, as faturas em atraso de vencimento mais antigo.

A rota consolida em uma requisição os dados de uma tela. Trata-se de exceção deliberada à organização por recurso, justificada pela natureza da apresentação.

### 5.10 Parâmetros de consulta

| Parâmetro | Listagens | Valores | Padrão |
|---|---|---|---|
| `page` | Clientes, faturas, usuários | Inteiro positivo | `1` |
| `search` | Clientes, faturas | Texto | Ausente |
| `scope` | Clientes, faturas | `all`, `mine` | `all` |
| `userId` | Clientes, faturas | Identificador de usuário | Ausente |
| `status` | Faturas | `pending`, `paid`, `overdue` | Ausente |
| `sort` | Clientes | `name`, `pending`, `paid` | `name` |
| `sort` | Faturas | `customer`, `amount`, `dueDate` | `dueDate` |
| `order` | Clientes, faturas | `asc`, `desc` | Conforme a seção 4.8 |

O parâmetro `scope` restringe a listagem aos registros próprios e corresponde ao acionamento disponível ao papel usuário. O parâmetro `userId` restringe a listagem aos registros de usuário determinado e é admitido exclusivamente ao papel administrativo; sua presença em requisição de usuário comum resulta em `403`.

Os parâmetros `sort` e `order` são confrontados com a relação de valores admitidos. Valor não constante da relação resulta na aplicação do padrão, sem produção de erro.

A busca incide sobre o nome e o endereço de correio eletrônico do cliente. Na listagem de faturas, a incidência recai sobre o cliente vinculado.

---

## 6. Interface

### 6.1 Estrutura geral

A aplicação apresenta duas disposições distintas.

A **disposição de acesso** compreende exclusivamente a tela de autenticação, centralizada, sem navegação.

A **disposição principal** compreende as demais telas e organiza-se em barra lateral fixa à esquerda e área de conteúdo à direita. A barra lateral apresenta a identificação do sistema, a navegação, a identificação do usuário autenticado e o encerramento de sessão.

### 6.2 Navegação

| Item | Rota | Visibilidade |
|---|---|---|
| Painel | `/` | Qualquer papel |
| Faturas | `/faturas` | Qualquer papel |
| Clientes | `/clientes` | Qualquer papel |
| Usuários | `/usuarios` | Papel administrativo |

As rotas do cliente são redigidas em português, conforme a seção 8. O item correspondente à rota vigente recebe destaque visual.

A rota de administração de usuários é inacessível ao papel usuário. O acesso direto pelo endereço redireciona ao painel.

### 6.3 Tela de autenticação

Apresenta o formulário de acesso, com campos de endereço de correio eletrônico e senha, e o acionamento de entrada.

A credencial inválida produz mensagem única, apresentada acima do formulário, que não distingue endereço inexistente de senha incorreta. A distinção informaria a existência de conta a quem não a possui.

Durante o processamento, o acionamento permanece desabilitado.

### 6.4 Painel

Apresenta quatro indicadores dispostos em fileira, seguidos de duas listagens dispostas lado a lado: faturas recentes e faturas em atraso.

Cada indicador apresenta rótulo e valor. Os indicadores monetários observam a formatação da seção 6.9.

Cada listagem apresenta cinco faturas, com identificação do cliente, valor e data pertinente — vencimento, na listagem de atraso; emissão, na de recentes. A listagem sem registros apresenta mensagem correspondente.

### 6.5 Listagem de faturas

Apresenta, na ordem: título, acionamento de emissão, controles de consulta, tabela e navegação entre páginas.

Os controles de consulta compreendem campo de busca, seletor de situação, seletor de ordenação e acionamento de restrição. O acionamento de restrição alterna entre a visão integral e a visão dos registros próprios, e é apresentado ao papel usuário; ao papel administrativo, é substituído por seletor de usuário.

A tabela apresenta as colunas: cliente, com imagem e nome; valor; vencimento; situação; e responsável. A coluna de situação apresenta o rótulo correspondente, com distinção cromática conforme a seção 6.9.

Cada linha admite acionamento de edição, apresentado apenas nas faturas de clientes próprios.

O acionamento de emissão é apresentado apenas ao papel usuário.

### 6.6 Listagem de clientes

Observa a mesma estrutura da listagem de faturas.

A tabela apresenta as colunas: cliente, com imagem e nome; endereço de correio eletrônico; total pendente; total pago; e responsável.

### 6.7 Listagem de usuários

Acessível ao papel administrativo. Apresenta título, acionamento de cadastro, campo de busca, tabela e navegação entre páginas.

A tabela apresenta as colunas: nome; endereço de correio eletrônico; papel; e quantidade de clientes.

O acionamento de remoção é ausente na linha correspondente ao próprio administrador.

### 6.8 Diálogo modal

O cadastro, a alteração e a remoção ocorrem em diálogo modal sobreposto à listagem. A aplicação não possui telas dedicadas a formulário.

O diálogo observa três estados de conteúdo, mutuamente exclusivos: **formulário**, apresentando os campos e os acionamentos de cancelamento e confirmação; **confirmação de remoção**, apresentando a advertência de irreversibilidade e os acionamentos de cancelamento e remoção; e **processamento**, com os acionamentos desabilitados.

A confirmação de remoção substitui o conteúdo do diálogo vigente. O sistema não sobrepõe diálogos.

O acionamento de remoção é apresentado no diálogo de alteração, e é ausente no diálogo de cadastro.

A abertura desloca o foco ao primeiro campo. O fechamento restitui o foco ao elemento que originou a abertura. O diálogo é fechado pela tecla de escape e pelo acionamento de cancelamento; não é fechado pelo acionamento sobre a área externa, de modo a prevenir a perda involuntária de preenchimento.

A operação bem-sucedida fecha o diálogo e atualiza a listagem subjacente.

### 6.9 Apresentação de dados

**Valores monetários.** Apresentados em reais, com separador de milhar, duas casas decimais e símbolo da moeda. O valor armazenado em centavos é dividido por cem antes da formatação.

**Datas.** Apresentadas no formato de dia, mês e ano, separados por barra.

**Situação da fatura.** Apresentada como etiqueta, com cor correspondente: paga em verde, pendente em âmbar, atrasada em vermelho. A cor acompanha o rótulo textual e não constitui o único portador da informação.

**Imagem do cliente.** Apresentada em formato circular. Na ausência de endereço, ou na falha de carregamento, é substituída pelas iniciais do nome sobre fundo da cor primária. As iniciais correspondem às primeiras letras das duas primeiras palavras do nome, em maiúsculas.

### 6.10 Estados de listagem

| Estado | Apresentação |
|---|---|
| Carregamento | Indicação de carregamento em substituição ao conteúdo |
| Vazio sem critérios | Mensagem indicando ausência de registros e convite ao cadastro |
| Vazio com critérios | Mensagem indicando ausência de correspondência e acionamento de limpeza |
| Falha | Mensagem de falha e acionamento de nova tentativa |

A distinção entre os dois estados vazios é significativa: o primeiro indica sistema sem dados; o segundo, critérios sem correspondência. A orientação ao usuário difere em cada caso.

### 6.11 Adaptação a telas estreitas

A aplicação observa um ponto de quebra.

Em telas de largura reduzida, a barra lateral converte-se em navegação superior; os indicadores do painel dispõem-se em coluna; as listagens do painel dispõem-se em sequência; a tabela admite deslocamento horizontal; e o diálogo modal ocupa a largura disponível.

### 6.12 Sistema de design

**Cores**

| Função | Valor |
|---|---|
| Fundo | `#0F1117` |
| Superfície | `#171A21` |
| Superfície elevada | `#1F232C` |
| Borda | `#2A2F3A` |
| Texto | `#E8EAED` |
| Texto secundário | `#9AA1AE` |
| Primária | `#6E56CF` |
| Paga | `#3FB950` |
| Pendente | `#D29922` |
| Atrasada | `#F85149` |

**Tipografia.** Família Inter, nos pesos regular, médio e semibold. Colunas de valor monetário observam numerais tabulares.

**Espaçamento.** Escala em múltiplos de quatro pixels: 4, 8, 12, 16, 24, 32, 48.

**Raio de borda.** Seis pixels em campos e acionamentos; dez pixels em cartões e tabelas; quatorze pixels em diálogos.

---

## 7. Dados iniciais

### 7.1 Propósito

O seed popula o banco com dados fictícios que sustentam a operação do sistema durante o desenvolvimento e a demonstração. Sua composição não é arbitrária: o volume e a distribuição dos registros determinam se paginação, busca, ordenação, filtros e indicadores produzem resultado observável.

O seed é idempotente. Sua execução remove os registros existentes e os recria, restituindo o banco ao estado inicial. A propriedade permite restaurar dados corrompidos durante experimentação, tanto em desenvolvimento quanto na instância publicada.

### 7.2 Usuários

Cinco registros: um administrador e quatro usuários comuns.

| Nome | Papel |
|---|---|
| Usuário Administrador | `admin` |
| Ana Bessa | `user` |
| Carlos Daniel | `user` |
| Eder Fagundes | `user` |
| Gustavo Hebe | `user` |

Os endereços observam o domínio `gefina.com.br`. As senhas são idênticas entre si e documentadas no arquivo de descrição do repositório.

As iniciais dos quatro usuários comuns observam sequência alfabética, o que torna a identificação imediata durante a demonstração e produz iniciais distintas entre si na apresentação do avatar.

A existência de quatro usuários comuns é necessária ao propósito do seed. Com um único usuário, a restrição aos registros próprios apresentaria resultado idêntico à visão integral, e o seletor de usuário do papel administrativo não teria alternativas a oferecer.

### 7.3 Clientes

Vinte e quatro registros, distribuídos de forma desigual entre os quatro usuários comuns:

| Usuário | Clientes |
|---|---|
| Ana Bessa | 8 |
| Carlos Daniel | 7 |
| Eder Fagundes | 6 |
| Gustavo Hebe | 3 |

A distribuição desigual é deliberada. Quantidades idênticas impediriam observar o efeito da restrição por usuário, uma vez que todos os resultados apresentariam o mesmo volume.

Os nomes correspondem a organizações fictícias de ramos variados, com extensão suficiente para exercitar a geração de iniciais a partir de duas palavras. A relação contempla nomes iniciados por letras distribuídas ao longo do alfabeto, de modo que a ordenação alfabética produza reordenação perceptível.

Oito clientes possuem endereço de imagem; dezesseis não possuem. A presença de ambos os casos assegura que a imagem e as iniciais sejam observadas sem intervenção.

Nenhum cliente é atribuído ao administrador, conforme a matriz de permissões.

### 7.4 Faturas

Cento e oitenta registros, distribuídos entre os clientes em quantidades variáveis, de zero a quinze por cliente.

A existência de clientes sem faturas é necessária: permite observar o estado vazio da listagem filtrada e a remoção de cliente sem registros dependentes.

**Distribuição por situação**

| Situação | Proporção | Quantidade aproximada |
|---|---|---|
| Paga | 45% | 81 |
| Pendente com vencimento futuro | 35% | 63 |
| Pendente com vencimento transcorrido | 20% | 36 |

A proporção de faturas em atraso assegura que a listagem correspondente do painel apresente cinco registros e que o filtro por situação produza resultado significativo.

**Distribuição temporal**

As datas de emissão distribuem-se ao longo dos doze meses anteriores à execução do seed. O vencimento situa-se entre quinze e sessenta dias após a emissão.

As datas são calculadas em relação à data corrente, e não fixadas em literal. Datas fixas tornariam todas as faturas atrasadas com o transcorrer do tempo, e a proporção declarada na tabela anterior se perderia.

**Distribuição de valores**

Os valores situam-se entre cinquenta e oitenta mil reais, com concentração nas faixas intermediárias. A amplitude assegura que a ordenação por valor produza reordenação perceptível e que a formatação monetária seja exercitada com separador de milhar.

**Momento de registro**

O momento de registro difere da data de emissão, e a diferença é variável. A distinção sustenta a listagem de faturas recentes do painel, que observa o registro e não a emissão.

### 7.5 Volume resultante

| Listagem | Registros | Páginas |
|---|---|---|
| Faturas, visão integral | 180 | 18 |
| Faturas, restrição ao maior usuário | aproximadamente 70 | 7 |
| Clientes, visão integral | 24 | 3 |
| Usuários | 5 | 1 |

O volume é suficiente para que a paginação seja necessária em três das quatro listagens, e para que a navegação entre páginas seja exercitada sem tornar a demonstração laboriosa.

### 7.6 Execução

O seed é executado por script declarado no arquivo de manifesto do projeto, e opera sobre o banco indicado pela variável de ambiente vigente.

A execução sobre o ambiente de produção é admitida e constitui o procedimento de restituição da instância publicada ao estado inicial.

---

## 8. Política bilíngue

### 8.1 Critério

O idioma de cada elemento do sistema decorre do destinatário do texto.

Elementos destinados a pessoas são redigidos em **português do Brasil**. Elementos destinados a programas são redigidos em **inglês**.

O critério não é a visibilidade. O caminho de uma rota da API é visível no painel de rede do navegador, e nem por isso destina-se a uma pessoa: sua finalidade é estabelecer o contrato entre o cliente e o servidor. Inversamente, o caminho de uma rota do cliente figura na barra de endereços e é lido, comunicado e memorizado por pessoas.

### 8.2 Aplicação

| Elemento | Destinatário | Idioma | Exemplo |
|---|---|---|---|
| Texto de interface | Pessoa | Português | Faturas, Nova fatura, Vencimento |
| Rota do cliente | Pessoa | Português | `/faturas`, `/clientes`, `/usuarios` |
| Mensagem de erro ao usuário | Pessoa | Português | Este endereço já está cadastrado. |
| Documento de descrição do repositório | Pessoa | Português | — |
| Descrição de tarefa e critério de aceite | Pessoa | Português | — |
| Comentário no código | Pessoa | Português | — |
| Caminho da API | Programa | Inglês | `/api/invoices` |
| Campo de documento JSON | Programa | Inglês | `customerId`, `dueDate` |
| Tabela e coluna do banco | Programa | Inglês | `invoices`, `customer_id` |
| Identificador no código | Programa | Inglês | `invoiceService`, `findOverdue` |
| Nome de arquivo e de diretório | Programa | Inglês | `invoice.service.ts`, `services` |
| Mensagem de commit | Programa | Inglês | `feat(invoices): add creation endpoint` |
| Registro técnico e mensagem de exceção | Programa | Inglês | — |

### 8.3 Formatação de valores

A apresentação de valores observa as convenções brasileiras: moeda em reais, com vírgula decimal e ponto de milhar; datas em dia, mês e ano.

A representação transmitida pela API não observa essas convenções: valores monetários são transmitidos como número inteiro de centavos, e datas no formato internacional. A conversão ocorre na apresentação.

### 8.4 Nomes próprios

Nomes próprios não são traduzidos nem adaptados. Gefina e Simba conservam a grafia em qualquer contexto.

Denominações técnicas consagradas conservam a forma original quando a tradução não possui uso corrente. Permanecem em inglês, entre outros: commit, branch, merge, pull request, deploy, build, endpoint, hash, token, cookie e seed.

### 8.5 Grafia de identificadores

| Contexto | Convenção | Exemplo |
|---|---|---|
| Variável, função e propriedade | `camelCase` | `dueDate` |
| Tipo, interface e componente | `PascalCase` | `InvoiceForm` |
| Constante de configuração | `SCREAMING_SNAKE_CASE` | `SESSION_MAX_AGE` |
| Tabela e coluna do banco | `snake_case` | `customer_id` |
| Arquivo de camada | `recurso.camada.ts` | `invoice.service.ts` |
| Arquivo de componente | `PascalCase.tsx` | `InvoiceForm.tsx` |
| Demais arquivos | `camelCase` | `formatCurrency.ts` |

A divergência entre a grafia do banco e a do código é deliberada e observa a convenção de cada domínio. A conversão ocorre na camada de acesso a dados.

A nomeação por sufixo de camada aplica-se aos arquivos que pertencem a uma camada arquitetural, o que compreende a API. As camadas são `route`, `service`, `schema` e `middleware`, sempre no singular, com o recurso também no singular.

Não se aplica aos componentes do cliente, cujo nome corresponde ao componente exportado.

### 8.6 Correspondência terminológica

Cada conceito do domínio possui uma denominação em português e uma em inglês, estabelecidas no glossário da seção 1.3. As denominações não variam: o conceito designado por Fatura na interface é designado por `invoice` no código, em toda ocorrência.

A consistência terminológica constitui requisito e não preferência. A alternância entre denominações equivalentes impede a formação do vocabulário técnico e dificulta a correspondência entre o que o usuário observa e o que o código expressa.
