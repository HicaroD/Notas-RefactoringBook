# Refactoring notes

Esse documento será útil para armazenar minhas notas a respeito do livro "Refactoring: Improving the Design of Existing Code"
escrito por Martin Fowler e outros autores.

## Tabela de conteúdo

1. [Refactoring, a first example](#refactoring-a-first-example)

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
