---
layout: article
title: "Java - Fatorial"
categories: java
author: sakurai
date: 2020-05-16 23:18:00
tags: [java, recursividade, fatorial]
published: true
excerpt: Exemplo de método recursivo que calcula fatorial n!
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Exemplo de fatorial:

A chamada de um método para ele mesmo, é igual a chamada de qualquer outro método, exemplo de método recursivo que calcula o fatorial n!

O fatorial de um número é dado pela multiplicação de seus antecessores, ou seja, se n é igual 3, então seu fatorial será 3 * 2 * 1. O fatorial de 0! (zero) é igual a 1. 

O método recursivo fica da seguinte forma:

{% highlight java %}
package material.recursao;

public class Fatorial {
  public static void main(String[] args) {
    Fatorial r = new Fatorial();
    int resp = r.fatorial(3);
    System.out.println(resp);
  }
    
  /**
   * Calcula o valor do fatorial para um número qualquer positivo.
   * 
   * @param x - valor que será calculado o fatorial.
   * @return O valor do fatorial.
   */
  public int fatorial(int x) {
    // Se x for igual a 0 (zero) então retorna 1.
    if (x == 0)
      return 1;
        
    /* Para qualquer outro número, calcula o seu valor multiplicado
       pelo fatorial de seu antecessor. */
    return x * fatorial(x - 1);
  }
}
{% endhighlight %}

Dentro de um método recursivo é muito importante definirmos como será a condição base para que o método pare a recursão, ou seja, como o método vai parar de se chamar.

Neste caso queremos que o método para de chamar ele mesmo, quando o valor que será calculado o fatorial for igual a 0 (zero), pois neste caso sabemos a resposta direta sem ter que fazer cálculos.

Chamando o método fatorial(3), queremos calcular 3 * 2 * 1.

{% highlight java %}
3 * fatorial(2) 			    retorna (6)
        2 * fatorial(1) 		retorna (2)
            1 * fatorial(0) 	retorna (1)
{% endhighlight %}

Explicando o fluxo do programa:

1. O método fatorial recebe o valor de x igual a 3, verifica se x é igual a 0 (zero), como não é igual, então calcula 3 multiplicado por fatorial(2), neste ponto estamos fazendo uma chamada recursiva.

2. O método fatorial recebe o valor de x igual a 2, verifica se x é igual a 0 (zero), como não é igual, então calcula 2 multiplicado por fatorial(1).

3. O método fatorial recebe o valor de x igual a 1, verifica se x é igual a 0 (zero), como não é igual, então calcula 1 multiplicado por fatorial(0).

4. O método fatorial recebe o valor de x igual a 0 (zero), verifica se x é igual a 0 (zero), então para a execução do método e retorna o valor 1.

5. Volta para o método fatorial(1) na linha 26 e faz a multiplicação de x que vale 1 pelo resultado do fatorial(0) que é 1, ou seja 1 * 1 e retorna o valor 1.

6. Volta para o método fatorial(2) na linha 26 e faz a multiplicação de x que vale 2 pelo resultado do fatorial(1) que é 1, ou seja 2 * 1 e retorna o valor 2.

7. Volta para o método fatorial(3) na linha 26 e faz a multiplicação de x que vale 3 pelo resultado do fatorial(2) que é 2, ou seja 3 * 2 e retorna o valor 6.

8. Volta para o método que chamou o fatorial(3), neste caso o método main() na linha 7, guarda o resultado do fatorial(3) que é 6, dentro da variável resp, e imprime o resultado da variável resp na linha 8.