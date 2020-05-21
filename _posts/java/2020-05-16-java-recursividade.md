---
layout: article
title: "Implementando métodos recursivos"
categories: java
author: sakurai
date: 2020-05-16 23:12:00
tags: [java, recursividade]
published: true
excerpt: Recursão é um método de programação no qual uma função chama a si mesma. A recursão é utilizada quando queremos resolver um subproblema do mesmo tipo menor.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Recursividade

### Chamada de método

Quando um método precisa utilizar a funcionalidade de outro método, precisamos fazer com que um método chame outro método, no exemplo vamos criar um programa que imprima a soma dos números pares de `[1 .. x]`, em que `x` é um número inteiro positivo.

Vamos criar um método `main()` que é por onde começa a execução do programa, depois vamos criar um método chamado `private void imprimir(int x)` que irá somar os números pares entre `1` e `x`.

{% highlight java %}
package material.recursao;

public class ImprimirPares {
  public static void main(String[] args) {
    System.out.println("Inicio do programa.");
    ImprimirPares ip = new ImprimirPares();
    ip.imprimir(5);
    System.out.println("Fim do programa.");
  }

  private void imprimir(int x) {
    int soma = 0;

    for(int cont = 0; cont < x; cont++) {
      if(cont % 2 == 0)
        soma = soma + cont;
    }

    System.out.println("Soma dos pares: " + soma);
  }
}
{% endhighlight %}

- Na linha 10 foi criado o método `main()` por onde vai começar a execução do método.
- Na linha 11 vamos imprimir a frase "Inicio do programa.".
- Na linha 12 é criado um objeto da classe `ImprimirPares` para podermos chamar o método `imprimir()` dessa classe.
- Na linha 13 chama o método `private void imprimir(int x)` passando o valor `5` como parâmetro para a variável `x`. Quando a execução do programa chega nesta linha o método `imprimir()` é chamado e a execução do método vai para a linha 17.
- Da linha 18 a 23 temos o algoritmo que soma os números pares.
- Na linha 25 imprime a soma dos números pares entre 1 e 5, ou seja, vai imprimir "Soma dos pares: 6".
- Na linha 26 acaba o método `imprimir()`, agora a execução do programa volta para o método `main()`, pois foi ele que chamou o método `imprimir()`, repare também que a execução do método volta para a linha posterior a chamada do método, ou seja, vai para a linha 13.
- Na linha 14 vamos imprimir a frase "Fim do programa.".
- Na linha 15 acaba o método `main()` e encerra o programa.

Quando o programa é encerrado temos a seguinte saída no console:

{% highlight java %}
Inicio do programa.
Soma dos pares: 6
Fim do programa.
{% endhighlight %}

> toda vez que um método termina sua execução, o fluxo de execução do programa volta para a linha posterior a qual ele foi chamado.
	
### Recursão

Recursão é um método de programação no qual uma função chama a si mesma. A recursão é utilizada quando queremos resolver um subproblema do mesmo tipo menor.

Se o problema é pequeno
- não resolva o problema diretamente

Senão
- Reduza o problema em um problema menor, chame novamente o método para o problema menor e volte ao problema original.

A chamada ao método recursivo é igual a uma chamada de método normal, quando a execução do método terminar ele deve voltar para o mesmo método, só preste atenção, pois o estado do método pode ser diferente para cada vez que ele se chama.
	
Mas o que isso quer dizer?

Os valores das variáveis passadas por parâmetro podem ser diferentes para cada vez que o método se chama, no exemplo abaixo vamos reescrever o método imprimir, mas agora vamos criar seu código de forma recursiva.

Antes de começar a escrever o código um método recursivo, precisamos pensar qual condição será interrompida a chamada recursiva, ou seja, quando o método precisa parar de se chamar.

Neste caso ele precisa parar a chamada do método quando terminarmos de percorrer o intervalo de `1` até `x` ou de `x` até `1`.

Depois precisamos pensar no objetivo do método que é somar números pares.

Segue o código do método recursivo, o método ira somar os números pares de `x` até `1`.

{% highlight java %}
package material.recursao;

public class ImprimirParesRecursivo {
  public static void main(String[] args) {
    System.out.println("Inicio do programa.");
    ImprimirParesRecursivo ip = new ImprimirParesRecursivo();
    System.out.println(ip.imprimirRecursivo(5));
    System.out.println("Fim do programa.");
  }

  private int imprimirRecursivo(int x) {
    if(x == 0)
      return 0;

    if(x % 2 == 0)
      return x + imprimirRecursivo(x - 1);

    return imprimirRecursivo(x - 1);
  }
}
{% endhighlight %}

- Na linha 13 chama o método `private int imprimirRecursivo(int x)` passando o valor 5 como parâmetro. Quando a execução do programa chega nesta linha o método `imprimirRecursivo()` é chamado e a execução do método vai para a linha 17.
- Na linha 17 declaramos o método recursivo, perceba que agora o método está retornando um número inteiro, dessa forma podemos calcular o valor da soma dos números pares e retornar este valor.
- Na linha 18 verificamos se o valor de `x` é igual a `0` (zero), porque não queremos somar nenhum número menor que zero, e a soma do número 0 (zero) não irá alterar a resposta. Então se o valor de `x` for zero na linha 19 retorna o valor zero.
- Na linha 21 verificamos se o valor de `x` é par, porque queremos somar apenas os números pares, e se for par então na linha 22 soma o valor de `x` com o valor do resultado da chamada recursiva do método `imprimirRecursivo()` passando como parâmetro `x – 1`.
- A linha 24 será executada quando o valor de `x` for diferente de zero e também quando o número for impar.

Então teremos as seguintes chamadas de método:

<figure>
    <a href="/images/2020-05-16-java-recursao-01.png"><img src="/images/2020-05-16-java-recursao-01.png" alt="Chamadas de métodos recursivos para imprimir números pares."></a>
</figure>

O método `main()` vai chamar o método `imprimirRecursivo(5)`, que por sua vez vai chamar o método `imprimirRecursivo(4)` até chegar no `imprimirRecursivo(0)`, que irá parar as chamadas recursivas e volta para quem chamou ele passando o resultado da soma recursiva.

### Exemplo de ordem decrescente:

Método recursivo que recebe um número `x` por parâmetro e imprime seu valor em ordem decrescente até `1`.

{% highlight java %}
package material.recursao;

public class Imprimir {
  public static void main(String[] args) {
    Imprimir imprimir = new Imprimir();
    imprimir.imprimirSequencia(10);
  }
    
  // Método que imprime uma sequencia de números na ordem decrescente [x .. 1].
  public void imprimirSequencia(int x) {
    /* Quando o valor de x for igual a 0 (zero), para a chamada do método
       recursivamente. */
    if (x == 0)
      return;

    // Imprime o valor de x.
    System.out.println(x);
    // Chama recursivamente este mesmo método passando o valor de x menos 1.
    imprimirSequencia(x - 1);
  }
}
{% endhighlight %}

Quando usamos recursão, precisamos definir o momento de parada, quando a função não deve ser mais chamada.
No caso do exemplo anterior queremos que o método não si chame novamente quando o `x` for igual a `0` (zero), porque queremos apenas os números entre `[x ... 1]`:

{% highlight java %}
if (x == 0)
  return;
{% endhighlight %}

Agora precisamos definir o que nosso método deve fazer, neste caso deve imprimir o valor de `x`, e em seguida chama a si mesma diminuindo em `1` o valor de `x`.

{% highlight java %}
System.out.println(x);
imprimirSequencia(x - 1);
{% endhighlight %}

A próxima vez que o método `imprimirSequencia(int x)` for chamado, o valor de `x` diminui `1` até chegar a `0` (zero) e parar a execução do código.

<figure>
    <a href="/images/2020-05-16-java-recursao-02.png"><img src="/images/2020-05-16-java-recursao-02.png" alt="Chamadas de métodos recursivos para imprimir números pares."></a>
</figure>


### Conteúdos relacionados

- [Implementando Fibonacci de modo recursivo no Java](http://www.universidadejava.com.br/java/java-fibonacci/)
- [Calculando o fatorial de um número de forma recursiva no Java](http://www.universidadejava.com.br/java/java-fatorial/)
- [Entendendo como funciona a implementaçãdo do Quick Sort](http://www.universidadejava.com.br/pesquisa_ordenacao/quick-sort/)
- [Definindo, instanciando e usando array no Java](http://www.universidadejava.com.br/java/java-array/)