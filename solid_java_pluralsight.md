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

### Princípio da Responsabilidade Única

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

