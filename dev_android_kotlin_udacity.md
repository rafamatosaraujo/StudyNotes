# Desenvolvendo aplicativos Android com Kotlin

### Resource: https://www.udacity.com/course/developing-android-apps-with-kotlin--ud9012

***

### Introdução

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

#### Um pouco sobre Gradle

Gradle é a ferramenta de build do Android. Algumas das tarefas executadas por ele são:

* Descreve quais dispositivos podem rodar o app
* Compila o código e os resources em uma APK (Android Application Package)
* Gerencias as dependências do projeto
* Roda testes automatizados

Ao desenvolver um projeto, não se define cada um dos devices que o aplicativo irá funcionar. Define-se, dentro do `Gradle` uma API mínima ao qual o app dará suporte. O valor dessa API é que irá ser responsável por determinar se um aparelho pode ou não utilizar o app.

Por exemplo, se eu definir que a API mínima do meu projeto é 19, todos os aparelhos que estiverem com o Android maior ou igual a 19 poderão rodar o app.

![Definindo API mínima de suporte](/Images/Android_kotlin/sdk.png)
  
***

### Layouts

#### Grupos de Views e Hierarquia das Views

Em Android, cada elemento visual (Botão, texto, etc.) que compõe uma tela é uma View. E todos esses elementos são filhos da classe `View`. Algumas propriedades são comuns a todas esses elementos:

* Width
* Height
* Background

![Árvore de views](/Images/Android_kotlin/view_hierarchy.png)

A unidade para definir a localização e a dimensão desses elementos em um layout é o `dp(Density Independent Pixel)`. Essa é uma unidade abstrata baseada desinsidade de pixels física de uma tela, ou seja, ela varia de aparelho para aparelho. Por exemplo:

* Tela de 160 dpi -> 1dp == 1px
* Tela de 480 dpi -> 1dp == 3px

Views cujo objetivo principal é envelopar outras tipos de views são chamados de `ViewGroup`. Exemplos de ViewGroup são:

* LinearLayout
* FrameLayout
* ConstraintLayout

#### Data binding

Para obter uma referência das views de um layout, a activity faz uso do método `findViewById`. Porém esse método é chamado em tempo de execução, ou seja, toda vez que esse método é chamado, o Android precisa pesquisar por toda hierarquia de views para encontrar a view desejada. Se o app tiver um layout muito complexo, isso pode deixar o app lento.

![Renderizando views sem databind](/Images/Android_kotlin/after_databind.png)

O databind permite que eum layout se conecta a uma activity em tempo de compilação. O compilador gera uma classe helper chamada `Binding` que pode ser acessada pela activity a fim de se conectar com o xML.

![Renderizando views com databind](/Images/Android_kotlin/before_databinding.png)

Para utilizar o data binding primeiro é preciso habilitá-lo no arquivo gradle(app).

```kotlin
android {

    ...

    dataBinding {
        enabled = true
    }
}
```

No arquivo XML(layout) em que será utilizado o data binding, é necessário evelopar todo o conteúdo em uma tag do tipo `layout`.

```kotlin
<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas/android.com/apk/res-auto">
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        ...

    </LinearLayout>

</layout>
```

Por último é necessário definir o objeto binding na activity.

```kotlin
class MainActivity: AppCompatActivity() {
    
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        //setContentView(R.layout.activity_main)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        //findViewbyId<Button>(R.id.done_button).setOnClickListener {
        //    ...
        //}
        binding.done_button.setOnClickListener {
            ...
        }

    }

    ...
}
```

Além de fazer o bind entre layout e activity, a técnia do data binding também tem o poder de fazer o bind de dados. Por exemplo, imagine uma classe de dados que representa o nome completo de uma pessoa. É possível fazer o bind dos dados dessa classe diretamente na activity e no layout.

```kotlin
//Classe de dados

data class MyName(var firstName: String = "", var secondName: String = "")

//=======================================================

//Layout (XML)

<layout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas/android.com/apk/res-auto">

    //Adicione a tag data junto com a tag variable
    <data>
        <variable
            name="myName"
            type="com.example.android.nomeDoProjeto.MyName" />
    </data>
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <TextView
            android:id="@+id/first_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textAlignment="center"
            //No campo text, defina a variável firstName
            android:text="@={myName.firstName}"/>

        <TextView
            android:id="@+id/second_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textAlignment="center"
            //No campo text, defina a variável secondName
            android:text="@={myName.secondName}"/>

    </LinearLayout>

</layout>

//=======================================================

//Activity

class MainActivity: AppCompatActivity() {
    
    private lateinit var binding: ActivityMainBinding
    //Crie uma instância de MyName
    private val myName: NyName = myName("Rafael", "Matos Araújo")

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        //Defina o valor da variável do layout (binding.myName) com o valor da variável instanciada acima (myName)
        binding.myName = myName

        binding.done_button.setOnClickListener {
            ...
        }

    }

    ...
}
```

