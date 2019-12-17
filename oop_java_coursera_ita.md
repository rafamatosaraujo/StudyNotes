# Orientação a objetos com Java

### Resource: https://www.coursera.org/learn/orientacao-a-objetos-com-java

***
As entidades que colaboram para criar as funcionalidades do sistema são as `classes`, que são abstrações. Os `objetos` são instâncias concretas das `classes`.

![Diagrama representando instâncias de uma classe Cadeira](/Images/OOP_Java/diagram_Cadeira.png)

- As características de um objeto de determinada classe são chamados de `atributos`. Eles representam o estado daquela instância. Os `atributos respondem a seguinte pergunta: "O que a classe sabe?"
- As ações que um objeto de determinada classe pode realizar são representadas pelos `métodos`. Os métodos responsem a seguinte pergunta: "O que a classe faz?"

![Tabela representando a diferença entre classes e objetos](/Images/OOP_Java/tabela_classes_objetos.png)

Os `construtores` são métodos especiais para criar objetos de classes. Através dele é possível parametrizar o objeto criado e inicializar variáveis. Uma classe pode ter mais de um construtor desde que sejam diferentes.

A `responsabilidade` de uma classe é repesentada pelo que ela sabe e o que ela faz, ou seja, seus `métodos` e `atributos`. A lógica da responsabilidade nada mais é que o algorítimo correspondente dos métodos da classe.

***

### Relacionamento entre classes

Na programação orientada a objetos é importante escrever um código com responsabilidades bem divididas.

![Ilustração representando um código com responsabilidades bem divididas e outro não](/Images/OOP_Java/blocos_responsabilidades.png)

***

### Métodos e atributos estáticos

Atributos e métodos estáticos são aqueles compartilhados por qualquer instância de uma classe. Não é necessário uma instância da classe para acessar um atributo ou métodos estático.

**Atenção:** Não utilize variáveis estáticas como variáveis globais.

Métodos estáticos geralmente são utilizados como métodos auxiliares.

***

### Modelagem CRC

Uma abordagem informal para modelar uma aplicação com classes e objetos.

![Exemplo de cartão CRC](/Images/OOP_Java/cartao_crc.PNG)

- `Passo 1:` Especifica o sistema (formato de histório)
- `Passo 2:` Identifica objetos e classes. Substantivos/nomes na especificação são potenciais objetos e classes.
- `Passo 3`: Redefinir lista. Identificar nomes diferentes que representam a mesma classe e retirar nomes que representam atributos. Identificar classes e subclasses descrevendo o que cada uma faz.
- `Passo 4:` Identificar responsabilidades óbvias de cada classe.
- `Passo 5:` Valores da especificação como responsabilidades do tipo `FAZ` (métodos).
- `Passo 6:` Atribuição das novas responsabilidades. Para cada potencial responsabilidade verifique a classe a ser atribuida.
- `Passo 7:` Identificar colaborações.

***

### Herança

É um recurso da orientação a objetos que permite que classes especializem ou generalizem outras classes. A partir da herança é possível estender uma classe, adicionando atributos e modificando comportamentos.

![Exemplo de herança](/Images/OOP_Java/exemplo_heranca.png)

Ao estender uma classe é possível:

- Adicionar / modificar métodos
- Adicionar atributos

Não é possível: 

- Remover métodos
- Remover atributos

Ao estender uma classe é preciso horar o `contrato` da superclasse, a partir de seus métodos e atributos.

***

### Modificadores de acesso

Os modificadores de acesso permitem que os métodos e atributos de uma classe só possam ser acessados por aqueles que possuem permissão.

Um exemplo sobre a necessidade de utilizar os modificadores de acesso:

```java
Carro carro = new Carro();
carro.velocidade = 100;
```

No exemplo acima, aumenta-se a velocidade do carro através de uma instância da classe. Sendo que o correto seria chamar um método para isso. Como por exemplo: 

```java
carro.acelerar();
```

- `private` -> Só pode ser acessado por membros da classe
- `public` -> Pode ser acessado por qualquer outra classe
- `protected` -> Só pode ser acessado por subclasse e classes do mesmo pacote
- `default`(vazio) -> Acessível para todas as classes do mesmo pacote


