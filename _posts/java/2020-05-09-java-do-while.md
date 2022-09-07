---
layout: article
title: "do/while"
categories: java
author: sakurai
date: 2020-05-09 00:08:00
tags: [java, do while, loop, repetição]
published: true
excerpt: do/while em Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Estruturas de repetição do / while

A estrutura **do / while** tem seu bloco de instruções executados pelo menos uma vez, então se a condição ao final das instruções for **true**, o bloco de instruções é executado novamente.

{% highlight java %}
do {
		< instruções >
} while(condição);
{% endhighlight %}

Exemplo:

{% highlight java %}
import java.util.Scanner;

/**
 * Exemplo de estrutura de repetição do/while.
 */
public class ExemploDoWhile {
  public static void main(String[] args) {
    Scanner entrada = new Scanner(System.in);
    
    int opcao = 0;

    do {
      System.out.println("Escolha uma opcao:");
      System.out.println("1 - Iniciar jogo");
      System.out.println("2 - Ajuda");
      System.out.println("3 - Sair");
      System.out.println("OPCAO: ");
      opcao = entrada.nextInt();
    } while (opcao != 3);
  }
}
{% endhighlight %}

Neste caso, será pedido ao usuário digitar um número, e enquanto o número digitado for diferente de **3**, o bloco será executado novamente.

{% highlight java %}
C:\>javac ExemploDoWhile.java
C:\>java ExemploDoWhile
Escolha uma opcao:
1 - Iniciar jogo
2 - Ajuda
3 - Sair
OPCAO:
1
Escolha uma opcao:
1 - Iniciar jogo
2 - Ajuda
3 - Sair
OPCAO:
3
{% endhighlight %}

No vídeo a seguir mostramos passo a passo como você pode usar a estrutura de repetição **do / while**:

<iframe width="420" height="315" src="https://www.youtube.com/embed/0YpK1hrgc6c" frameborder="0" allowfullscreen></iframe>