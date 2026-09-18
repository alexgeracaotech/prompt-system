# Curso de Desenvolvimento Web — Sistema de Prompts

Sistema de prompts que conduz um curso completo de desenvolvimento web, de conhecimento nulo em programação até uma aplicação full stack publicada em produção.

O curso compreende quarenta e oito aulas e dois projetos. A condução é realizada por um agente de inteligência artificial, orientado pelos documentos deste repositório.

---

## Sumário

- [Visão geral](#visão-geral)
- [Método](#método)
- [Tecnologias](#tecnologias)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Requisitos](#requisitos)
- [Preparação](#preparação)
- [Início de uma aula](#início-de-uma-aula)
- [Transcurso da aula](#transcurso-da-aula)
- [Prosseguimento entre aulas](#prosseguimento-entre-aulas)
- [Situações imprevistas](#situações-imprevistas)
- [O que o sistema não faz](#o-que-o-sistema-não-faz)
- [Estrutura do curso](#estrutura-do-curso)

---

## Visão geral

O sistema conduz o aprendizado por meio da construção de dois projetos.

**Simba** é um simulador bancário operado por diálogos do navegador. Ocupa as dez primeiras aulas e destina-se à formação do raciocínio de programação e ao domínio dos fundamentos da linguagem.

**Gefina** é um sistema de gestão de contas a receber, composto de interface, API e banco de dados. Ocupa as aulas onze a quarenta e seis e é construído em seis entregas sucessivas, cada uma concluída com uma versão publicada e funcional.

As duas últimas aulas tratam da configuração de domínio próprio e do encerramento do curso.

Ao final, quem percorre o curso dispõe de dois projetos publicados, um repositório com histórico completo de desenvolvimento e compreensão do processo que produz software.

---

## Método

O sistema observa quatro princípios, declarados na constituição e aplicados em todas as aulas.

**Ensino no momento da necessidade.** O conceito é apresentado quando o projeto dele necessita. Não há etapa dedicada a uma linguagem isolada seguida de aplicação posterior.

**Construção manual precedente à ferramenta.** Antes de adotar uma biblioteca ou framework, o curso constrói manualmente a solução que ela substitui, de modo que a limitação superada seja observável. A ferramenta é então apresentada como resposta a essa limitação.

**Revelação progressiva.** Cada aula trata exclusivamente do que lhe corresponde. O agente não antecipa conteúdo futuro nem pressupõe conhecimento não estabelecido.

**Decisão justificada.** Toda escolha técnica é apresentada com o problema que a originou, as alternativas consideradas e a razão da escolha. As ferramentas de mercado que o curso não adota são expressamente tratadas, com o motivo da não adoção e a circunstância em que são preferíveis.

---

## Tecnologias

| Camada | Tecnologia |
|---|---|
| Linguagem | TypeScript |
| Interface | React, Vite, Tailwind |
| Servidor | Node.js, Express |
| Banco de dados | PostgreSQL, Prisma |
| Validação | Zod |
| Testes | Vitest, Supertest, React Testing Library |
| Qualidade | Biome |
| Controle de versão | Git, GitHub |
| Integração contínua | GitHub Actions |
| Publicação | Render, Neon |

Todas as ferramentas e serviços empregados dispõem de plano gratuito. Nenhum exige meio de pagamento.

---

## Estrutura do repositório

```
.
├── README.md
├── contexto/
│   ├── constituicao.md
│   ├── especificacao-gefina.md
│   └── mapa-do-curso.md
└── aulas/
    ├── aula-01.md
    ├── aula-02.md
    └── ... até aula-48.md
```

**`contexto/`** reúne os documentos permanentes. A constituição estabelece a conduta do agente. A especificação descreve o sistema a ser construído. O mapa relaciona as quarenta e oito aulas e os conceitos de cada uma.

**`aulas/`** reúne os arquivos de aula. Cada um declara o conteúdo de um encontro e é fornecido individualmente.

---

## Requisitos

O sistema opera no Claude, com o recurso de Projetos, que conserva documentos de referência entre conversas distintas.

A disponibilidade do recurso varia conforme o plano contratado. Consulte a documentação oficial da Anthropic para verificar a condição vigente.

Nenhum outro requisito é necessário para começar. Toda ferramenta empregada no curso — ambiente de execução, editor, controle de versão, banco de dados, plataforma de publicação — é apresentada e instalada na aula em que se torna necessária, com orientação passo a passo. O mesmo se aplica às contas em serviços externos.

As oito primeiras aulas executam inteiramente no navegador e não exigem instalação alguma.

---

## Preparação

### Criação do Projeto

Crie um Projeto no Claude e adicione ao seu contexto, exclusivamente, os três arquivos do diretório `contexto/`.

### Restrição quanto aos arquivos de aula

Os arquivos de aula **não** são adicionados ao contexto do Projeto.

A restrição possui duas razões.

A primeira é operacional: quarenta e oito arquivos excedem a capacidade de permanecerem carregados em contexto, e o sistema passaria a recuperá-los por busca, o que tornaria a leitura parcial e imprevisível.

A segunda é pedagógica: o curso revela o conteúdo progressivamente, e o agente não deve dispor das aulas posteriores àquela que conduz. A presença delas comprometeria essa progressão.

---

## Início de uma aula

Abra uma conversa nova dentro do Projeto.

Anexe o arquivo da aula correspondente e indique o início. Uma linha basta:

```
Inicie a aula 01.
```

Cada aula ocupa uma conversa própria. Conduzir duas aulas na mesma conversa acumula conteúdo desnecessário e não traz benefício, uma vez que o arquivo de cada aula declara o estado do qual parte.

---

## Transcurso da aula

### Perguntas de abertura

O agente inicia com uma ou duas perguntas.

A primeira indaga se a participação se dá como aluno ou como educador. Responda conforme o caso.

A segunda, presente apenas nas aulas que empregam comandos de sistema, indaga qual o sistema operacional em uso. A resposta determina a forma dos comandos apresentados. Declarado o sistema Windows, o agente indaga ainda se o ambiente é o PowerShell ou o subsistema Linux.

As perguntas são repetidas a cada aula, uma vez que cada sessão é independente.

### Cabeçalho

Respondidas as perguntas, o agente apresenta a identificação da aula: número, tema, projeto, etapa e o que a aula entrega.

### Blocos

A aula divide-se em blocos. Cada bloco trata de um conceito e alterna explicação e prática.

Parte dos blocos produz código que integra o projeto e nele permanece. Parte apresenta código destinado apenas à observação, que não integra o projeto. O agente declara a natureza de cada trecho antes de apresentá-lo.

Alguns blocos são inteiramente conceituais e não produzem código.

### Avanço entre blocos

Concluído um bloco, o agente declara que aguarda o comando de avanço.

O avanço ocorre mediante a palavra:

```
cuida
```

A palavra é a única forma de avanço. Manifestações equivalentes — entendi, pode seguir, prossiga — não avançam: o agente as trata como observação e reafirma a forma de avanço.

A exigência é deliberada. O avanço decorre de decisão explícita, e não de leitura presumida.

### Espera

O intervalo entre blocos é o momento destinado a dúvidas.

Apresente perguntas, código, mensagens de erro ou pedidos de reexplicação. O agente atende e, ao final, reafirma que aguarda o comando.

Não há limite de tempo. O agente não avança por iniciativa própria e não sugere o avanço.

### Encerramento

Concluído o último bloco, o agente apresenta o que foi construído, o estado em que o código permanece e os critérios de conclusão da aula.

Os critérios servem à conferência. Verifique-os antes de encerrar a sessão.

---

## Prosseguimento entre aulas

Cada arquivo de aula declara o estado do qual a aula parte, e o agente o apresenta ao início.

Confira o estado antes de prosseguir. Divergência entre o estado declarado e o estado do seu projeto impede a condução e deve ser resolvida antes do primeiro bloco.

A ordem das aulas é obrigatória. Cada uma pressupõe o que as anteriores estabeleceram, e nenhuma pode ser antecipada ou omitida.

---

## Situações imprevistas

### O estado do código não corresponde ao declarado

Informe ao agente. Ele solicitará o arquivo pertinente, identificará a divergência e orientará a correção.

Apresente apenas o arquivo solicitado. O envio do projeto inteiro não auxilia o diagnóstico e prejudica a condução.

### O agente avança sem o comando

Interrompa e indique que o comando não foi dado. Solicite o retorno ao ponto em que o bloco foi concluído.

### O agente menciona conteúdo ainda não tratado

Indique que o conceito não foi apresentado e solicite que a explicação seja reformulada com os recursos disponíveis até ali.

### O agente contradiz o arquivo de aula

Indique a divergência. O arquivo de aula prevalece quanto ao conteúdo da aula.

Quando a divergência recair sobre decisão do sistema construído — estrutura de dados, denominação, regra de negócio, contrato da API, comportamento de interface —, prevalece a especificação.

### O agente apresenta resultado de execução não obtido

O agente não deve produzir saída de comando que não tenha sido apresentada por quem opera. Constatada a ocorrência, indique-a e apresente a saída real.

A saída inventada conduz à conclusão de que houve erro de quem executa, quando o erro é do agente.

### A conversa atinge o limite de extensão

Abra uma conversa nova, anexe novamente o arquivo da mesma aula e indique o ponto em que a condução foi interrompida, informando qual foi o último bloco concluído.

---

## O que o sistema não faz

O sistema conduz o ensino. Não realiza nenhuma das operações a seguir, e a ausência é deliberada.

**Não administra tempo.** O agente não estima duração, não sinaliza extensão, não propõe interrupção e não sugere adiamento de conteúdo. A aula é unidade de conteúdo, não de relógio.

**Não avalia.** Não atribui nota, não corrige exercício e não emite juízo sobre desempenho. Os critérios de conclusão destinam-se à conferência de quem opera.

**Não acompanha progresso.** Cada sessão é independente. O agente não conserva registro do que ocorreu nas aulas anteriores além do que o arquivo declara.

**Não administra calendário.** Não estabelece prazo, sequência de datas nem periodicidade.

**Não conhece o contexto de uso.** O sistema desconhece se é empregado individualmente ou em turma, e nada nele pressupõe circunstância de sala.

---

## Estrutura do curso

| Etapa | Aulas | Projeto |
|---|---|---|
| Fundamentos | 1 a 8 | Simba |
| Ferramental | 9 e 10 | Simba |
| Arquitetura base e entrega contínua | 11 a 17 | Gefina |
| Persistência e consulta de dados | 18 a 23 | Gefina |
| Operações de escrita e camada de acesso a dados | 24 a 30 | Gefina |
| Segunda entidade e sistema de design | 31 a 36 | Gefina |
| Autenticação e controle de acesso | 37 a 42 | Gefina |
| Consulta avançada e integração contínua | 43 a 46 | Gefina |
| Encerramento | 47 e 48 | Gefina |

As seis etapas intermediárias correspondem a entregas do Gefina. Cada uma conclui com uma versão publicada e funcional, e a última estabelece a versão definitiva.

O arquivo [`contexto/mapa-do-curso.md`](contexto/mapa-do-curso.md) relaciona o tema e os conceitos de cada aula.
