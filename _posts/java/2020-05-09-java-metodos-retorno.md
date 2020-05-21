---
layout: article
title: "Métodos com retorno"
categories: java
author: sakurai
date: 2020-05-09 14:27:00
tags: [java, método, method, return]
published: true
excerpt: Utilizando métodos com retorno de valor.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Métodos com retorno de valor

Os métodos das classes em Java servem para executar partes específicas de códigos. Utilizando os métodos podemos reaproveitar o código. Uma vez que o método é criado, o mesmo pode ser usado em diversas partes do sistema. Os métodos podem receber variáveis como parâmetros e devolver uma variável como retorno da execução do método.

Para que seja possível realizar o retorno do método, primeiramente é necessário declarar qual o tipo de retorno será realizado pelo método. No exemplo a seguir, criamos dois métodos:

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso de
 * métodos com retorno de valor;
 */
public class MetodoRetorno {
  /* Declaração dos atributos da classe. */
  public int atributo1;

  /* Declaração dos métodos da classe. */
  /**
   * Método utilizado para retornar o atributo1.
   * @return int com o valor do atributo1.
   */
  public int metodo1() {
    System.out.println("Chamou o metodo 1.");
    return atributo1;
  }

  /**
   * Método que verificar se o atributo1 é maior ou igual a 0 (zero).
   * @return boolean informando se o atributo1 é positivo
   */
  public boolean metodo2() {
    System.out.println("Chamou o metodo 2.");
    return atributo1 >= 0;
  }
}
{% endhighlight %}

Note que no ponto em que até há pouco utilizávamos a palavra-chave **void** (que significa vazio ou sem retorno) no retorno do método, agora utilizamos a palavra-chave **int**. Essa mudança na declaração significa que pretendemos que este método retorne para quem o chamou um valor do inteiro tipo int.

Porém, para que o método realmente retorne algum valor, é necessário identificar qual valor será retornado. Para tal, faremos uso da palavra-chave **return**, da seguinte forma:

{% highlight java %}
public int metodo1() {
  return atributo1;
}
{% endhighlight %}

Com isso, temos agora o **metodo1()** que retornará o valor do **atributo1**, que é do tipo **int**.

É importante lembrar que, uma vez declarado um retorno para o método, é obrigatório que este retorno aconteça. O fato de não se enviar um retorno resulta em um erro de compilação.

No **metodo2()** verificamos se o atributo1 é maior ou igual a 0 (zero), e retornamos a resposta dessa verificação, ou seja, **true** (verdadeiro) ou **false** (falso).

A seguir vamos criar um objeto da classe **MetodoRetorno** e vamos testar a chamada para os métodos **metodo1()** e **metodo2()**.

{% highlight java %}
/**
 * Classe utilizada para demonstrar o uso de
 * métodos com retorno de valor;
 */
public class TesteMetodoRetorno {
  /**
   * Método principal para testar está classe.
   */
  public static void main(String[] args) {
    System.out.println("Criando um objeto da classe MetodoRetorno.");
    MetodoRetorno teste = new MetodoRetorno();
    teste.atributo1 = 30;
    System.out.println(teste.metodo1());
    System.out.println(teste.metodo2());
  }
}
{% endhighlight %}

Quando executamos a classe **TesteMetodoRetorno**, temos a seguinte saída no console:

{% highlight java %}
C:\>javac MetodoRetorno.java
C:\>javac TesteMetodoRetorno.java
C:\>java TesteMetodoRetorno
Criando um objeto da classe MetodoRetorno
Chamou o metodo 1.
30
Chamou o metodo 2.
true
{% endhighlight %}