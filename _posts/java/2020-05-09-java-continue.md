---
layout: article
title: "Java - continue"
categories: java
author: sakurai
date: 2020-05-09 12:02:00
tags: [java]
published: true
excerpt: continue - passando para a próxima iteração do laço.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## continue 

A palavra-chave **continue** tem um comportamento semelhante ao break, porém não interrompe completamente a execução do laço. Este comando pode ser utilizado com qualquer laço, porém ao invés de interromper a execução completa do laço, ele faz com que o laço salte para sua próxima iteração, por exemplo:

{% highlight java %}
import java.util.Scanner;

/**
 * Exemplo de uso da palavra-chave continue.
 */
public class ExemploContinue {
  public static void main(String[] args) {
    Scanner entrada = new Scanner(System.in);
    System.out.println("Digite um numero de 1 a 9 exceto o 5: ");
    int numero = entrada.nextInt();
    System.out.println("Contando de 1 ate o numero que voce digitou…");
    for(int cont = 0; cont <= numero; cont++) {
      if(cont == 5) {
        System.out.println("# PULANDO O 5 #");
        continue;
      }
      System.out.print(cont + " ");
    }
  }
}
{% endhighlight %}

Note que o código acima é bem semelhante ao anterior, porém observe que desta vez, ao invés de interromper o laço caso o usuário digite o número 5 ele irá contar de 1 até o número digitado pelo usuário. Caso ele passe pelo número 5, conforme o condicional da linha 13, ele irá imprimir em tela a mensagem da linha 14 e saltará o restante do código e retornará ao início do laço na linha 12. 

{% highlight java %}
C:\>javac ExemploContinue.java
C:\>java ExemploContinue
Digite um numero de 1 a 9: (desta vez saltaremos o 5)
8
Contando de 1 ate o numero que voce digitou...
0 1 2 3 4 # PULANDO O 5 #
6 7 8
{% endhighlight %}