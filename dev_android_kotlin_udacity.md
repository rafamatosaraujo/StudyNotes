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

