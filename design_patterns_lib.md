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

***

#### Event Aggregator

Esse padrão busca simplificar o registro de eventos ao promover um local centralizado para armazená-los, reduzindo o acoplamento entre assinantes e classes publicadores, assim como a fricção pela introdução de novos eventos.

De uma forma mais simples o Event Aggregator é um padrão de projeto que visa desacoplar as classes assinantes das classes publicadoras, resolvendo além desse problema, a temida duplicação de código.

![UML do padrão Decorator](/Images/Design_patterns/event_agg.png)

Quando utilizar?

Esse padrão pode ser uma boa escolha quando se tem múltiplos objetos que podem ser fontes de eventos. Ao invés de ter um observer para cada um desses objetos, pode-se centralizar a lógica de registros no Event Aggregator.

* Telas complexas
* Muitos assinates e publicadores
* Muitos eventos
* Novos eventos adicionados com frequência

Como funciona o Event Aggregator?

* Cada assinante e publicador possui uma referencia do aggregator
* Publicadores chamam métodos de publicação para notificar os assinantes
* Assinantes chamam métodos de assinatura para receber notificações

***

#### Facade

Objetivos do padrão Facade:

* Fornecer uma interface simplificada para um corpo de código maior.
* Fazer com que uma API mais complexa seja mais fácil de ser utilizada.
* Expor um conjunto de interações complexas entre objetos atravpes de uma única interface.
* Encapsular uma API mal projetada em uma melhor.

Seu objetivo é implementar uma forma de interagir com um sistema que seja mais fácil do que a atual, com a intenção de usar um subconjunto do sistema em questão. Ou seja, busca simplificar o uso de um sistema existente a partir de uma interface própria definida.

![UML do padrão Facade](/Images/Design_patterns/facade.jpg)

Consequências da utilização:

* Simplificar o uso de APIs existentes
* O Facade deverá ser atualizado sempre que novas funcionalidades forem adicionadas ao sistema
* Muitas vezes não será possível replicar 100% da capacidade das APIs através do Facade

***

#### Factory

Motivações para o uso do padrão Factory:

* Inceerteza sobre qual implementação concreta de uma interface deve ser retornada
* Separar a criação da representação de um objeto
* Muitos if/else para decidir qual classe concreta criar
* Switch para definir qual classe concreta criar

Objetivos do padrão Factory:

* Separar a criação do objeto da decisão sobre qual objeto criar
* Adicionar novas classes e funcionalidades sem quebrar o OCP (Open-Closed Principle)
* Salvar qual tipo de objeto criar fora da aplicação principal

**Simple Factory**

* Encapsula a criação de objetos
* Permite configurações posteriores independete da instancia criada
* O cliente sabe qual factory concreta ele precisa

![UML do padrão Simple Factory](/Images/Design_patterns/simple_factory.png)

**Factory Method**

Define uma interface para a criação de um objeto, mas deixa com a subclasse a decisão sobre qual class instanciar.

![UML do padrão Factory Method](/Images/Design_patterns/factory_method.png)

Vantagens:

* Elimina referencias para classes concretas
* Facotries podem ser herdadas para promover maior especialização na criação de objetos
* Regras para a inicialização de objetos centralizadas

Desvantagens:

* Pode ser necessário criar uma factory apenas para se obter uma classe concreta
* A hierarquia de herança pode ficar complexa

**Abstract Factory**

Fornece uma interface para criar uma família de objetos relacionados ou dependentes sem especificar suas classes concretas.

![UML do padrão Factory Method](/Images/Design_patterns/abstract_factory.png)