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

