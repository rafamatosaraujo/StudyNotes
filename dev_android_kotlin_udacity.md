# Desenvolvendo aplicativos Android com Kotlin

### Resource: https://www.udacity.com/course/developing-android-apps-with-kotlin--ud9012

***

#### Anatomia básica de um app

1 - Arquivos `kotlin` contendo a lógica do aplicativo

2 - Uma pasta de `resources` para conteúdo estático como imagens, icones e etc.

3 - Um arquivo de `manifesto` que define os detalhes básicos do aplicativo para que o sistema operacional possa iniciá-lo

4 - Scripts `Gradle` para buildar e rodar o aplicativo

***

#### Activity e Layout

`Activity` -> Uma classe core do Android responsável tanto pelo desenho de uma interface quanto pela lógica que define a forma como o usuário interage com essa interface. Em resumo, uma activity descreve o que o App faz.

```kotlin
class MainActivityt: AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

Geralmente, ao criar uma Activity em Android estende-se a subclasse `AppCompatActivity` para obter acesso a funcionalidades mais atuais do Android e ao mesmo tempo dar suporte à versões anteriores.

Diferentemente de outras lingagens, não é necessário definir a função `main` para definir o ponto inicial da aplicação. Essa definição é feita diretamente no arquivo de `manifesto`.

`Layout` -> Cada activity possui um arquivo de layout associado. Esse arquivo é um XML e define a apresentação do App, ou seja, a parte gráfica.

```kotlin
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello Android!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

Os elementos que compõem um layout são chamados de `views`. Ex: (TextView, Button, etc.).

A conexão entre activies e layouts é feita através de um processo chamado `layout inflation`. Esse processo interpreta o XML e cria um objeto em memória para cada view definido no layout, e os organiza como uma árvore de hierarquia.

![Diagrama de árvore](/Images/Android_kotlin/tree_diagram.png)

A activity terá acesso à todos os objetos do layout, de uma única vez, logo é interessante identificar cada elemento com um `id` único no XML. Ao fazer isso o Android Studio irá criar automaticamente uma constante para cada `id`, a fim de facilitar a identificação dos itens do layout na activity. 

![Conexão entre XML e activity através da classe R](/Images/Android_kotlin/connection_xml_activity.png)

***

#### Adicionando elementos à tela e definindo comportamentos

No exemplo abaixo será criado um aplicativo simples, mostrando um texto com um número de 1 a 6 e um botão para alterar o valor desse texto, imitando o comportamento de um dado.

![Funcionamento do app de dado com números](/Images/Android_kotlin/dice_number.gif)

Começando pelo layout, inicialmente temos o seguinte código:

```kotlin
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

Primeiramente altere o valor da propriedade `id` do elemento `TextView` para "@+id/valor". Em seguida altere o valor da propriedade `text` "1", a fim de representar o valor inicial do dado.

```kotlin
<TextView
        android:id="@+id/valor"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
```

Agora adicione um elemento `Button` logo abaixo do elemento `TextView`, conforme o código abaixo. Note que o `id` do botão foi definido como "@+id/jogar".

```kotlin
<Button
        android:id="@+id/jogar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/valor" />
```

**Obs: Não precisa se preocupar com as outras propriedades do layout nesse momento**

O resultado final do XML ficará assim:

```kotlin
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/valor"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="1"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/jogar"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/valor" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

Agora é necessário conecetar o layout com a activity e definir o comportamento de seus elementos. Abaixo segue o código inicial da activity.

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

O método  `setContentView(R.layout.activity_main)` é o responsável por definir o layout que está conectado à activity, mas ainda é necessário conectar seus elementos. Adicione o código abaixo, logo abaixo do método `setContetView`.

```kotlin
val valor = findViewById<TextView>(R.id.valor)
val jogar = findViewById<Button>(R.id.jogar)
```

Esse código é responsável por indentificar o elemento TextView e Button adicionados ao layout. Note que eles são identificados pelo id que foi definido no passo anterior.

Agora é necessário adicionar um comportamento ao aplicativo para quando o o usuário clicar no botão. Para isso será utilizado o método `setOnClickListener` do elemento `Button`. Mas antes crie um funcção para retornar um número aleatório de 1 a 6.

```kotlin
private fun jogarDado(): Int {
    return Random().nextInt(6) + 1
}
```

Em seguida defina o método `setOnClickListener` do elemento `Button` para alterar o atributo `text` do elemento `TextView`.

```kotlin
jogar.setOnClickListener {
    valor.text = jogarDado().toString()
}
```

O resultado final será o seguinte:

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val valor = findViewById<TextView>(R.id.valor)
        val jogar = findViewById<Button>(R.id.jogar)

        jogar.setOnClickListener {
            valor.text = jogarDado().toString()
        }
    }

    private fun jogarDado(): Int {
        return Random().nextInt(6) + 1
    }
}
```
***

#### Adicionando imagens

Seguindo no mesmo exemplo acima, é hora de deixar o app um pouco mais legal e trocar o número por imagens reais. No exemplo do curso, utiliza-se imagens das faces dos dados, mas pode ser qualquer uma para fins de aprendizado.

Adicionar imagens ao projeto é tarefa bem simples, basta arrastar e soltar as imagens que deseja adicionar ao projeto dentro da pasta `drawable`, só se atenten para fazer dentro da estrutura de `Project` e não `Android`

![Local onde salvar as imagens](/Images/Android_kotlin/pastas.png)

Agora é preciso substituir o elemento `TextView` do layout pelo elemento `ImageView`, e fazer as devidas alterações na Activity. Para facilitar, defina o `id` do ImageView com "valor", igual ao TextView.

```kotlin
<ImageView
        android:id="@+id/valor"
        android:src="@drawable/dice_1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>
```

Note que o atributo responsável por identificar a imagens é o `src`.

Agora é necessário alterar a activity em dois pontos:

1 - Alterar o tipo da variável valor para ImageView

```kotlin
val valor = findViewById<ImageView>(R.id.valor)
```

2 - Alterar o algoriítimo do clickListener do botão para alterar a imagem de acordo com o valor retornado pelo método jogarDado

```kotlin
jogar.setOnClickListener {
    val randomInt = jogarDado()

    val resource = when(randomInt) {
        1 -> R.drawable.dice_1
        2 -> R.drawable.dice_2
        3 -> R.drawable.dice_3
        4 -> R.drawable.dice_4
        5 -> R.drawable.dice_5
        else -> R.drawable.dice_6
    }

    valor.setImageResource(resource)
}
```
A fim de deixar o código da activity um pouco mais clean, optei por separar em métodos diferentes a configuração das views e dos listener, e chamar esses métodos dentro do `onCreate`. Abaixo segue o código completo.

```kotlin
class MainActivity : AppCompatActivity() {

    lateinit var valor: ImageView
    lateinit var jogar: Button

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        setupView()
        setupListeners()
    }

    private fun setupView() {
        valor = findViewById(R.id.valor)
        jogar = findViewById(R.id.jogar)
    }

    private fun setupListeners() {
        jogar.setOnClickListener {
            jogarDado()
        }
    }

    private fun jogarDado() {
        val randomInt = Random().nextInt(6) + 1

        val resource = when(randomInt) {
            1 -> R.drawable.dice_1
            2 -> R.drawable.dice_2
            3 -> R.drawable.dice_3
            4 -> R.drawable.dice_4
            5 -> R.drawable.dice_5
            else -> R.drawable.dice_6
        }

        valor.setImageResource(resource)
    }
}

```

#### Um pouco sobre Gradle

Gradle é a ferramenta de build do Android. Algumas das tarefas executadas por ele são:

* Descreve quais dispositivos podem rodar o app
* Compila o código e os resources em uma APK (Android Application Package)
* Gerencias as dependências do projeto
* Roda testes automatizados

Ao desenvolver um projeto, não se define cada um dos devices que o aplicativo irá funcionar. Define-se, dentro do `Gradle` uma API mínima ao qual o app dará suporte. O valor dessa API é que irá ser responsável por determinar se um aparelho pode ou não utilizar o app.

Por exemplo, se eu definir que a API mínima do meu projeto é 19, todos os aparelhos que estiverem com o Android maior ou igual a 19 poderão rodar o app.

![Definindo API mínima de suporte](/Images/Android_kotlin/sdk.png)
  
