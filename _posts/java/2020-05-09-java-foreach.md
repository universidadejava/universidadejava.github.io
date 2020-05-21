---
layout: article
title: "foreach"
categories: java
author: sakurai
date: 2020-05-09 11:57:00
tags: [java, for, laço, foreach]
published: true
excerpt: Muitas vezes o for é utilizado para percorrer um array ou uma coleção, e para facilitar seu uso foi adicionado na versão 5 do Java o enhanced for ou foreach.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Estruturas de repetição "enhanced for" ou “for-each” 

Muitas vezes o **for** é utilizado para percorrer um array ou uma coleção de objetos, para facilitar seu uso foi adicionado na versão 5 do Java o **enhanced for**.

{% highlight java %}
for(<Tipo> <identificador> : <expressão>) {
		<instruções>
}
{% endhighlight %}

Exemplo:

{% highlight java %}
import java.util.ArrayList;
import java.util.List;

/**
 * Exemplo de estrutura de repetição For Each.
 */
public class ExemploForEach {
  public static void main(String[] args) {
    String[] nomes = {"Altrano", "Beltrano", "Ciclano", "Deltrano"};
    //Percorre um array.
    for(String nome : nomes) {
      System.out.println(nome);
    }

    List<Integer> valores = new ArrayList<Integer>();
    valores.add(100);
    valores.add(322);
    valores.add(57);
    //Percorre uma coleção.
    for(Integer numero : valores) {
      System.out.println(numero);
    }
  }
}
{% endhighlight %}

Neste caso, o primeiro **enhanced for** vai percorrer um array de Strings e imprimir os valores “Altrano”, “Beltrano”, “Celtrano” e “Deltrano”.

Depois ira percorrer uma lista de inteiros imprimindo os valores 100, 322 e 57.

{% highlight java %}
C:\>javac ExemploForEach.java
C:\>java ExemploForEach
Altrano
Beltrano
Ciclano
Deltrano
100
322
57
{% endhighlight %}