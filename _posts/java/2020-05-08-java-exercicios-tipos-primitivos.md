---
layout: article
title: "Java - exercícios com operadores e tipos primitivos"
categories: java
author: sakurai
date: 2020-05-08 22:00:00
tags: [java, tipos, primitivos, operadores, exercícios]
published: true
excerpt: Exercícios com operadores e tipos primitivos em Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Exercícios com operadores e tipos primitivos

1. Qual resultado será impresso:

a)

{% highlight java %}
public class Main {
  public static void main(String[] args) {
    int a = 3;
    int b = 4;
    int c = 7;

    System.out.println((a+b) / c);
  }
}
{% endhighlight %}

b)

{% highlight java %}
public class Main {
  public static void main(String[] args) {
    int a = 3;
    int b = 4;
    int c = 7;

    System.out.println(!((a > b) && (a < c)));
  }
}
{% endhighlight %}

c)

{% highlight java %}
public class Main {
  public static void main(String[] args) {
    int a = 3;
    int b = 4;
    int c = 7;

    if(a++ >= b)
      System.out.println(--c);
    else
      System.out.println(c++);
  }
}
{% endhighlight %}

d)

{% highlight java %}
public class Main {
  public static void main(String[] args) {
    int a = 3;
 
    System.out.println(a % 2 == 0 ? ++a : a++);
  }
}
{% endhighlight %}

e)

{% highlight java %}
public class Main {
  public static void main(String[] args) {
    int a = 178;
    int b = 131;
    int c = 33;

    System.out.println(a & b | c);
  }
}
{% endhighlight %}

2. O que acontece se tentarmos compilar e executar o seguinte código?

{% highlight java %}
class Teste {
    public static void main(String args) {
        System.out.println(args);
    }
}
{% endhighlight %}

a) Não imprime nada.
b) Erro de compilação na linha 2
c) Imprime o valor de args
d) Exceção na thread "main" java.lang.NoSuchMethodError: main
e) Erro em tempo de execução.


3. O que acontece se tentarmos compilar e executar o seguinte código?

{% highlight java %}
class Soma{
    int x, y, z;
    public static void main (String[] args) {
        System.out.println(x + y + z);
    }
}
{% endhighlight %}

a) Não imprime nada
b) Imprime: null
c) Imprime: 0
d) Erro de compilação
e) Erro em tempo de execução


4. O que acontece se tentarmos compilar e executar o seguinte código?

{% highlight java %}
class Teste {
    public static void main(String[] args) {
        char a = 'a'; // 'a' = 97
        char b = 'b'; // 'b' = 98
        System.out.println(a + b + "" + b + a);
    }
}
{% endhighlight %}

a) abba
b) 97989897
c) 195ba
d) ab195
e) 390
f) Erro de Compilação
g) Erro em tempo de execução