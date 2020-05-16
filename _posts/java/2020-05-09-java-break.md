---
layout: article
title: "Java - break"
categories: java
author: sakurai
date: 2020-05-09 12:00:00
tags: [java, break]
published: true
excerpt: break - interrompendo um laço de repetição.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## break

A palavra-chave **break** é uma instrução de interrupção imediata de qualquer laço, seja ele qual for, independente de sua condição de parada ter sido atendida.

Vamos ao exemplo:

{% highlight java %}
import java.util.Scanner;

/**
 * Exemplo de uso da palavra-chave break.
 */
public class ExemploBreak {
  public static void main(String[] args) {
    Scanner entrada = new Scanner(System.in);
    System.out.println("Digite um numero de 1 a 9 exceto o 5: ");
    int numero = entrada.nextInt();
    System.out.println("Contando de 1 ate o numero que voce digitou…");
    for(int cont = 0; cont <= numero; cont++) {
      if(numero == 5 || numero < 1 || numero > 9) {
        System.out.println("Um numero proibido foi digitado!");
        break;
      }
      System.out.print(cont + " ");
    }
  }
}
{% endhighlight %}

Observe que, segundo o condicional da linha 13, caso um número inválido seja digitado o laço **for** iniciado na linha 12 será interrompido imediatamente devido a instrução break existente na linha 15. Uma vez executado o código acima, a seguinte saída será projetada caso o usuário digite o número 5:

{% highlight java %}
C:\>javac ExemploBreak.java
C:\>java ExemploBreak
Digite um numero de 1 a 9 exceto o 5 :
5
Contando de 1 ate o numero que voce digitou...
Um numero proibido foi digitado!
{% endhighlight %}