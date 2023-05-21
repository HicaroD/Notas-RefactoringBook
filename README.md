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
6. [Composing methods](#composing-methods)
7. [Moving Features Between Objects](#moving-features-between-objects)
8. [Organizing data](#organizing-data)
9. [Simplifying Conditional Expressions](#simplifying-conditional-expressions)
10. [Making Method Calls Simpler](#making-method-calls-simpler)
11. [Dealing with Generalization](#dealing-with-generalization)
12. [Big Refactorings](#big-refactorings)

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

### Composing methods

- Uma boa parte do tempo usando na refatoração é por conta de métodos longos. Métodos longos são
  grandes problemas, na medida em que eles guardam muitas informações e, ao longo do tempo,
  eles enterram a lógica do problema, tornando tudo difícil de entender.

- A chave principal para refatorar métodos longos é basicamente extrair esses métodos longos em
  outros métodos menores. Agora o método grande é composta por várias chamadas de funções com nomes
  mais claros, só assim a leitura daquele antigo método longo torna-se mais fluída.

- Muitas vezes nós extraímos tanto uma função que criamos um método que não ajuda de forma alguma
  na legibilidade do código, ou seja, teria sido melhor deixar o corpo do método extraido lá na
  função do que ter extraído para outra.

- Um grande problema que temos na extração de métodos para refatorar métodos longos são as variáveis
  temporárias, pois muitas vezes precisaremos passar as variáveis temporárias para o novo método
  extraído. Chega um momento onde variáveis locais geram problemas, dessa maneira precisaremos
  evitá-las. Para evitar esse problema das variáveis temporárias, nós usamos a técnica
  **Replace Temp With Query**.

  Essa técnica basicamente diz o seguinte: substitua todo o resultado de uma expressão que está
  armazenado em uma variável para um método e chame o método no lugar da variável temporária. Só
  assim, eliminamos a necessidade de uma variável temporária e o nome do método faz o papel dela.

- Quando uma variável temporária é usada em mais de um lugar ao longo do código, você deve separar
  essas variáveis. Assim como uma função, classe, método e etc, uma variável deve ter apenas uma
  responsabilidade. Após separar essas responsabilidades, você é capaz de aplicar a técnica
  **Replace Temp With Query** para poder remover as variáveis temporárias.

- Tem algumas situações onde temos várias variáveis locais e elas estão tão correlacionadas que você
  não consegue separá-las usando extração de métodos. Uma coisa que você pode fazer para resolver esse
  problema é usar a técnica **Replace Method with Method Object**, ou seja, pegar aquelas variáveis
  locais que estão bem correlacionadas, criar uma classe onde os atributos dela serão as variáveis
  locais.

- Se você viu que o algoritmo interno de uma função pode ser melhorado, use a técnica
  **Substitute Algorithm**.

- Mais uma vez, quando começar a extrair funções de um outra função grande, preste atenção na
  quantidade de parâmetros e nas variáveis locais. De preferência, não deve ter nenhuma variável
  local, nem parâmetros, contudo isso é quase impossível. Logo, dê seu melhor para ter a menor
  quantidade disso possível.

- Existem situações onde o corpo de um método é mais claro do que o próprio nome que você deu para
  função. Quando essa situação ocorrer, remova a função e use o corpo da função dentro do
  código ao invés da chamada de função. Essa técnica é chamada de **Inline method**.

- Quando temos o uso de uma variável temporária e essa variável temporária segura o resultado de
  alguma expressão, podemos usar **Replace Temp With Query** para refatorar e remover essa variável
  temporária.

  A ideia desa técnica é remover a variável temporária criando um método que retorna o resultado
  daquela expressão e usa a chamada daquele método no lugar da variável temporária. Só assim, a
  variável temporária seria eliminada.

  A ideia por trás de remover variáveis temporárias e locais é de evitar que elas apareçam
  em mais de um lugar no código, como em outro método ou classe. Quando transformamos em um
  método, toda a classe terá acesso àquele método e não precisamos criar essas variáveis em
  outros métodos. Vale ressaltar que **Replace Temp With Query** é um passo vital antes de
  usar a técnica de **Extract Method**.

- Temos situações onde existe uma expressão muito complexa dentro de um if, por exemplo. Uma forma
  de resolver esse problema é colocar essa expressão dentro de uma variável que explica o propósito
  daquela expressão. Essa técnica é chamada de **Introduce Explaining Variable**. Eu pessoalmente
  confesso que prefiro usar **Extract Method**, na medida em que criar uma variável nova é mais uma
  variável temporária que futuramente poderia ser um método com um nome explicativo, o que é bem
  melhor do que uma variável.

  Só que existem situações onde fica difícil de extrair um método e deixar aquela expressão complexa
  é algo ruim, dessa maneira é melhor usar a técnica **Introduce Explaining Variable**.

- Use variáveis temporárias para poder receber os valores dos parâmetros, de forma que você não
  mude o valor original do objeto.

- Existem situações onde você tem um método longo que você não consegue usar a técnica **Extract Method**
  por conta da quantidade de variáveis locais. Uma coisa que você pode fazer é pegar aquele método
  transformar em um método de outra classe e aquelas variáveis temporárias se tornariam atributos
  da classe. Essa técnica é chamada de **Replace Method with Method Object**.

- Existe uma técnica chamada **Substitute Algorithm** onde você substitui o corpo de um método por
  um algoritmo que é mais claro.

### Moving Features Between Objects

- Uma das principais decisões no design de software orientado a objetos é saber onde colocar as responsabilidades.
  Na maioria das vezes, de primeira, a classe fica cheia de responsabilidades com métodos que fazem coisas diferentes
  que não deveriam estar naquela classe. Esses sintomas dificilmente são identificados de primeira, mas com
  refatoração, isso pode ser resolvido.

- A solução mais simples para remover excesso de responsabilidades é usar **Move Field** e **Move Method** para poder
  mover aqueles métodos e fields da classe que não deveriam estar lá, pois a classe não deveria ser responsável por
  aquela funcionalidade. Muitas vezes precisamos criar outra classe para gerenciar aquela funcionalidade, outras
  vezes já temos uma classe que aquela funcionalidade é apropriada.

- Uma coisa legal para identificar esse tipo de sintoma mencionado acima é observar os métodos da classe e ver se ele
  está usando mais atributos / métodos de outras classes do que métodos da própria classe que ele vive. Isso é um
  code smell chamado de **Feature Envy** e deve ser evitado.

- Existem situações onde uma field de uma classe é usada mais por outra classe do que pela própria. Nessas
  situações a ideia é mover a field para a classe que mais usa aquela field. Ela com certeza estará em um lugar
  melhor.

- Muitas vezes você tem uma classe que está fazendo o trabalho de duas ou mais. Nessas situações a gente extrai
  novas classes para separar as responsabilidades.

- Essa situação acima ocorre bastante e nós extraímos diversas classes, contudo muitas vezes extraímos classes
  que não fazem o suficiente para serem dignas de uma classe própria e os métodos / fields poderiam
  ser colocados em uma classe já existente. Essa técnica é chamada de **Inline Class**.

### Organizing data

- Existem situações onde você tem um valor numérico que está sendo usado em vários locais. Pode ser que, inicialmente,
  você saiba exatamente o que aquele valor faz, contudo quando você voltar para esse código dias depois, provavelmente
  você terá esquecido o que aquele valor. Dessa maneira, a ideia é que você substitua aquele valor por uma constante
  simbólica, dando um nome preciso e claro para aquele valor numérico. Só assim, todos que lerem o seu código sabem
  o propósito daquele valor.

- Existem situações onde você tem um determinado tipo de dado que precisa de funcionalidades adicionais. Para dar
  essas funcionalidades adicionais, você pode criar uma classe que contém aquele dado e criar métodos que agem sobre
  aquele dado.

- Existem situações onde criamos um array para armazenar dados que fazem sentido juntos, contudo isso pode gerar uma
  confusão. Uma coisa legal a se fazer é criar uma classe e os dados do array são atributos dessa classe, só assim
  daríamos mais sentido àqueles dados armazenados no array.

- Algo bom a se fazer é encapsular coleções. Ao invés de retornarmos uma coleção de forma que o usuário possa alterar,
  você pode retornar uma coleção read-only e expor métodos que possam editar aquela coleção. Essa técnica é chamada
  de **Encapsulate Collection**. Esse encapsulamento não permitirá que o cliente altere aquela coleção sem saber
  o que ele está fazendo. Além disso, você pode adicionar regras de alteração nos métodos, só assim você pode gerenciar
  corretamente o que pode entrar e sair daquela coleção. A mesma coisa serve para outros campos em uma classe, um
  campo público faz com que outros objetos alterem seu código da maneira que eles bem quiserem, caso a gente
  torne-o privado e criar métodos (getters e setters), a gente está encapsulando a field e tornando mais segura
  a alteração desses dados, sobretudo se você adicionar regras de alteração nos setters. Essa técnica é chamada
  de **Encapsulate Field**.

### Simplifying Conditional Expressions

- A técnica principal para refatorar expressões condicionais é usar **Decompose Conditional**. Existem outras técnicas,
  como **Introduce Null Object** com polimorfismo para evitar checagem por objetos nulos.

- Falando especificamente sobre **Decompose Conditional**, essa técnica consiste em separar expressões condicionais
  complexas em métodos que deixam claro o que aquela condição está checando. Muito semelhante a um **Extract Method**,
  mas para condições.

- Muitas vezes você tem uma sequência de condições que geram o mesmo resultado, uma coisa que você pode fazer é pegar
  essa sequência de condições em um método só e usar apenas uma condição com aquele método.

- Existem situações onde você tem o mesmo fragmento de código em todas as condições. Uma coisa que você pode fazer é
  retirar aquele fragmento de código da condição e colocar depois das condições, isso funciona porque o fragmento de
  código está em todas as condições, logo ele será executado em todas as condições. Graças a isso, é melhor colocar
  depois das condições, evitando repetições desnecessárias.

- Existe muitas situações onde podemos substituir condições por polimorfismo.

- É extremamente chato e repetitivo checar por objetos nulos. Quase sempre existirá uma repetição desnecessária. Par
  resolver esse problema, podemos usar a técnica **Introduce Null Object**. Essa técnica consiste em basicamente criar
  uma classe que herda da classe que você estava checando por objetos nulos e essa nova classe vai representar um
  objeto nulo daquela outra classe, dessa maneira você não precisa checar se o objeto é nulo, pois os métodos da
  classe nula não irão fazer nada, você pode chamá-los e nenhum efeito será causado.

- As vezes criamos comentários para poder estipular pré-requisitos e dizer que "certa função não pode receber x valor,
  pois dará problema". Ao invés desse comentário, podemos criar um "assert" para garantir esse pré-requisito, é melhor
  do que criar um comentário, que não terá efeito algum. Essa técnica é chamada de **Introduce Assert**.

  Além disso, os asserts podem ser removidos no futuro após o código ser testado de forma apropriada.

### Making Method Calls Simpler

- Uma das técnicas mais simples para tornar uma chamada de método mais simples é renomear o nome desse método para
  algo mais claro a respeito do que ele está fazendo.

- Existem situações onde temos uma longa lista de parâmetros de um método. Uma possível refatoração, no caso de você estar
  recebendo uma certa quantidade de atributos de um mesmo objeto, é justamente passar o objeto inteiro. Isso iria
  ajudar a reduzir a quantidade de parâmetros. Essa técnica é chamada de **Preserve the Whole Object**. Caso
  esse objeto não exista, então você pode criar uma classe que representa os parâmetros passados, essa técnica é
  chamada de **Introduce Parameter Object**.

- Error codes nem sempre são uma boa ideia, pois muitas vezes eles se tornam confusos para tratar. Uma coisa que você
  pode fazer é substituir esse error codes por exceções.

- Quando o nome do método não revela seu próposito, mude o nome do método.

- Quando você tem uma certa quantidade de parâmetros que naturalmente vão juntos, você pode criar uma classe que leva
  todos juntos, só assim podemos substituir o parâmetro por um objeto e diminuir a quantidade de parâmetros, dessa maneira
  podemos simplificar a chamada do método no futuro.

### Dealing with Generalization

- Quando duas ou mais subclasses possuem o mesmo campo, podemos mover essa field para super classe (parente), só
  assim evitamos a repetição de fields. A mesma coisa serve para métodos. Mas, por quê? Apesar de que você mantenha
  as fields e métodos, eles se tornarão suscetíveis a erros no futuro, pois você precisa alterar em mais de um lugar
  e possivelmente irá esquecer de alterar em todos os lugares, especialmente se você tiver muitas subclasses.

- Existem situações também onde você tem um método na superclasse que não é usada por todas as subclasses, nessa
  situação, a ideia é que agora você "desça o método" (ou field) para as subclasses. Isso porque não queremos
  expor métodos / fields a classes onde não é relevante.

- Quando temos duas ou mais classes que você possuem características semelhantes, podemos extrair uma super classe
  e fazer com que essas classes herdem da super classe. Só assim, podemos aplicar os dois métodos de refatoração
  mencionados acima para poder deixar essas classes e subclasses mais legíveis.

- Quando diversas classes usam o mesmo subconjunto de métodos, podemos extrair uma interface e fazer com que elas
  implementem essa interface.

- Cuidado para não adicionar mais classes só por adicionar e achar que está refatorando o código. Quando uma superclasse
  e subclasse não são tão diferentes, então é melhor você mesclar as duas e ter apenas uma classe.

- Você pode usar o **Template Method** em situações onde temos passos semelhantes de um algoritmo, mas algumas etapas
  são diferentes.

### Big Refactorings
