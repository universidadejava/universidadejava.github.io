---
layout: article
title: "Android - Criando uma calculadora com TextView e ImageView"
categories: android
author: sakurai
date: 2014-03-14 15:12:00
tags: [android, textview, imageview]
published: true
excerpt: Vamos criar uma aplicação de Calculadora para o Android.
comments: true
image:
  teaser: 2013-03-14-teaser-android-calculadora-textview-imageview.png
ads: true
---

Vamos criar uma aplicação de Calculadora para o Android. Nesse exercício será abordado a criação de uma aplicação Android, utilizando uma Activity e os componentes TextView e ImageView. A aplicação ficará conforme a imagem a seguir:

<figure>
    <a href="/images/2013-03-14-android-calculadora-textview-imageview-01.png"><img src="/images/2013-03-14-android-calculadora-textview-imageview-01.png" alt="Aplicativo de Calculadora Android."></a>
</figure>

Para criar um projeto Android escolha no Eclipse a opção **File (Arquivo) → New (Novo) → Project... (Projeto...)**.

Na tela **New Project (Novo Projeto)** escolha no item **Android** o tipo de projeto **Android Project (Projeto Android)** e clique em **Next > (Próximo >)**.

Na tela New Android Project (Nova Projeto Android) configure:

* **Project name (Nome do projeto):** Calculadora;
* **Build Target (Alvo da construção):** Android 2.2 (API 8) - está é a versão da API do Android utilizada no projeto;
* **Application name (Nome da aplicação):** Calculadora;
* **Package name (Nome do pacote):** br.metodista.ads5.calculadora;
* **Create Activity (Criar Activity):** MainActivity - Nome da atividade inicial do projeto.

<figure>
    <a href="/images/2013-03-14-android-calculadora-textview-imageview-02.png"><img src="/images/2013-03-14-android-calculadora-textview-imageview-02.png" alt="Novo projeto Android."></a>
</figure>

Vamos começar alterando o layout da tela, para usar o LinearLayout para deixar um componente embaixo do outro e também vamos alterar o TextView para apresentar os números da calculadora no topo da tela:

{% highlight xml %}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <TextView
        android:id="@+id/resultado"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:scrollHorizontally="true"
        android:textSize="40dp"/>
</LinearLayout>
{% endhighlight %}

Vamos criar logo após o TextView, mais um LinearLayout para colocarmos os botões para os números 7, 8 e 9 e a operação dividir:

{% highlight xml %}
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="20dp"
    android:gravity="center_horizontal" >
    <Button
        android:id="@+id/button1"
        android:layout_width="80dp"
        android:layout_height="70dp"
        android:text="7"
        android:textSize="40dp" />
    <Button
        android:id="@+id/button2"
        android:layout_width="80dp"
        android:layout_height="70dp"
        android:text="8"
        android:textSize="40dp" />
    <Button
        android:id="@+id/button3"
        android:layout_width="80dp"
        android:layout_height="70dp"
        android:text="9"
        android:textSize="40dp" />
    <Button
        android:id="@+id/dividir"
        android:layout_width="80dp"
        android:layout_height="70dp"
        android:text="/"
        android:textSize="40dp" />
</LinearLayout>
{% endhighlight %}

Note que nesse LinearLayout foi utilizado a propriedade **layout_marginTop** para dar um espaço entre os botões e a apresentação do resultado.

Crie agora os próximos botões para deixarmos a tela com o seguinte layout:

<figure>
    <a href="/images/2013-03-14-android-calculadora-textview-imageview-03.png"><img src="/images/2013-03-14-android-calculadora-textview-imageview-03.png" alt="Layout da calculadora."></a>
</figure>

Para aumentar o tamanho de um botão podemos utilizar a propriedade **layout_height** para aumentar o comprimento e a propriedade **layout_width** para aumentar a altura, se quiser utilizar o tamanho total do componente pai, pode ser dado o valor **match_parent**.

Para adicionar uma ação ao clicar no botão, utilizamos a propriedade onClick que recebe como valor o nome do método que será chamado na Activity, por exemplo podemos alterar o botão 7:

{% highlight xml %}
<Button
    android:id="@+id/button1"
    android:layout_width="80dp"
    android:layout_height="70dp"
    android:text="7"
    android:textSize="40dp"
    android:onClick="adicionarNumero"/>
{% endhighlight %}

Quando clicar no botão será chamado o método **adicionarNumero** que está declarado na classe **br.metodista.ads5.calculadora.MainActivity** e irá adicionar o valor deste botão no TextView que representa o resultado.

{% highlight java %}
public void adicionarNumero(View view) {
    String numero = ((TextView) view).getText().toString();
    TextView resultado = ((TextView) findViewById(R.id.resultado));
    resultado.setText(resultado.getText() + numero);
}
{% endhighlight %}

Adicione esse comportamento para todos os botões de números.

Crie um método que será chamado pelas ações somar, subtrair, multiplicar, dividir  e igual que deve executar o calculo da operação e apresentar o resultado na tela.
