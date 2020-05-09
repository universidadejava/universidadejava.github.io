---
layout: article
title: "Java - switch"
categories: java
author: sakurai
date: 2020-05-09 00:01:00
tags: [java]
published: true
excerpt: switch em Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Estruturas de controle switch

O **switch** é uma estrutura de seleção semelhante ao **if** com múltiplas seleções. É uma estrutura muito fácil de utilizar e apresenta uma ótima legibilidade, porem trabalha apenas com valores constantes dos tipos primários byte, short, int e char. É possível também enumerar n possíveis blocos de instrução.

Sua utilização deve ser feita da seguinte maneira:

{% highlight java %}
switch( variável ) {
  case <possível valor da constante> :
    < instruções>
    break;

  case <possível valor da constante> :
    < instruções>
    break;

  default:
  < instruções>
    break;
}
{% endhighlight %}

Cada **case** é um caso no qual os comandos dentro dele são executados se o valor dele for o mesmo que a variável recebida no **switch**.

É importante lembrar que a utilização do comando **break** é facultativa, porem indispensável caso se queira que apenas aquele bloco seja executado e não todos os demais abaixo dele.

O bloco de comandos **default** representa uma condição geral de execução caso nenhuma das anteriores tenha sido atendida, sendo a sua utilização também opcional.

Segue um exemplo de utilização do comando switch:

{% highlight java %}
/**
 * Exemplo de estrutura de seleção SWITCH
 */
public class ExemploSwitch {
  public static void main(String[] args) {
    char nota = 'D';

    switch(nota) {
      case 'A':
        System.out.println("Aluno aprovado. Conceito excelente!");
        break;
      case 'B':
        System.out.println("Aluno aprovado. Conceito bom!");
        break;
      case 'C':
        System.out.println("Aluno aprovado. Conceito medio!");
        break;
      default:
        System.out.println("Aluno reprovado!");
        break;
    }
  }
}
{% endhighlight %}

Neste caso, será impressa a mensagem **“Aluno reprovado !”**, pois nenhum dos **cases** foi atendido, então a estrutura **default** foi executada.

{% highlight java %}
C:\>javac ExemploSwitch.java
C:\>java ExemploSwitch
Aluno reprovado!
{% endhighlight %}