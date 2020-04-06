# SOLID Princípios de design de software em Java

### Resource: https://app.pluralsight.com/library/courses/solid-software-design-principles-java/table-of-contents

***

## Parte 1 - Introdução

### Problemas que surgem quando os princípios do SOLID não são utilizados

**Fragilidade** - tendência de um software quebrar em vários lugares diferentes cada vez que ocorre um mudança no código.

**Rigidez** - tendência de um software ser difícil de mudar, mesmo quando as mudanças são simples. Cada mudança causa um efeito cascata de mudanças subsquentes em módulos dependentes.

**Débito técnico** - Custo de priorizar uma entrega rápida em detrimento de uma entrega mais demorada, porém com com um código de melhor qualidade.

**Custo do débito técnico com o passar do tempo**

![Gráfico - custo do débito técnico com o passar do tempo](/Images/SOLID_Java/custo_debito_tecnico.png)

**Custo do débito técnico com o passar do tempo em relação ao tempo de resposta**

![Gráfico - custo do débito técnico com o passar do tempo em relação ao tempo de resposta](/Images/SOLID_Java/custo_debito_tecnico_vs_resposta_para_client.png)

Os dois gráficos são bem simples de entender. Quanto maior a quantidade de débitos técnicos no projeto, maior o tempo de resposta do time em relação à qualquer solicitação, consequetemente, maior o custo de manter a aplicação.

Entretanto, independente da qualidade do time, todo projeto terá débitos técnicos, e eles irão se acumular ao longo do tempo. O importante, ao saber disso, é dedicar tempo para refatorar o código e sanar os débitos técnicos para mantê-los sob controle.

### SOLID

Acrônimo para cinco princípios de design, destinados a facilitar a compreensão, o desenvolvimento e a manutenção de software.

* Princípio da Responsabilidade Única - (SRP - Single Responsability Principle)
* Princípio Aberto-Fechado - (OCP - Open Closed Principle)
* Princípio da Substituição de Liskov (LSP - Liskov Substituition Principle)
* Princípio da Segregação de Interfaces (ISP - Interface Segregation Principle)
* Princípio da Inversão de Dependência (DIP - Dependency Inversion Principle)

***

### SRP - Princípio da Responsabilidade Única

Cada função, classe ou módulo deve ter apenas uma razão para mudar.

`Razão para mudar = Responsabilidade`

**Exemplos de responsabilidades:**

* Regras de negócio
* Interface do usuário
* Persistência
* Log
* Orquestração
* Ususários

Sempre reduza as razões que um componente tem para mudar para apenas uma.

**Por usar o SRP:**

* Deixa o código mais fácil de entender, consertar e manter
* Classes menos acopladas e mais resisilentes à mudanças (fragilidade e rigidez em nívieis bem baixos)
* Mais fácil de testar

#### Identificando multiplas razões para mudar

**Operadores If e Switch**

```Java
if (employee.getMonthlyIncome() > 2000) {
    // Uma razão para mudar
} else {
    // Outra razão para mudar
}
```

A lógica dentro do `if` é uma razão para mudar, enquanto a lógica dentro do `else` é outra razão para mudar. O que poderia ser feito aqui é extrair a lógica de dentro do `if` para uma classe ou método. E fazer o mesmo com a lógica de dentro do `else`.

O mesmo acontece com os `operadores Switch`. Cada `case` representa uma responsabilidade.

**Métodos muito grandes**

```Java
Income getIncome(Employee e) {
    Income income = employeeRepository.getIncome(e.id);
    StateAuthorityApi.send(income, e.fullname);
    Payslip payslip = PayslipGenerator.get(income);
    JsonObject payslipJson = convertToJson(payslip);
    EmailService.send(e.email, payslipJson);
    ...
    return income
}
```

No exemplo acima o método se chama `getIncome`. Ao ler o nome do método espera-se que ele apenas retorne o valor da renda do empregado. No entanto o método faz diversas operações além de retornar o valor esperado.

Ou seja, esse metódo tem muitas responsabilidades e consequentemente muitas razões para mudar.

**Classes Deusas (God Classes)**

```Java
class Utils {
    void saveToDb(Object o) {...}
    void convertToJson(Object o) {...}
    byte[] serialize(Object o) {...}
    void log(String msg) {...}
    String toFriendlyDate(LocalDateTime date) {...}
    int roundDoubleToInt(double val) {...}
}
```

É melhor ter classes especializadas em alguns casos de uso do que criar classes grandes para salvar códigos não tão importantes. Por exemplo, crie uma classe `DateUtils` para tratar apenas de datas.

**Pessoas**

```Java
Report generate() {
    // metódo utilizado pelos atores RH e Gerentes
    // em algum momento cada um vai precisar funcionalidades diferentes
}
```

Por causa dos múltiplos atores que utilizam o método, é melhor separá-lo em dois. Dessa forma cada método terá sua única razão para mudar. Identificar esse tipo de situação é bem complicado, e depende muito do conhecimento do negócio.

**Exemplo de SRP**

```Java
class ConsoleLogger {
    void logInfo(String msg) {
        System.out.println(msg);
    }

    void logError(String msg, Excepetion e) {...}
}
```

#### Perigos de escrever um código com responsabilidades múltiplas

* Código mais difícil de ler e interpretar
* Pior qualidade devido a dificuldade de testar
* Acoplamento alto - nível de interdependência entre componentes (Se o módulo A sabe muito sobre o módulo B, qualquer mudança interna no módulo B pode quebrar as funcionalidades do módulo A)

***

### OCP - Princípio Aberto-Fechado

Classes, funções e módulos devem ser fechados para modificação, porém abertos para extensão.

* Fechado para modificação - Uma funcionalidade nova não deve modificar o código existente
* Aberto para extensão - Um componente deve poder ser extendido para se comportar de formas diferentes

Em um sistema complexo, fazer mudanças em uma classe existente pode afetar diversas outras partes do software que dependem dessa classe. 

Algumas vantagens de aplicar o OCP:

* Novas funcionalidades podem ser facilmente implementadas e com um custo mínimo
* Minimiza o risco de bugs de regressão
* Diminui o acoplamento ao isolar as mudanças em um componenete específico (trabalho junto com o SRP)

#### Estratégias para implementar o OCP

Existem duas boas formas de extender as capacidades de uma classe sem quebrar o OCP. A primeira forma é através da `Herança`. E a segunda através de uma pdrão de projeto chamado `Strategy`.

Exemplo:

```Java
public class BankAccount {
    ...

    void trasnferMoney(double amount) {
        ...
    }
}
```

A classe acima possui a funcionalidade para transferir dinheiro internamente (no mesmo pais). Porém existe a necessidade de implementar a funcionalidade de transferir dinheiro externamente.

A pior maneira de se atingir esse objetivo seria editando o código existente.

**Herança**

```Java
public class InternationalBankAccount extends BankAccount {
    ...

    @Override
    void trasnferMoney(double amount) {
        // lógica para transferência internacional
    }
}
```

O único problema da herança é que ela aumenta o acoplamento do seu código, especialmente quando se usa uma classe concreta como base.

**Padrão de projeto Strategy**

```Java
//Processador de trasnferência de dinheiro
public interface MoneyTransferProc {
    public void trasnferMoney(double amount);
}
```

Ao invés de criar uma nova classe com a nova funcionalidade, cria-se uma interface. E então cria-se quantas classes forem necessárias para implementar essa interface.

```Java
public class BankAccount implements MoneyTrasnferProc {
    public void trasnferMoney(double amount) {...}
}

public class InternationalBankAccount implements MoneyTrasnferProc {
    public void trasnferMoney(double amount) {...}
}
```

A grande vantagem dessa abordagem, é que as classes podem progredir de forma independente, uma vez que não existe acoplamento entre elas.

Após criar a `Strategy` é necessário contrauir uma `Factory` que será capaz de contruí-las de acordo com um tipo de dado.

```Java
public class MoneyTrasnferProcessorFactory {
    public void MoneyTrasnferProc build(TrasnferType type) {
        if (type == TrasnferType.Local) {
            return new BankAccount();
        } else if (type == TrasnferType.International) {
            return new InternationalBankAccount();
        }
    }
}
```

Com isso, ao processar pagamentos pode-se fazer uso de um método mais genérico.

```Java
void processPayment(double amount, TrasnferType type) {
    MoneyTrasnferProc mtp = factory.build(type);
    mtp.trasnferMoney(amount);
}
```

**Estratégia para refactoração**

1 - Pequenas mudanças - Faça as mudanças inline. (Bugfix pode ser feito dessa forma)

2 - Mais mudanças - Considere herança

3 - Muitas mudanças / Descisão dinâmica - Considere interfaces de padrões de projeto

***

### LSP -  Princípio da Substituição de Liskov

Esse princío tem várias definições:

* *Se F é um subtipo de T, logo objetos do tipo T podem ser substituídos por objetos do tipo S sem alterar as funcionalidades do software*
* *Qualquer objeto de um tipo deve ser substituível por objetos derivados desse tipo, sem alterar as funcionalidades do software*

O princípio de Liskov está completamente ligado à relação entre os objetos de um software. Em orientação a objetos, muitas vezes somos levado a pensar na relação `é um`.

*O quadrado `é um` retângulo*

Entretanto, para seguir o princípio da substituíção de Liskov, precisa-se substituir a relação `é um` por uma relação `é substituível por`.

*A classe retângulo `pode ser completamente substituída` pela classe quadrado?*

#### Violações do LSP

```Java
class Passaro {
    public void voar(int altitude) {
        setAltitude(altitude);
        ...
    }
}

class Avestruz extends Passaro {
    @Override
    public void voar(int altitude) {
        // Não faz nada, pois uma Avestruz não pode voar
    }
}

Passaro avestruz = new Avestruz();
avestruz.voar(1000)
```

No exemplo acima, o software irá funcionar normalmente. Entretanto, ao chamar o método voar, nada irá acontecer, uma vez que um avestruz não sabe voar apesar de ser um pássaro.

Nesse caso nota-se uma violação do princípio de Liskov. Apesar de na vida real o avestruz ser um pássaro, em orientação a objetos essa relação não pode acontecer, pois `o pássaro não é completamente substituível pelo avestruz`.

```Java
class Rectangle {
    public void setHeight(int height) {...}
    public void setWidth(int width) {...}

    public int calculateArea() {
        return this.width * this.height;
    }
}

class Square extends Rectangle {
    public void setHeight(int height) {
        this.height = height;
        this.width = height;
    }

    public void setWidth(int width) {...}
}

Rectangle r = new Square();
r.setWidth(10);
r.setHeight(20);
r.calculateArea(); // Retorna 400
```

No exemplo acima, apesar de um quadrado ser um retângulo do ponto de vista matemático, do ponto de vista do software existe uma grande inconsistência. 

Ao utilizar o método setWidth, não se espera que a altura também seja configurada. E o mesmo ocorre com o método setHeight.

Por isso, `o retângulo não é completamente substituível pela classe quadrado`.

```Java
interface Account {
    void processLocalTransfer(double amount);
    void processInternationalTransfer(double amount);
}

class SchoolAccount implements Account {
    void processLocalTransfer(double amount) {...}

    void processInternationalTransfer(double amount) {
        throw new RuntimeException("Not implemented");
    }
}

Account account = new SchoolAccount();
account.processInternationalTransfer(1000); // Quebra a aplicação
```

Uma violção do princípio de Liskov acontece toda vez que, ao implementar uma interface, um método retornar uma exception por falta de implementação. Isso também viola o princípio da segregação de interfaces (ISP).

```Java
for(Task t: tasks) {
    if (t instanceof Bugfix) {
        Bugfix bf = (Bugfix)t;
        bf.initializeBugDescription();
    }
    t.setInProgress();
}
```

Sempre que é necessário verificar o tipo de um objeto para realizar uma tarefa, como no exemplo acima, temos um indicativo da quebra do princípio de Liskov. Isso indica que o tipo de objeto com o qual se está trabalhando não pode ser substiuído pelo tipo adequado.

#### Refatorando o código

* Elimine relações incorretos entre objetos
* Utilize o princípio "mande, não pergunte!" (*tell, don't ask*) para eliminar casting e verificação de tipos

```Java
class Passaro {
    public void voar(int altitude) {...}
}

class Avestruz {
    ...
}
```

No exemplo do avestruz, o correto seria criar duas classes diferentes, sem um relacionamento de herança entre as duas. O mesmo pode ser feito no exemplo do quadrado.

```Java
class SchoolAccount implements LocalAccount {
    void processLocalTransfer(double amount) {...}
}
```

No exemplo da escola, ao invés de criar apenas uma interface com dois métodos, crie duas interfaces e utilize apenas àquela que for necessária.

```Java
class BugFix extends Task {
    @Override
    public void setInProgress() {
        this.initializeBugDescription();
        this.setInProgress();
    }
}

for (Task t: tasks) {
    t.setInProgress();
}
```

No exemplo do bugfix, ao invés de perguntar dentro do loop se o objeto era do tipo Bugfix, basta reescrever o método setInProgress dentro da própria classe.

*Quanto menor for a classe base, menor a chance de quebrar esse princípio.*
















