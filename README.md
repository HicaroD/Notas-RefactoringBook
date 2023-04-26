# Refactoring notes

Esse documento será útil para armazenar minhas notas a respeito do livro "Refactoring: Improving the Design of Existing Code"
escrito por Martin Fowler e outros autores.

## Tabela de conteúdo

1. [Refactoring, a first example](#refactoring-a-first-example)
2. [Principles in Refactoring](#principles-in-refactoring)
3. [Bad Smells in Code](#bad-smells-in-code)

### Refactoring, a first example

- Um computador não se importa com código sujo, mas humanos, sim! Nós escrevemos códigos para seres humanos.

- Quando você quer adicionar uma feature em um programa e você percebe que ele não está estruturado de forma apropriada
  para poder receber aquela feature. Então, primeiramente, refatore aquele programa de forma que torne mais simples a adição daquela
  feature para depois adicionar a feature.

- Antes de refatorar, tenha certeza de que você tem um conjunto de testes direcionados ao código que você está tentando refatorar.
  Dessa maneira, você irá refatorar o código de forma um pouco mais segura porque você saberá que o comportamento do programa
  não foi alterado com a refatoração (algo que você não quer).

- "Any fool can write code that a computer can understand. Good programmers write code that humans can understand."

- Tente ler o código como se fosse um texto qualquer de um livro. Caso essa leitura não esteja fluindo como se fosse um livro,
  identifique o porquê, lá é onde você precisa refatorar.

- A ideia central é pegar o ritmo da refatoração: teste, pequena mudança, teste, pequena mudança, teste e assim por diante.
  Esse é o ritmo que te permite mover de forma rápida e, sobretudo, mais segura.

### Principles in Refactoring

- Otimização do código para performance geralmente torna o código mais sujo. Em muitas situações, refatoração e clean code
  tornam o código menos performático e isso é parte de escrever código.

  Você está disposto a desperdiçar um pouco de performance em prol de um sistema limpo onde seus desenvolvedores podem escrever
  de forma mais simples e menos custosa?

- Sem refatoração o design do código vai decair. A medida que os desenvolvedores forem cegamente adicionando mais features sem
  uma preocupação e olhos para refatoração, irá se tornar cada vez mais difícil de observar a estrutura do código apenas lendo-o.

- Refatoração de forma regular ajuda o código a manter o "shape".

- Geralmente quando codamos, nós não pensamos nos futuros desenvolvedores.

- "I'm not a great programmer; I'm just a good programmer with great habits." - Kent Beck

- Muitas pessoas pensam que quando você refatora, você fica estagnado, isso porque você não está adicionando uma nova feature ao
  sistema, você está apenas alterando o que já está feito.

  Isso é um problema.

  Quando você refatora, você está ganhando tempo. Quando você refatora, você impede que o design do software decaia.
  Você está ganhando um tempo que será extremamente valioso depois.

- Não existe um horário certo para quando você deve refatorar, mas certamente existem alguns momentos que podem indicar que
  você deve refatorar. Por exemplo, quando você adiciona uma nova feature, quando você quer consertar um bug (um código mais limpo
  te ajuda a encontrar um bug), quando você está fazendo code review

- Um conselho do Martin Fowler: não diga ao seu manager que apenas liga para "mais features" sobre refatoração.
  Nosso trabalho como desenvolvedores de software é de escrever software de qualidade e a maioria dos managers não irão entender
  que melhorar o código que já temos irá melhorar a qualidade do código futuro.

- Existem códigos que são tão ruins que seria melhor reescrevê-lo do que refatorá-lo.

### Bad Smells in Code

#### Duplicated code

- Se você ver a mesma estrutura de código em mais de um lugar diferente, tenha certeza de que o seu programa será 
  melhor caso você ache uma maneira de unificá-la.

- Uma das maneiras mais simples de conseguir resolver esse problema do código duplicado é extrair a parte 
  duplicada em um método e chamar esse método nos lugares que aquela estrutura é repetida.

#### Long method

- Aqueles programas que tendem a viver mais e bem são aqueles com métodos pequenos.

- Quanto maior o método, mais difícil de entender.

- Para extrair métodos longos, podemos extrair o método em submétodos para fazer diferentes etapas do método.

- Geralmente comentários te dão indícios de que um código deve ser refatorado. Isso porque geralmente o comentário
  tenta dar uma semântica a determinados blocos de código. Então, uma coisa que você poderia fazer é tornar aquele
  bloco de código comentado em uma função com um bom nome que denota a semântica / sentido que o comentário
  tentou passar.

- Loops e expressões condicionais também dão indícios de que aquilo deve ser extraido. Expressões condicionais
  podem ser extraidas em um método a parte e o método deve ter um nome do que aquela expressão condicional está
  checando, e a parte interna de um loop pode ser extraida em seu próprio método.

#### Large class
