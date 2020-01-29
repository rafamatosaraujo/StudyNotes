# TDD - Desenvolvimento de Software guiado por testes

### Resource: https://www.coursera.org/learn/tdd-desenvolvimento-de-software-guiado-por-testes

***

#### Introdução

`Test Driven Development` é uma técnica de desenvolvimento e projeto de software em que os testes são criados antes do código de produção. Não é uma técnica de testes, mas sim uma técnica de desenvolvimento que utiliza testes como ferramenta.

![Ciclo do TDD](/Images/TDD/tdd.png)

O ciclo do TDD funciona da seguinte forma

1 - Primeiro cria-se um teste, e ao executá-lo obviamente ele irá falhar jáz que não existe um código implementado.

* Projetar a interface da classe que vai ser criada
* Definir o comportamento esperado da classe

2 - Implementa-se o código para fazer o teste passar.

* Criar o comportamento da classe
* Buscar a solução mais simples para os testes existentes

3 - Uma vez que o teste está passando, faz-se um refactor para atender às boas práticas de desenvolvimento de software.

* Limpar o código que foi criado
* Ajustar o design da classe para seu estado atual

`Baby Steps` -> Deve-se alternar com frequência entre criar testes e código de produção.

***

#### Refatoração

Refatoração é a transformação de um código escrito sem qualidade em um código escrito com qualidade. Essa transformação deve tornar o código:

* Mais fácil de ser compreendido (leitura)
* Mais fácil de ser modificado (manutenção)
* Sem alterar seu comportamento observável

Segundo [Martin Fowler](https://pt.wikipedia.org/wiki/Martin_Fowler): "Code refactoring é uma série de pequenos passos, cada um dos quais muda a estrutura interna do programa sem alterar o seu comportamento externo."

Um código de qualidade é:

* Fácil de ler
* Fácil de compreender
* Fácil de promover mudanças

O que não fazer durante a refatoração:

* Não se adiciona nova característica ou responsabilidade
* Não se adiciona teste

***

#### Princípios do TDD

1 - Feedback rápido é importante para o aprendizado

2 - Usar a solução mais simples que funciona

3 - O design é evolutivo e segue as necessidades da aplicação (nunca tente prever uma mudança)

4 - Os testes trazem segurança para que funcionalidades não sejam quebradas

5 - Os testes são uma documentação sem atualizada de como a classe funciona

6 - Todo código é culpado até que se proce inocente

7 - É mais divertido testar antes

***

#### Mitos e lendas do TDD

`TDD sempre usa teste de unidade` -> A maior parte das vezes o TDD é feito utilizando teste de unidade, mas nada impede que seja feito TDD com testes de componenete ou integração. O benefício do teste de unidade é que ele ajuda a desacoplar a classe de suas dependências. O importante é conseguir manter um ritmo alternado entre o código de teste e de produção.

`Ao criar todos os testes primeiro para depois implementar, também está fazendo TDD` -> No TDD é importante avançar em pequenos passos, trabalhando um teste por vez. Lembre-se sempre dos baby steps, é muito improtante alterat entre testes e código de produção.

`TDD faz o código ficar a prova de erros` -> O TDD pode ajudar a prevenir muitos erros, mas não garante que não vão acontecer erros. Algum caso especial pode ter sido esquecido nos testes, a implementação do teste pode estar errada, o requisito pode ter sido mal interpreteado, e etc. Há vários motivos que podem fazer com que o código esteja errado mesmo com a utilização do TDD.

`O desenvolvedor não pode testar o próprio código` -> Existem diferentes tipos de testes com diferentes objetivos. O objetivo do teste feito no TDD é verificar se o código tem o comportamente esperado pelo desenvolvedor, mas ele não elimina nenhum outro tipo de teste que o cliente deseje.

***

#### O papel do mau cheiro (bad smell)

Para definir o que deve ser refatorado, primeiro é preciso examinar o código em busca de coisas que não estão bem implementadas no código, ou que podem causar dor de cabeça no futuro caso não seja corrigido. Esses sintomas são chamadas de `mau cheiro`.

Tipos mais comuns de mau cheiro:

* Nome inadequado de métodos e variáveis
* Código duplicado
* Método grande (máximo de 10 linhas)
* Classe grande (God class)
* Comandos de `if` e `switch` de forma abusiva
* Inveja de característica -> Tell don't ask
* Intimidade imprópria -> Parecido com a God class mas em uma menor escala
* Comentários

**O ciclo da refatoração**

![Ciclo do TDD](/Images/TDD/ciclo_refactor.png)

***

#### Casos de testes a partir de responsabilidades

Os casos de testes devem ser definidos a partir dos requisitos do projeto. Para cada responsabilidade identificada obetem-se uma lista de casos de testes.

O ciclo do TDD vai consumir casos de testes relativos a uma dada repsonsabilidade.

![Ciclo do TDD](/Images/TDD/req_cicloTDD.png)

1 - Para cada requisito defina "N" casos de testes

2 - Resolva cada caso de teste listado no passo anterior sguindo ciclo do TDD

3 - Se houver mais requisitos repita os passos 1 e 2 até terminar de mapear todos os requisitos

***

#### Testando classes com dependências

Ao se deparar com o teste de uma classe que possua dependências externas, e importate isolar essas dependências, pois:

* A dependência pode demorar para executar
* A dependências pode depender da infraestrutura
* A dependência pode ter comportamento complexo

O TDD pode definir não sómente a interface interna de uma classe, mas também a interface das dependências. Para fazer isso, deve-se trocar a dependência por uma `Mock object`.

* O `Mock object` é um objeto simulado que copia o comportamento de uma objeto real de forma controlada.

Mas nem sempre é possível trocar as dependências por um mock. Quanto menor o desacoplamento da classe, mais fácil é utilizar o mock. As classes precisam permitir que os mocks possam ser colocados no lugar de suas dependências, e para conseguir isso é importante utilizar `injeção de dependências`.

As três diretivas de um mock object são:

* Imitar a interface da dependência
* Simular o comportamento do cenário de teste
* Verificar as chamadas esperadas da classe