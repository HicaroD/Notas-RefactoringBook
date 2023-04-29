# Refactoring notes

Esse documento será útil para armazenar minhas notas a respeito do livro "Refactoring: Improving the Design of Existing Code" escrito por Martin Fowler e outros autores.

Além disso, irei consultar sempre que quiser refatorar / fazer code reviews e etc.

Deixarei apenas um aviso, muitas dessas notas não são 100% inspiradas no livro do Martin Fowler. Em algumas delas, eu
faço comentários adicionados baseado em experiência própria e outros livros que li sobre o tópico. Dessa maneira,
**caso queira usar essas notas para tomar como referência de algo, use por sua conta e risco**.

## Tabela de conteúdo

1. [Refactoring, a first example](#refactoring-a-first-example)
2. [Principles in Refactoring](#principles-in-refactoring)
3. [Bad Smells in Code](#bad-smells-in-code)

   3.1. [Duplicated code](#duplicated-code)
   3.2. [Long method](#long-method)
   3.3. [Large class](#large-class)
   3.4. [Long parameter list](#long-parameter-list)
   3.5. [Divergent change](#divergent-change)
   3.6. [Shotgun surgery](#shotgun-surgery)
   3.7. [Feature Envy](#shotgun-surgery)

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

- Muitas situações quando extraímos métodos para refatorar nosso código, a gente acaba passando
  muitos parâmetros e variáveis temporárias para poder atingir nossos objetivos. Ter muitos parâmetros e muitas variáveis temporárias não é algo legal.

  Dessa forma, para retirar algumas (ou até todas) variáveis temporárias, podemos usar **"Replace Temp with Query"**, ou seja, ao invés de usar uma variável temporária para armazenar algum valor temporário, podemos criar um método que calcula esse valor temporário e passar lá ao invés da variável.

  Quando temos uma lista longa de parâmetros, podemos resolver com **"Introduce Parameter Object"**, ou seja, se a lista de parâmetros tiver uma certa semântica, podemos criar uma classe onde cada campo da classe será um elemento da lista de parâmetro, só assim teremos bem menos parâmetros e um objeto parametral conciso e simples. Além disso, também podemos usar **"Preserve Whole Object"**, ou seja, você obtem diversos valores de um objeto e passa
  esses valores como parâmetros (isso aumenta o número de parâmetros), para evitar isso, é melhor passar o objeto inteiro.

- Existem situações onde você tem duas classes com a mesma expressão. Para resolver isso, você pode criar uma classe maior que possui essa expressão e herda essa classe para as outras, só assim ambas terão as mesmas fields e você não precisará gerenciar as duas ao mesmo tempo, já que são expressões iguais.

#### Large class

- Um indício de que uma classe está fazendo coisa demais é uma grande quantidade de atributos. Esse problema
  vai tornar a classe grande demais, código duplicado estará presente, muitas vezes, e dentre outros problemas.

- Uma das soluções para tornar classes menores é fazer algo semelhante ao que fazemos com funções: extrair em outras
  classes onde cada uma terá um papel na classe maior.
  Muitas vezes um indício de que podemos extrair alguns atributos são os nomes que damos a esses atributos. Ex.:

  - "depositAmount" e "depositCurrency", esse prefixo "deposit" pode ser um indício de que deveríamos criar uma
    classe chamada "Deposit" e dentro dela teríamos "amount" e "currency".

#### Long Parameter List

- Antigamente, você era instigado a passar muitos parâmetros para funções porque isso ajudaria a evitar variáveis
  globais. Variáveis globais são ruins e geralmente a maior causadora de bugs, especialmente se você está lidando
  com múltiplas threads acessando o mesmo valor no escopo global.

  Hoje em dia, temos outras visões a respeito da lista de parâmetros. Quanto menos, melhor!
  Uma lista longa de parâmetros tornam o código difícil de entender.

  // TODO: aprender como resolver esse problema da lista longa de parâmetros

#### Divergent Change

- Se sua classe muda por mais de uma razão, extraia-a em outras classes. Uma classe deve ter apenas uma razão para
  mudar. CADA CLASSE DEVE POSSUIR APENAS UMA RESPONSABILIDADE.

- Quando uma classe muda de forma diferentes por diferentes razões, nós chamamos isso de
  alteração divergente (divergent change)

#### Shotgun surgery

- Esse tipo de code smell é bem semelhante ao [anterior](#divergent-change), mas é o oposto. Quando precisamos fazer alguma alteração e, para isso, faz-se necessário fazer pequenas mudanças em diversas classes diferentes. Isso é um problema, na medida em que, caso as alterações estejam em todos os lugares, fica difícil de encontrar o problema e é simples de você se perder e esquecer alguma alteração importante.

- Nesse caso, precisaremos mover os métodos e os campos para uma classe só para evitar que
  as mudanças sejam feitas em diferentes classes. As mudanças estarão concentradas em uma classe.

- Caso nenhuma classe pareça ser uma boa candidata para receber os novos métodos e campos, então crie uma nova.

- Alteração divergente é uma classe que sofre diversas alterações por razões diferentes, já shotgun surgery são várias classes que recebem várias alterações.

#### Feature Envy

- A ideia de termos objetos é de reunir dados com métodos que possam operar nesses dados. Tomando isso como referência, existe um code smell onde existem métodos em certas classes que estão mais interessados nos dados de outras classes do que da sua própria classe.

- Por isso é chamado de "feature envy" (funcionalidade invejosa), na medida em que o método "inveja" os dados de outra classe.

- Podemos identificar um "feature envy" ao observar que um método invoca um monte de métodos de outras classes para calcular algum valor. Isso é uma "feature envy". Já que esse método quer tanto invocar os dados de outra classe, então é melhor deslocar esse método para essa classe, ele estará melhor lá dentro.

- Esse problema pode ser facilmente resolvido movendo o método para essa classe que ele está "invejando" os dados. Claro que nem sempre essa solução irá funcionar, pois ele pode estar "invejando" os dados de diversas classes, mas certos casos podem ser refatorados dessa maneira.
