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

Primeiro defina o contrato da classe de dados:

```kotlin
data class MyName(var firstName: String = "", var secondName: String = "")
```

Em seguida, no arquivo de layout, adicione a tag `data` encapsulando a tag `variable`. E defina o valor da propriedade de `text` dos TextView.

```kotlin
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
```

Por fim, na activity, instancie um objeto `MyName` e defina o valor de `firstName` e `secondName`. Dentro do método `onCreate` defina o valor da váriavel `myName` criada no layout.

```kotlin
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

***

#### Constraint Layout

`Constraint` -> Uma conexão ou alinhamento, relativo a outro elemento da UI, ao layout pai, ou a uma guideline invisível.

Vantagens do Constraint Layout

* Responsivo
* Geralmente a hierarquia de views é mais simples
* Otimizado para organizar as views
  
`Proporção (Ratio)` -> É possível determinar a dimensão de elementos através de uma proporção. Para fazer isso define-se um dos atributos de dimensão (width ou height) com o valor desejado, e outros como 0dp.

Também é necessário definir a proporção do elemento. Nesse caso utiliza-se o atributo `layout_constraintDimesionRatio`.

```kotlin
<Button
    android:layout_width="wrap_content"
    android:layout_height="0dp"
    android:layout_constraintDimesionRatio="1:1"
    ...
    />
```
Nesse caso o elemento terá a forma de uma quadrado, com a proporção de 1:1, ou seja, o valor do atributo `height` será exatamente igual ao valor do atributo `width`.

***

### Navigation

#### Fragments

Os fragments foram introduzidos no android 3.0, basicamente para dar suporte a dispositivos com tela maior, como tablets. Neste caso, a `activity` atuava como um frame, que poderia conter um ou mais `fragments`, cada um com seu respectivo layout. É uma ideia bem parecida com a das `divs` do HTML.

![Conceito de fragment e activity](/Images/Android_kotlin/fragments.png)

Os fragments funcionam como uma view comum, mas devem ser criados em classes separadas da activity.

No contexto de navagação, quando se navega de uma activity para outra, a activity anterior fica salva em uma pilha ordenada da aplicação. O mesmo acontece quando se navega de um fragment para o outro, a diferença é que o gerenciamento dessa pilha é feito por uma classe especial chamada `FragmentManager`.

#### Pricípios da navegação

1 - Sempre existe um ponto inicial
2 - Você sempre pode voltar (Last In First Out)
3 - O botão `up` (seta de voltar no topo dos apps) sempre retorna para o destino anterior.

#### Intents e compartilhamento

As `intenets` representam uma intenção do app, e podem ser implicitas ou explicitas.

`Intent explicita` -> Carrega uma activity utilizando a referencia exata da mesma.

`Intent implicita` -> Define o que se deseja fazer, e deixa que o sistema decida qual activity chamar. Exemplo: Geralmente os botões de share utilização intents implicitas dizendo que querem compartilhar dados, e o sistema opoeracional define quais apps possuem essa funcionalidade.

Todas as intents implicitas devem possuir uma `intent action`, que é responsável por descrever o tipo de ação que deve ser executada (ACTION_VIEW, ACTION_DIAL, ACTION_EDIT ... ). 

Junto com a intent action deve haver uma `intent category`, que nada mais é do que um subtipo da ação (CATEGORY_APP_MUSIC, CATEGORY_APP_MAPS ... ).

Por fim, as intents implicitas também podem definir um tipo de dado, como texto por exemplo.

Para serem inicializadas, todas as activies devem ser declaradas no arquivo `manifest.xml`. 

```xml
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <activity android:name=".ui.ASActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
</application>
```

No caso de uma activity comum, ela pode ser declarada apenas com a tag `activity`, porém caso seja um activity que aceite algum tipo de intent implicita, ela deve declarar também a tag `intent-filer`.

No exemplo acima, a activity `ASActivity` possui um intent filter da ação `MAIN` e da categoria `LAUNCHER`.

***

### Ciclo de Vida de Activites e Fragments

Cada activity ou fragment possui seu próprio ciclo de vida. Esse ciclo de vida é composto pelos diferentes estados em que a activity passa do momento em que é inicializada ao momento em que é destruída.

![Digrama do ciclo de vida de uma activity](/Images/Android_kotlin/activity_lifecycle.png)

#### Estados do ciclo de vida de uma actitity:

* Created - Activity acabou de ser criada, mas não está visível e não possui foco.
* Started - Activity está visível mas não possui foco.
* Resumed - Activity está rodando, está visível e possui foco.
* Destroied - Activity foi destruída por completo.
* Initialized - Estado para toda vez que uma activity for criada. É um estado transitório, automáticamente vai para `created`.

#### Métodos de callbacks do ciclo de vida da activity:

* onCreate - Chamado quando a activity foi criada, mas ainda não está visível para o usuário
* onStart - Chamado quando a activity se torna visível para o usuário
* onResume - Chamado quando a activity tem foco (usuário pode interagir com a activity)
* onDestroy - Chamado quando a activity é finalizada por completo
* onPause - Chamado quando a activity perde o foco
* onStop - Chamado quando a activity é retirada da tela do usuário
* onRestart - Bem parecido com o `onCreate`. A diferença é que o onCreate é chamado na primiera inicialização da activity, enquando o onRestart é chamado quando a activity já foi inicializada.

![Digrama do ciclo de vida de um fragment](/Images/Android_kotlin/fragment_lifecycle.png)

O ciclo de vida do fragment é bem parecido com o da activity, com algumas diferenças. Logo, apenas os métodos diferentes serão abordados.

#### Métodos de callbacks do ciclo de vida do fragment:

* onCreateView - Chamado entre os métodos `onCreate` e `onActivityCreated`. Quando o sistema irá renderizar o fragment pela primeira vez ou quando o fragment irá se tornar visível novamente para o usuário.
* onAttach - Chamado quando o fragment é anexado pela primeira vez na activity.
* onActivityCreated - Chamado quando o método `onCreate` da activity retorna e a mesma tenha sido inicializada. O objetivo desse método é conter qualquer código que necessite de uma instância da activity. Ele é chamado várias vezes durante o ciclo de vida de um fragment.
* onDestroyView - Chamado quando o método `onDestroy` da activity é chamado
* onDetach - Chamado quando a associação entre o fragment e activity é destruída

#### Links úteis

[The Android Lifecycle cheat sheet — part I: Single Activity](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-i-single-activities-e49fd3d202ab)

[The Android Lifecycle cheat sheet — part II: Multiple Activities](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-ii-multiple-activities-a411fd139f24)

[The Android Lifecycle cheat sheet — part III: Fragments](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-iii-fragments-afc87d4f37fd)

#### Lifecycle Library

Bibloteca criada pelo Google e disponibilizada em 2017 para ajudar a trabalhar com o ciclo de vida das activities e dos fragments sem a necessidade de utilizar os métodos de callback.

```kotlin
class MainACtivity: AppCompatActivity() {
    // resto da classe
    fun onButtonClick() {
        if (this.lifecycle.currentState.isAtLeast(Lifecycle.State.STARTED) {
            // fazer algo
        })
    }
}
```
Além disso, essa biblioteca disponibiliza duas interfaces:

* LifecycleOwner - um classe que possui um ciclo de vida (activity ou fragment)
* LifecycleObserver - Permite que objetos observem o ciclo de vida dos `LifecycleOwners`

Abaixo segue um exemplo de implememntação:

```kotlin
class DessertTimer(lifecycle: Lifecycle) : LifecycleObserver {
    init {
        lifecycle.addObserver(this)
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun startTimer() {...}

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun stopTimer() {...}
}

class MainACtivity: AppCompatActivity() {
    private lateinit var dessertTimer: DessertTimer

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        dessertTimer = DessertTimer(this.lifecycle)
    }
}
```

***

### Arquitetura  - Camada de UI

A arquitetura de uma aplicação é a forma como se projeta as classes da aplicação, assim como a forma como essas classes se relacionam. A arquitetura de um app depende muito do contexto em que este está inserido assim como de suas necessidades.

![Exemplo de arquiteutra](/Images/Android_kotlin/ui_achiteure_1.0.png)

**Link útil:** [Android Architeures Blueprint](https://github.com/android/architecture-samples)

**Arquiteura que será abordada no curso - MVVM**

![Exemplo de arquiteutra MVVM](/Images/Android_kotlin/ui_achiteure_1.1.png)

#### Separação de responsabilidades

Divida o código em classes separadas, cada qual com sua responsabilidade bem definida. No caso da arquitetura MVVP, trabalha-se com 3 tipos diferentes de classes na camada de UI.

* UI Controller (Activity ou Fragment) - Responsável pelas tarefas relacionadas à interface do usuário, como por exemplo mostrar ou esconder um botão.
* ViewModel - Guardar toda informação necessária pela interface do usuário e prepará-la para apresentação. Além disso, a ViewModel também pode fazer cálculos e transformações simples com tais informações.
* LiveData - As instâncias dessa classe estão contidas na ViewModel. São cruciais para comunicação entre a ViewModel e a UI Controller.

#### ViewModel

Classe abstrata que guarda as infromações de UI do app. Sobrevive à mudanças de configuração.

Sem a classe ViewModel, as informações que são exibidas na interface do usuário ficam salvas no próprio fragment ou activity. Dessa forma, sempre que há uma mudança de configuração no app (por exemplo, ao rotacionar o telefone), o fragment ou activity é destruído e reconstruído, ou seja, se as infromações de UI não foram salvas antes dessa mudanças, elas se perderam.

![Exemplo de arquiteutra sem ViewModel](/Images/Android_kotlin/ui_achiteure_1.2.png)

Ao utilizar a classe ViewModel e apenas referênciá-la na fragment ou activity, quando há uma mudança de configuração os dados não são perdidos, uma vez que a ViewModel não é afetada. Dessa forma, basta reconectar o novo fragment ou activity ao ViewModel que os dados estarão disponíves.

![Exemplo de arquiteutra com ViewModel](/Images/Android_kotlin/ui_achiteure_1.3.png)

Para utilizar as classes ViewModel e LiveData, é necessário importar as seguintes dependências no arquivo do gradle e seguir os seguintes passos:

```kotlin
// Gradle
implementation "androidx.lifecycle:lifecycle-extensions:$lifecycle_version"

// Classe
class AppViewModel: ViewModel()

// Fragment
private lateinit var viewModel = AppViewModel

// Dentro do método onCreateView
viewModel = ViewModelProviders.of(this).get(AppViewModel::class.java) 
```

**Benefícios de uma boa arquitetura**

* Código mais organizado
* Mais fácil de debugar
* Menos problemas com ciclo de vida
* Aplicação modular
* Mais fácil de testar

#### LiveData

Para que essa arquitetura funcione através de um fluxo unidirecional é necessário fazer com que o ViewModel posssa se comunicar de alguma forma com o UI controller para avisá-lo sobre mudanças em quaisquer dods dados, sem que o ViewModel tenha alguma referência de Views.

Para atingir esse objetivo utiliza-se a class LiveData, que é uma classe observável e também tem conhecimento sobre o ciclo de vida.

![LiveData e observer pattetrn](/Images/Android_kotlin/ui_achiteure_1.4.png)

Por exemplo: 

* Imagine um app que apresenta o número 1 na interface do usuário.
* Toda vez que o usuário toca na tela, soma-se mais 1 a esse número.

No exemplo acima. O valor que será exibido na interface do usuário fica salvo na `ViewModel` como uma instância da clases `MutableLiveData`.

O fragment, ou activity, vai observar essa instância, e sempre que o valor desta for alterado, a mudança é automaticamente refletida na UI controller.

```kotlin
// Ui Controller

viewModel.number.observe(this, Observer { newNumber ->
    ...
})
```

![Arquitetura Ui Controller + ViewModel](/Images/Android_kotlin/ui_achiteure_1.5.png)

