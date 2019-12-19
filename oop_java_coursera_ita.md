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

Cuidado para não expor demais os membros das classes. Algumas variáveis só podem ser modifcadas de certas formas. O acesso direto a essas variáveis pode quebrar o funcionamento da classe.

***

### Sobrescrita de métodos

É possível sobrescrever métodos de uma superclasse a fim de modificar seus comportamentos e ainda assim continuar honrrado o contrato da superclasse.

Quando uma classe não pode ser estendida ou um método não pode ser sobrescrito, estes devem ser identificados como `final`.

```java
//classe
public final class Empregado { ... }

//método
public final class Empregado {

    public final double salario() {  ... }
}
```

Já em variáveis, o `final` tem um significado diferente. Neste caso ele indica que não é possível atribuir outro valor à variável em questão.

***

### Classes abstratas

Alguns conceitos são muito abstratos para serem instanciados (ex: veículo). Nessa caso cria-se classes abstratas.

```java
public abstract class Veiculo { ... }

//Errado
Veiculo v = new Veiculo()

//Correto
public class Carro extends Veiculo { ... }
```

Esse tipo de classe não pode ser instanciada, apenas estendida.
Uma classe abstrata também pode possuir métodos abstratos, que precisam ser implementados pelas subclasses.

```java
public abstract class Veiculo {
    public abstract void mover();
    public abstract String getPosicao();
}
```

Em resumo, as classes abstratas definem um contrato e funcionalidades comuns a um grupo de classes.

***

### Encapsulamento

O encapsulamento permite com o que desenvolvedor trabahe em um código sem necessidade de conhecê-lo por completo, apenas a superfície do que interage com a parque em que elestá desenvolvendo. Por isso é importante pensar o software de forma encapsulada, dentro de classes e componentes.

Quando um método de uma classe retorna uma objeto completo (array ou objeto), ele está retornando um apontamento para atributo da classe, e nao uma nova referência. Isso pode gerar problemas, pois ao fazer qualquer alteração no atributo retornado, altera-se também o atributo da classe. Isso é chamado de `quebra de encapsulamento`.

**Soluções:**

1 - Retornar um clone

```java
public class Atriz {

    private Ator amigo;

    public Ator getAmigo() {
        Ator at = new Ator();
        //Copia as informações de amigo
        return at;
    }
}
```

2- Esconder o objeto

```java
public class Atriz {

    private Ator amigo;

    public String getNomeAmigo() {
        return amigo.getNome();
    }

    public int getIdadeAmigo() {
        return amigo.getIdade();
    }
}
```

***

### Acoplamento

Acoplamento descreve os relacionamento e dependências entre classes. Um `acoplamento alto` é carcaterizando quando existem muitas dependências entra uma classe e outra, ou entre uma classe e várias outras classes. O `acoplamento baixo` é o contrário.

`Quanto maior o grau de acoplamento, pior para o projeto.`

![Exemplificação de coplamento alto e baixo](/Images/OOP_Java/acoplamento.png)

Sempre utilize o princípio `Tell, don't ask` para reduzir o acoplamento das classes. Basicamente ele diz para não pedir ao objeto a informação necessária para realizar um trabalho, mas sim ordene à classe que realize o trabalho.

***

### Interfaces

Quando classes possuem comportamentos em comum, mas não possuem um conceito em comum, não faz sentido utilizar herença.

**Exemplo:** Classe Cavalo e classe carro. Ambas se movem (comportamento em comum), mas um cavalo não tem um conceito em comum com carro. 

Nesse caso ao invés de utilizar a herença, faz-se uso das interfaces para abstrair um comportamento. A interface também é um `contrato` que define os métodos que uma classe precisa ter.

```java
public interface Movel {
    public void mover();
    public void parar();
    public double getVelocidade();
    //Todos os métodos de uma interface são abstratos
}

public class Cavalo implements Movel {
    // Precisa implementar todos os métodos da interface
}

public class Carro implements Movel {
    // Precisa implementar todos os métodos da interface
}
```
**OBS:** Uma interface pode estender uma ou mais interfaces. Da mesma forma, uma classe pode implementar mais de uma interface.

***

### Polimorfismo

Em um software orientado a objetos, uma classe pode assumir a formas de sua superclasse ou de uma de suas interfaces.

```java
Cavalo cavalo = new Cavalo();
Animal animal = cavalo;

// Um objeto do tipo Cavalo sendo atribuído à uma variável do tipo Animal, sua superclasse.
```
Um objeto pode ser sempres passado em parâmetros do tipo de sua superclasse.

```java
Carro carro = new Carro();
//Carro implementa a interface Movel

void addCorrida(Movel m) {
    m.mover(LARGADA);
}

addCorrida(carro);
```
Ao chamar o método `mover` no examplo acima, o método da classe Carro é chamado, não da interface Movel. Isso implica que o mesmo código pode ter diferentes comportamentos dependendo do objeto que está sendo utilizado.



