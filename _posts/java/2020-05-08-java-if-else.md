---
layout: article
title: "Java - if/else"
categories: java
author: sakurai
date: 2020-05-08 23:51:00
tags: [java]
published: true
excerpt: if/else em Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Estruturas de controle if / else

A estrutura de controle *if* (se), é utilizada para executar alguns comandos apenas se a sua condição for true (verdadeira). O *else* (senão) pode ou não acompanhar o *if*, mas o *else* não pode ser usado sozinho, e é utilizado para executar alguns comandos caso a condição do *if* for *false* (falso).

Na linguagem Java, esta estrutura pode ser utilizada de diversas maneiras, conforme listadas abaixo.

- Com execução de um bloco de instruções, apenas caso a condição seja atendida:

{% highlight java %}
if(condição) {
  // Comandos executados caso a condição verdadeira
}
{% endhighlight %}

- Com execução de um bloco de instruções, caso a condição seja atendida, e com um fluxo alternativo para os casos de condição não atendida:

{% highlight java %}
if(condição) {
  // Comandos executados caso a condição verdadeira.
} else {
  // Comandos executados caso a condição falsa.
}
{% endhighlight %}

- Com múltiplas condições:

{% highlight java %}
if(condição 1) {
  // Comandos executados caso a condição 1 verdadeira.
} else if(condição 2) {
 // Comandos executados caso a condição 2 verdadeira.
} else if(condição 3) {
 // Comandos executados caso a condição 3 verdadeira.
} else {
  // Comandos executados caso nenhuma das condições for verdadeira.
}
{% endhighlight %}

No exemplo a seguir, verificamos se o valor da variável *idade* é *maior ou igual a 18*, caso a condição seja *verdadeira* então entra no bloco do *if*, caso contrário entra no bloco do *else*.

{% highlight java %}
/**
 * Exemplo de estrutura de controle if.
 */
public class ExemploIf {
  public static void main(String[] args) {
    int idade = 15;

    if(idade >= 18) {
      System.out.println("Permissão para dirigir");
    } else {
      System.out.println("Idade minima para dirigir eh 18 anos.");
    }
  }
}
{% endhighlight %}

Quando executarmos a classe *ExemploIf*, temos a seguinte saída no console:

{% highlight java %}
C:\>javac ExemploIf.java
C:\>java ExemploIf
Idade minima para dirigir eh 18 anos.
{% endhighlight %}

Dentro de um bloco *{ }* do *if / else* pode ser utilizado outras variáveis declaradas no método ou declarados dentro do bloco, mas estas variáveis podem apenas ser utilizadas dentro deste próprio bloco, por exemplo:

{% highlight java %}
/**
 * Exemplo de estrutura de controle if.
 */
public class ExemploIf2 {
  public static void main(String[] args) {
    int valor = 10;

    if(valor > 9) {
      int xpto = 10;
    } else {
      xpto = 11; // Erro de compilação.
    }

    xpto = 12; // Erro de compilação.
  }
}
{% endhighlight %}

Se tentarmos compilar a classe *ExemploIf2*, teremos os seguintes erros:

{% highlight java %}
C:\>javac ExemploIf2.java
ExemploIf2.java:11: cannot find symbol
symbol  : variable xpto
location: class ExemploIf2
           xpto = 11;
           ^
ExemploIf2.java:14: cannot find symbol
symbol  : variable xpto
location: class ExemploIf2
           xpto = 12;
           ^
2 errors
{% endhighlight %}

Neste caso é criado uma variável chamada *xpto* dentro do bloco do *if*, então esta variável pode ser *utilizada somente dentro do if*, não pode ser usada no *else* e nem fora do bloco.

Observação: este conceito de variáveis criadas dentro de blocos *{ }*, funciona para todos os blocos.