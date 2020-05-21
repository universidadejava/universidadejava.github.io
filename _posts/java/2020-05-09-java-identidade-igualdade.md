---
layout: article
title: "Identidade e igualdade"
categories: java
author: sakurai
date: 2020-05-09 23:05:00
tags: [java, equals, new, referência, instanciação]
published: true
excerpt: Identidade e igualdade de objetos.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Identidade e igualdade de objetos

Ao criar um objeto a partir de uma classe, criamos uma variável que serve de meio de acesso ao objeto criado em memória, ou como chamamos, uma **referência**.

Exatamente por este motivo, sempre que declaramos um novo objeto, temos de utilizar o comando **new**, pois o mesmo irá efetivamente criar o novo objeto em memória e fornecer o seu "endereço" a variável declarada. 

O comando **new** recebe um parâmetro que é um construtor de classe.

No exemplo a seguir, vamos trabalhar com a classe Pessoa:

{% highlight java %}
/**
 * Classe utilizada para representar uma Pessoa.
 */
public class Pessoa {
  private String nome;
  private int idade;

  public int getIdade() {
    return idade;
  }

  public void setIdade(int idade) {
    this.idade = idade;
  }

  public String getNome() {
    return nome;
  }

  public void setNome(String nome) {
    this.nome = nome;
  }
}
{% endhighlight %}

Logo, na referência a seguir:

{% highlight java %}
Pessoa manuel = new Pessoa();
{% endhighlight %}

O correto é dizer que **manuel** é uma referência a um objeto do tipo **Pessoa** e não que **manuel** é o objeto propriamente dito.

Com isso, podemos afirmar que em Java uma variável nunca é um objeto, ela apenas será a representação de um tipo primitivo ou uma referência a um objeto.

No exemplo a seguir, vamos ver como funciona a referência de um objeto:

{% highlight java %}
/**
 * Classe utilizada para mostrar identidade de objeto.
 */
public class TestePessoa {
  public static void main(String[] args) {
    Pessoa paulo = new Pessoa();
    paulo.setIdade(24);

    Pessoa pedro = paulo;
    pedro.setIdade(pedro.getIdade() + 1);

    System.out.println("A idade de Paulo eh: " + paulo.getIdade());
    System.out.println("A idade de Pedro eh: " + pedro.getIdade());
  }
}
{% endhighlight %}

Neste exemplo, a saída do programa será a seguinte:

{% highlight java %}
A idade de Paulo é: 25
A idade de Pedro é: 25
{% endhighlight %}

Agora por que a idade de Paulo foi exibida como **25**, sendo que apenas incrementamos a idade de Pedro?

Isso acontece, pois o comando:

{% highlight java %}
Pessoa pedro = paulo;
{% endhighlight %}

Na verdade copia o valor da variável paulo, para a variável pedro. Por este motivo, como falamos acima, ele copia a referência de um objeto de uma variável para outra. Seria como imaginarmos a figura a seguir:

<figure>
    <a href="/images/2020-05-09-java-identidade-igualdade.png"><img src="/images/2020-05-09-java-identidade-igualdade.png" alt="Referência dos objetos na memória."></a>
</figure>

Nesta figura, podemos notar que, como ambas as variáveis apontam para o mesmo objeto, qualquer operação realizada através da **referência pedro**, também refletirá na **referência paulo**.

Quando queremos comparar se duas variáveis apontam para o mesmo objeto, podemos usar a igualdade **( == )**, por exemplo:

{% highlight java %}
/**
 * Classe utilizada para mostrar igualdade de objeto.
 */
public class TestePessoa2 {
  public static void main(String[] args) {
    Pessoa paulo = new Pessoa();
    paulo.setIdade(24);

    Pessoa pedro = paulo;

    if(paulo == pedro) {
      System.out.println("É a mesma pessoa!!!");
    } else {
      System.out.println("São pessoas diferentes!!!");
    }
  }
}
{% endhighlight %}

Esta comparação vai retornar **true**, pois ambas variáveis paulo e pedro estão apontando para o mesmo objeto da memória.

E quando queremos saber se o conteúdo de dois objetos são iguais, utilizamos o método **equals()**, todas as classes do Java possuem este método.

{% highlight java %}
/**
 * Exemplo para mostrar igualdade de String.
 */
public class ExemploIgualdadeString {
  public static void main(String[] args) {
    String a = new String("a");
    String b = "a";

    if(a.equals(b)) {
      System.out.println("As duas Strings são iguais.");
    } else {
      System.out.println("As duas Strings são diferentes.");
    }
  }
}
{% endhighlight %}

Neste exemplo, será impresso **"As duas Strings são iguais"**, pois o conteúdo das Strings são iguais.

Agora se compararmos essas duas Strings com o operador de igualdade **==** teremos um resposta que não é esperada:

{% highlight java %}
/**
 * Exemplo para mostrar igualdade de String.
 */
public class ExemploIgualdadeString2 {
  public static void main(String[] args) {
    String a = new String("a");
    String b = "a";

    if(a == b) {
      System.out.println("As duas Strings são iguais.");
    } else {
      System.out.println("As duas Strings são diferentes.");
    }
  }
}
{% endhighlight %}

Neste exemplo, será impresso **"As duas Strings são diferentes."**, pois as variáveis não apontam para o mesmo objeto String.

Então cuidado na comparação entre objetos, de preferência ao método **equals()**, use comparação **==** somente com tipos primitivos.

Quando criamos uma nova classe, está classe tem um método equals() que vem da classe **java.lang.Object**.

**O método equals(), que existe na classe que criamos, tem a mesma função que a igualdade ( == ). Caso precisemos saber se duas instancias desta mesma classe são iguais, então temos de sobrescrever o método equals().**