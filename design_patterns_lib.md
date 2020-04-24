# Biblioteca de padrões de projeto

### Resource: https://app.pluralsight.com/library/courses/patterns-library/table-of-contents

***

Padrões de projeto são soluções gerais e reutilizáveis para problemas comuns em desenvolvimento de software, cada um com um nome específico para facilitar sua identificação.

É importante ressaltar que os padrões de projetos não são soluções finanis, mas sim um template para resolver certos problemas. Isso não quer dizer que esses problemas não podem ser resolvidos de outra maneira.

Padrões de projeto se preocupam com:

* Arquitetura de software
* Abstrações
* Relação entre classes
* Problemas que já foram resolvidos

Padrões de projeto **NÃO** se preocupam com:

* Algorítmos
* Implementações específicas de classes

Classificação dos padrões de projeto:

* Criacional
* Estrutural
* Comportamental
* Segurança
* Concorrência
* SQL
* Interface de usuário
* Relacional
* Social
* Distribuído

Porque os padrões de projeto são importantes:

* Ajuda a não ficar "reinventando a roda"
* Fornece um ponto de partida para uma solução
* Pode aumentar eficiência de um time
* Geralmente melhora a arquitetura de um software

***

#### Adapter

Motivações para utilizar o padrão Adapter:

* Utilizar uma classe que não implementa a interface necessária;
* Desenvolver uma classe, ou framework, garantindo que ele possa ser utilizado por uma grande variedade de classes / aplicações ainda não desenvolvidas.

![Imagem exemplificando o padrão adapter](/Images/Design_patterns/adapter_motivation.jpeg)

Objetivos do padrão adapter:

* Converter a interface de uma classe em uma interface esperada por determinado cliente
* Permitir com que classes trabalhem juntas, mesmo que possuam interfaces incompatíveis

![Imagem exemplificando o padrão adapter](/Images/Design_patterns/adapter_strucutre.png)

Como utilizar o padrão Adapter:

* Clientes dependem da interface do Adapter ao invés de depender da implementação concreta Adapter
* Pelo menos uma classe concreta deve ser criada implementando a interface do adapter para que o cliente posssa utilizá-la
* Caso haja a necessidade de utilizar novas classes concretas, cria-se essas classes implementando a interface do adapter

`É uma forma efetiva de garantir o princípio Aberto / Fechado (OCP - Open Closed Principle)`

Padrões de projeto relacionados ao padrão Adapter:

* Repository
* Strategy
* Facade

***

#### Bridge

Definição de acordo com o GOF - "*Desacoplar uma abstração de sua implementação para que ambas possam variar de forma independente.*"

Em uma liguagem mais simples, ele diz que para que uma abstração e uma implementação dessa abstração possam variar de forma independente, deve-se desacoplá-las. A fim de atingir esse objetivo, cria-se mais uma camada de abstração entre estes dois itens, para trabalhar como uma ponte entre eles.

![UML do padrão Bridge](/Images/Design_patterns/bridge.png)

***

#### Builder

Definição do GOF - *Separar a construção de um objeto complexo da sua representação para que o mesmo processo possa criar diferentes representações.*

Nessa definição, a construção é uma `processo` e a representação são os `dados`. Então basicamente, pretende-se separar a lógica para trabalhar com dados diferentes.

**Quando utilizar?**

* Construtores com muitos parâmetros
* A ordem dos parâmetros influência na construção do objeto


![UML do padrão Bridge](/Images/Design_patterns/builder.png)

**Director**

* Usa o `Concrete builder`
* Sabe como construir o objeto (lógica)
* Código do `Client` chama o `Director` diretamente

**Builder**

* Classe abstrata ou interface
* Define os passos que o `Director` vai serguir
* Possui a instância do `Product`

**Concrete builder**

* Pode haver mais de um
* Fornece a implementação da interface definida pelo `Builder`
* É uma receita

**Product**

* O que está sendo consutrído
* Não um tipo dieferente, mas um objeto com dados diferentes

