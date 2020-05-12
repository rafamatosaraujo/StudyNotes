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

![UML do padrão Builder](/Images/Design_patterns/builder.png)

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

***

#### Chain of Responsability

Chain of Responsibility é um padrão GOF cuja principal função é evitar a dependência entre um objeto receptor e um objeto solicitante. Consiste em uma série de objetos receptores e de objetos de solicitação, onde cada objeto de solicitação possui uma lógica interna que separa quais são tipos de objetos receptores podem ser manipulados. O restante é passado para o próximo objetos de solicitação da cadeia.

* O solicitante só conhece o próximo receptor da cadeia
* Cada receptor só tem conhecimento sobre o próximo recepetor da cadeia
* Os receptores podem receber a menssagem e processá-la ou mantê-la na cadeia
* O solicitante não sabe qual receptor irá processar a menssagem
* O primeiro receptor a processar a menssagem finaliza a cadeia
* A ordem dos receptores importa

![UML do padrão Chain of Responisbility](/Images/Design_patterns/chain_of_resp.jpg)

**Quando utilizar**

* Quando houver mais de uma receptor capaz de executar uma solicitação
* O receptor adequado não é explicitamente conhecimento pelo solicitante
* Os receptores podem ser definidos dinamicamente

***

#### Command

 O command é um padrão de projeto no qual um objeto é usado para encapsular toda informação necessária para executar uma ação ou acionar um evento em um momento posterior.

 ![UML do padrão Command](/Images/Design_patterns/command.png)

 Algumas aplicabilidades desse padrão são:
 
 * Logging
 * Validação
 * Desfazer (Undo)

Consequencias:

* Cada comando deve valer por si só (o cliente não deve passar nenhum argumento)
* Deve ser fácil de adicionar novos comandos (Princípio OCP)

Quando considerar esse padrão?

* Quando deseja-se desacoplar o cliente que executa um comnado da lógica e dependências desse comando
* Aplicação de linha de comando
* Validação
* Desfazer

***

#### Composite

Entende-se por Composite um padrão de projeto de software utilizado para representar um objeto formado pela composição de objetos similares. Este conjunto de objetos pressupõe uma mesma hierarquia de classes a que ele pertence. Tal padrão é, normalmente, utilizado para representar listas recorrentes - ou recursivas - de elementos. Além disso, este modo de representação hierárquica de classes permite que os elementos contidos em um objeto composto sejam tratados como se fossem um objeto único. Desta forma, os métodos comuns às classes podem ser aplicados, também, ao conjunto agrupado no objeto composto.

Um exemplo prático seria a tarerfa de enviar e-mail.
Pode-se enviar o mesmo e-mail para várias pessoas diferentes, uma a uma (sem o padrão Composite), ou pode-se criar um grupo com todas as pessoas, e enviar um e-mail para o grupo (com o padrão Composite).

![UML do padrão Composite](/Images/Design_patterns/composite.png)

Esse padrão pode ser usado quando há situações com:

* Grupos e coleções
* Distribuição
* Estrutura de árvore
  
***

#### Decorator

O Decorator surgiu da necessidade de adicionar um comportamento, funcionalidade ou estado extra a um objeto em tempo de execução, por exemplo quando Herança não é concebível por ser um caso que geraria um número muito alto de sub-classes.

![UML do padrão Decorator](/Images/Design_patterns/decorator.png)

Objetivos do padrão Decorator:

* Adicionar funcionalidade a objetos existenes em tempo de execução
* Alternativa a subclasses
* Design flexível
* Suportar o OCP

Consequências de utilizar o padrão Decorator:

* O objeto original não conhece os decorators
* Não existe um classe gigante contendo todas as features necessárias
* Os decorators podem ser implementados juntos, em uma forma de mix, para atender determinado requisito
* Pode aumentar a complexidade do código

