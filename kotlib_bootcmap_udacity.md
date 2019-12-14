# Bootcamp Kotlin para programadores

### Resource: https://www.udacity.com/course/kotlin-bootcamp-for-programmers--ud9011

<hr/>

`Boxing` -> Tipos primitivos no kotlin são "encapsulados" em "objetos", portanto é possível chamar métodos diretamente de um tipo primitivo.

#### Exemplo

```kotlin
// Caminho longo
var number: Int = 1
number.toLong()

// Caminho curto
1.toLong()
```` 

<hr/>

### Definindo variáveis

- `val` -> Constante
- `var` -> Variável

### Definindo null como valor de variável

Se o tipo da variável for explicitamente definido, não é possível passar `null` como valor.

```kotlin
var number: Int = null //Não funciona
```

Nesse caso é necessário garantir que a variável possa receber `null` como valor de forma explicita, utilizando a interrogação no final da tipagem.

```kotlin
var number: Int? = null //Funciona
```

Ao acionar um método em uma varíavel que pode receber `null` como resultado, pode se utilizar uma sintaxe especial (elvis operator) para fazer a varificação do valor.

#### Exemplo

```kotlin
// Utilizando if
val l: Int = if (b != null) b.length else -1

// Utilizando elvis operator
val l = b?.length ?: -1
```

Em ambos os casos o resultado será o mesmo. Se o valor da variável `b` for `null`, a variável `l` assumirá -1 como valor, caso contrário `b.length`.

<hr/>

### String template

```kotlin
val a = "Rafael"
val b = "kotlin"

return "$a está escrevendo sobre $b"
// Rafael está escrevendo sobre kotlin
```

<hr/>

### Condicionais

#### if

```kotlin
val number = 50
if (number in 1..100) return true
// true
```

### when

```kotlin
val number = 50
when(number) {
    0 -> println("vazio")
    50 -> println("meio cheio")
    100 -> println("cheio")
}
// meio cheio
```

<hr/>

### Arrays

- Se um array é definido como `val` não é possível redefiní-lo novamente, mas é possível manipulá-lo através de seus métodos.
- Ao definir um array de inteiros com `intArrayOf()` só é possível adicionar inteiros no array, porém se o tipo não é explicito (`arrayOf()`), é possível misturar os tipos.

```kotlin
var array = Array(5) { it * 2 }
// [0, 2, 3, 6, 8]
```

<hr/>

### Loops

```kotlin
//#1
for (element in array) { ... }
//#2
for ((index, element) in array.withIndex) { ... }
```

### Loops com ranges

```kotlin
//#1
for (i in 'b'..'g') { ... }
//#2
for (i in 1..5) { ... }
//#3
for (i in 5 downTo 1) { ... }
//#4
for (i in 3..6 step 2) { ... }
```

<hr/>

### Funções

Todo programa em kotlin começa com a função `main`

```kotlin
fun main(args: Array<String>) { ... }
```

Em kotlin toda função tem um retorno, mesmo quando este não está explicito. Sempre que esse retorno não está definido (a exemplo da função `main`) será retornado um `Unit`.

Tudo em kotlin é uma expressão, sendo possível definir o valor de uma variável através de uma expressão.

```kotlin
val isHot = if (temp > 50) true else false
```

#### Sintaxe com rentorno explicito

```kotlin
fun teste(): String { ... }
// Retorna uma string
```

#### Funções com valores padrões

```kotlin
fun teste(arg: String = "olá") {
    println(arg)
}

teste() // Olá
teste("Tchau") // Tchau
```

É importante se atentar as boas práticas ao escrever funções co multiplos parâmetros em que um ou mais parâmetros possuem valor padrão. `Nesse caso os argumentos sem valor padrão devem ser definidos primeiro`.

```kotlin
fun teste(valor: Int,
        quantidade: Int = 1,
        dia: String = "Segunda-feira"
) {
    println("$dia vendi $quantidade por $valor reais")
}

teste(10) //Segunda-feira vendi 1 por 10 reais
teste(10,3) // Segunda-feira vendi 3 por 10 reais
teste(10,7,"Terça-feira") // Terça-feira vendi 7 por 10 reais
teste(10, dia="Quarta-feira") // Quarta-feira vendi 1 por 10 reais
```

Quando a função `when` é utilizada sem parâmetros ela trabalha como um `if statement`

```kotlin
fun teste(a: string, b: int = 22) {
    return when {
        a == "hoje" -> true
        b > 30 -> true
        else -> false
    }
}
```

Quando o corpo da função é composto por uma única expressão não é necessário declarar o tipo do retorno.

```kotlin
fun isHot(temp:Int) = temp > 30 // Retorno = boolean
```

O valor de um argumento padrão também pode ser uma função

```kotlin
fun teste(a: Int = getInt()) { ... }
```

É importante ressaltar que quanto maior for o custo de processar essa função, pior a performance do código.

Dê uma olhada na bibiloteca de funções padrões do kotlin, há bons recursos disponíveis na linguagem. Além do `when`, `if/else`, etc., há funções como `repeat` por exemplo que podem facilitar o dia a dia.

#### Filtros

**OBS:** bem parecido com `ES6`

```kotlin
val decora = listOf("pedra", "planta", "tapete", "quadro")
println(decora.filter { it[0] == 'p' })
// ["pedra", "planta"]
```

**OBS:** em kotlin, aspas simples signifcam `caractere`enquanto aspas duplas significam `string`

- 'p' -> caractere
- "p" -> string

#### Lambda

Uma expressão que cria uma função.

```kotlin
val teste = { arg: Int -> arg / 2 }
```

#### High Order Functions

Uma função que possui outra função como parâmetro. Lembrando que a função deve ser sempre o último argumento

```kotlin
fun teste(arg: Int, operation: (Int) -> Int) : Int {
    return operation(arg)
}

val valor = teste(10, { 10 -> 10 + 2 })
```

<hr/>

### Orientação a objetos com kotlin

Criando classes

```kotlin
class MinhaClasse { ... }

// Inicializando variáveis no contrutor
class MinhaClasse(val a: Boolean = true) { ... }
```

Não é necessário usar o `new` para instanciar uma classe

```kotlin
val teste = MinhaClasse()
```

É possível criar getters e setters de forma bem rapida apenas definindo os métodos abaixo da declaração da variavel.

```kotlin
val area: Int
    get() = altura * largura
    set(valor)

    //Pode definir uma expressão para o setter também
    set(valor) { ... }
```

#### Visibilidade

- `public`-> Padrão
- `private` -> Apenas arquivo
- `internal` -> Apenas módulo

Além de ser possível inicializar variáveis diretamente no construtor, é possível utilizar o método `init` para a mesma finalidade.

```kotlin
class Peixe(val grande: Boolean = true, volume: Int) {
    val size: Int

    init {
        if (a) size = volume
        else size = volume *2
    }
}
```

Apesar de ser possível utilizar vários construtores e inits na mesma classe, isso não é uma boa prática. Caso seja necessário outro construtor, considere criar um método helper em forma de API.

Para que uma classe possa ser extendida, é necessario que essa seja declaraa como `open`.

```kotlin
open class teste { ... }
```

**Obs:** Toda classe em kotlin é uma extensão da classe superir `Any`

#### Herança

```kotlin
open class A(open var b: Int = 0) {
    open var count: Int = b * 2
}

class B(): A() {
    override count = b * 3
}
```

Variáveis e metódos só estarão dispoiveis para a subclasse se forem definidos como `open`.

#### Interfaces e Classes abstratas

- Interfaces e classes abstratas não podem ser instanciadas diretamente
- Classes abstratas possuem construtores, interfaces não

```kotlin
abstract class Peixe { ... }

interface AcaoDoPeixe {
    fun comer()
}

class Tubarao: Peixe(), AcaoDoPeixe {
    override fun comer() { ... }
}
```

- Utilize interfaces quando houver muitos métodos mas poucos possuem uma implementação padrão
- Utilize classes abstratas sempre que não for capaz de completar uma classe

#### Classes de dados

São utilizadas como container de dados. Ao avaliar o valor de uma classe de dados, será avaliado o valor de suas variáveis e não de sua instância.

```kotlin
data class Time(val time: String) { ... }
val a = Time("Cruzeiro")
println(a)

// (time = Cruzeiro)
```

#### Singleton

Só pode haver uma instância em todo projeto. Utilize com cautela.

```kotlin
object MeuSingleton { ... }
```

#### Enum

``` kotlin
enum class Color(val rgb: Int) {
    RED("0xFF0000")
}
```

#### Classes seladas

Pode conter subclasses, porém somente no mesmo arquivo em que ela for declarada. Normalmente são utilizadas para retornar o sucesso ou erro de uma API.

```kotlin
sealed class Response

class Sucesso: Response()
class Erro: Response()

fun verifica(r: Response): Boolean {
    return when(r) {
        is Sucesso -> true
        is Erro -> false
    }
}
```

<hr/>

### Pares

Pares são tipos de dados em kotlin que permitem um par genérico de valores.

```kotlin
val equipamento = "tesoura" to "cortar papel"

println(equipamento.first) // tesoura
println(equipamento.second) // cortar papel

//Outra possibilidade
val equipamento = ("tesoura" to "cotar papel") to ("de tamanho grande" to "e folha dupla")
```

Também é possível "destruir" um par e passar seus valores para variáveis distintas.

```kotlin
val equipamento = "tesoura" to "cortar papel"
val (ferramenta, uso) = equipamento

println(ferramenta) // tesoura
println(uso) // cortar papel
```

Retorno de função.

```kotlin
fun giveMeATool(): Pair<String, String> {
    return ("tesoura" to "cortar papel")
}

val (ferramenta, uso) = giveMeATool()
```

<hr/>

### Listas

Revertendo uma lista

```kotlin
val testList = listOf(11,12,13,14,15,16)

// Forma normalmente utilizada
fun reverseList(list: List<Int>): List<Int> {
    val result = mutableListOf<Int>()
    for (i in 0..list.size - 1) {
        result.add(list[list.size-i-1])
    }
    return result
}

// Simplificando com kotlin
fun reverseList(list: List<Int>): List<Int> {
    val result = mutableListOf<Int>()
    for (i in list.size - 1 downTo 0) {
        result.add(list.get(i))
    }
    return result
}

// Simplificando ainda mais com kotlin
testList.reversed() // retorna uma nova lista
```

- `listOf`-> Não pode ser modificado
- `mutableListOf` - Pode ser modificado

Outros métodos pré definidos

```kotlin
val caracteristicas = mutableListOf("tímidos", "cabelo vermelho", "gosta de dormir", "rápido", "inteligênte")

//Adicionar ou remover elementos
caracteristicas.add("teimoso")
caracteristicas.remove("gordo")

//Verificação
caracteristicas.contains("vermelho") // false
caracteristicas.contains("cabelo vermelho") // true

//Outros
println(caracteristicas.subList(4, caracteristicas.size)) // [rápido, inteligênte]
listOf(1,2,3).sum() // 6
listOf("a", "b", "cc").sumBy { it.length } // 4
```

Em kotlin é possível mapear quase tudo com `mapOf`. Para um map modificável utilize `mutableMapOf`.

```kotlin
val curas = mapOf("aspirina" to "qualquer doença", "salompas" to "dores musculares")

println(curas.get("aspirina"))
println(curas["aspirina"])

println(curas.getDefault("água", "não conheço"))
// Segundo elemento é a resposta padrão para quando o elemento procurado não é encontrado

curas.getOrElse("água") { "não conheço" }
```

<hr/>

### Constantes

```kotlin
val teste = 2
const val testeReal = 3
```

Se `val` representa um valor constate, qual a diferença de usar `const val`?

O valor de `const val` é sempre determinado no momento da compilação. Já o valor de `val` pode ser determinado durante a execução do programa. Isso significa que não é possível atribuir o retorno de uma função como valor de `const val`.

`Const val` não pode ser utilizado em qualquer classe. Apenas aquelas declaradas como `object`. Para utilizar uma constante em uma classe regular, é necessário adicionar um `companion object`.

```kotlin
class MinhaClasse {
    companion object {
        const val cor = "VERMELHO"
    }
}
```

`companioon object` são inicializados no contrutor da classe. Logo só são criados quando a classe é instânciada.

