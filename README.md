# Refactoring notes

Esse documento será útil para armazenar minhas notas a respeito do livro "Refactoring: Improving the Design of Existing Code" escrito por Martin Fowler e outros autores.

Além disso, irei consultar essas notas sempre que quiser refatorar / fazer code reviews e etc.

Deixarei apenas um aviso, muitas dessas notas não são 100% inspiradas no livro do Martin Fowler. Em algumas delas, eu
faço comentários baseado em experiência própria e outros livros que li sobre o tópico. Dessa maneira,
**caso queira usar essas notas como referência, use por sua conta e risco**.

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

   3.8. [Data clumps](#data-clumps)

   3.9. [Primitive obsession](#primitive-obsession)

   3.10. [Switch statements](#switch-statements)

   3.11. [Parallel Inheritance Hierarchies](#parallel-inheritance-hierarchies)

   3.12. [Lazy class](#lazy-class)

   3.13. [Speculative Generality](#speculative-generality)

   3.14. [Temporary field](#temporary-field)

   3.15. [Message chains](#message-chains)

   3.16. [Middle man](#middle-man)

   3.17. [Inappropriate Intimacy](#inappropriate-intimacy)

   3.18. [Alternative Classes with Different Interfaces](#alternative-classes-with-different-interfaces)

   3.19. [Incomplete Library Class](#incomplete-library-class)

   3.20. [Data class](#data-class)

   3.21. [Refused bequest](#refused-bequest)

   3.22. [Comments](#comments)

4. [Building tests](#building-tests)
5. [Toward a Catalog of Refactorings](#toward-a-catalog-of-refactorings)

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

#### Data clumps

- Muitas vezes você verá campos muito semelhantes andando juntos em diferentes classes. Para resolver esse problema,
  você pode extrair esses campos em uma classe nova, já que eles sempre andam juntos.

#### Primitive obsession

- Geralmente, em ambientes de programação, temos dois tipos de dados: Record e Primitives. Dados do tipo Record permitem
  que você estruture outros dados em grupos que possuem significado. Dados do tipo Primitives são "building blocks"
  para construção de dados do tipo "Record".

- Pessoas que são novas no mundo dos objetos ficam com receio de usar classes para poder resolver pequenas tarefas,
  como criar uma classe para representar um "Customer" que terá apenas o nome dele lá.

  Pensar dessa maneira não é algo tão bom, pois criar uma classe para representar esses objetos dão uma certa semântica
  e isolamos os dados específicos daquele objeto dentro de uma classe que dá sentido ao código.

- Se você tem grupos de campos que sempre vão juntos, extraia em uma classe nova.

- Se você tem tipos primitivos na lista de parâmetros, tente usar **Introduce Parameter Object**, ou seja, criar uma
  classe que possui os parâmetros como campos da classe. Só assim, diminuiríamos a quantidade de parâmetros e melhoraríamos a legibilidade do código.

- Se você se encontra usando um array para armazenar dados que juntos fazem sentido, você poderia usar **Replace Array with Object**, ou seja, criar uma classe para representar o que o conteúdo dentro do array significa.

#### Switch statements

- Já não é novidade para ninguém que, para muitos autores, como Martin Fowler e Bob Martin, switch statements não é uma boa ideia, na maioria dos casos. Tomando como referência o Martin Fowler, o maior problema dos switch statements é a duplicação de código, na medida em que você se vê colocando aquele switch statement em diversos locais do código.

- Sempre que você ver em um switch statement, pense em polimorfismo. Polimorfismo no mundo das linguagens orientadas a objetos é a solução para elimininação dos switch statements. Muitas vezes você faz switch no tipo da classe para fazer determinada tarefa, mas isso é desnecessário quando temos polimorfismo.

- Para resolver esse problema, vejamos um exemplo em Java de como o autor Martin Fowler recomenda a resolução desse problema. Para resolver isso, usamos a técnica **Replace Conditional with Polymorphism**.

  Antes:

  ```java
  class Bird {
    // ...
    double getSpeed() {
      switch (type) {
        case EUROPEAN:
          return getBaseSpeed();
        case AFRICAN:
          return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
        case NORWEGIAN_BLUE:
          return (isNailed) ? 0 : getBaseSpeed(voltage);
      }
      throw new RuntimeException("Should be unreachable");
    }
  }
  ```

  Depois:

  ```java
  abstract class Bird {
    // ...
    abstract double getSpeed();
  }

  class European extends Bird {
    double getSpeed() {
      return getBaseSpeed();
    }
  }
  class African extends Bird {
    double getSpeed() {
      return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
    }
  }
  class NorwegianBlue extends Bird {
    double getSpeed() {
      return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
  }

  // Somewhere in client code
  speed = bird.getSpeed();
  ```

  Com esse tipo de solução, não existe a necessidade da gente ficar alterando sempre o switch case, na medida em que **não existe mais um switch case**. Caso a gente criar um novo tipo de passáro, basta criar uma nova classe que `extends` de `Bird` e implementar o método `getSpeed`. Nossas alterações estão concentradas em apenas um local.

  Essa solução pode ser overkill, caso você tenha um código que você não espera mudar com frequência e o seu switch case
  possui pouquíssimos casos.

#### Parallel Inheritance Hierarchies

- Todas as vezes que você cria uma subclasse de uma classe, você precisará criar uma subclasse de outra. Para remover
  esse problema, você usa **Move Method** e **Move Field**.

#### Lazy class

- Cada classe que você cria custa dinheiro e tempo de manutenção e de entendimento. Dessa maneira, uma classe que não faz
  o suficiente para ser digna ser de uma classe a parte deve ser eliminada.

#### Speculative Generality

- Existem situações onde você pensa "eita, podemos criar x classe para poder fazer y coisa no futuro". Isso acaba
  adicionando uma complexidade ao código totalmente desnecessária no momento. O problema dessa complexidade é que ela
  não está sendo usada, ela foi criada para algo que pode ou não vir à tona no futuro.

- Se você se ver na situação onde você criou métodos, classes e etc que estão sendo apenas usadas pelos casos de testes
  e não pelo cliente real, então você deve remover isso, na medida em que ninguém usa.

#### Temporary field

- Quando temos uma classe que possui campos temporários que eles são setados e usados em certas situações tornam tudo
  mais difícil de entender. Para resolver esse problema, pegamos todos esses campos temporários e jogamos em uma classe
  específica, além disso pegamos o código que aquele campo temporário está agindo e colocamos nessa classe nova.

#### Message chains

TODO.

#### Middle Man

- Existem situações onde criamos classes que apenas recebem e passam a mensagem entre duas identidades. O problema
  é que essa classe que fica no meio não faz nada além de repassar a mensagem, esse code smell é chamado de "Middle Man". Para resolver isso, nós removemos a entidade que fica no meio e fazemos com que as duas outras identidades
  se comuniquem entre si.

#### Inappropriate Intimacy

- Existem situações onde uma classe faz a utilização de atributos e métodos privados de outra classe. Isso pode se tornar
  um problema. Em razão disso, chamamos esse code smeel de "intimidade inapropriada".

  Boas classes devem conhecer bem pouco sobre as implementações internas de outra classes, caso contrário o conceito
  de encapsulação é ferido.

- Uma possível solução é mover métodos e atributos entre as classes com o objetivo de remover a "intimidade". Se classes
  possuem um interesse em comum, então poderia ser legal extrair em uma classe onde a intimidade delas estaria sendo
  preservada.

#### Alternative Classes with Different Interfaces

TODO.

#### Incomplete Library Class

TODO.

#### Data Class

- Data classes são classes "burras" que apenas servem para carregar dados e nada mais. Uma coisa que você precisa ter
  cuidado é com atributos públicos. Use atributos privados e getters e setters.

TODO: entender o porquê que o autor afirmou isso!

#### Refused bequest

- Existem situações onde temos uma subclasse que herdam métodos de outras classes que elas não precisam. Isso não pode
  acontecer, isso significa que a hierarquia está errada.

- Uma coisa que você pode fazer é jogar os campos e/ou métodos para as subclasses que precisam daquele método ao invés
  de mantê-los na superclasse e fazer com que subclasses que não precisem daquele método sejam capazes de acessar aquele
  método e/ou atributo.

#### Comments

- Se tiver um bloco de código onde possui um comentário explicando-o, extraia aquele bloco em um método e dê um nome claro
  a esse método. Geralmente, parte do comentário se tornará o nome do método.

- Se o método já está extraído e ainda assim possui um comentário, então dê um nome melhor ao método.

- Se você tem um comentário que estipula um pré-requisito para dado método / bloco / instrução funcionar, use `assert`
  para garantir aqueles pré-requisitos e não comentários.

### Building Tests

- Se você quiser refatorar algum código, o pré-requisito mor é possuir testes sólidos.

- Geralmente consertar um bug é uma tarefa simples, mas encontrar o bug é um pesadelo. Na maioria das
  vezes, nós, programadores, passamos mais tempo debugando código do que escrevendo código em si.

- Os testes irão te ajudar a identificar bugs, adicionar novas features e, sobretudo, refatorar.
  Por que testes são tão importantes para refatoração? Na refatoração, nós não mudamos o comportamento
  do código, apenas a sua forma para algo mais palatável para seres humanos. Dessa maneira, aqueles
  testes que estavam presentes antes da refatoração ainda devem passar, na medida em que você mudou
  apenas a forma, o código **deve** produzir os mesmos resultados anteriores, mas agora ele está
  melhor para nós, humanos programadores, lermos.

- A ideia é: adicionei uma feature / função / instrução, eu adiciono um teste para isso. Isso vai me
  dar uma segurança maior na hora de refatorar o código e identificar bugs futuros.

### Toward a Catalog of Refactorings

- Esse capítulo apenas anuncia que os próximos capítulos será basicamente um catálogo de técnicas
  de refatoração que o autor catalogou durante os anos de experiência que ele possui.
