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
