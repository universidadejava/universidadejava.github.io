---
layout: article
title: "Java - exercicios classe, método e objeto"
categories: java
author: sakurai
date: 2020-05-09 14:41:00
tags: [java]
published: true
excerpt: Exercícios sobre classe, método e objeto.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Exercícios sobre classe, método e objeto

1. Crie a classe Vetor a seguir e siga as instruções nos comentários.

{% highlight java %}
/**
 * Classe utilizada para representar um conjunto de números, em que podemos
 * localizar o valor do maior elemento, menor elemento e
 * media dos elementos do vetor.
 *
 * @author <<Nome>>
 */
public class Numeros {
  int[] valores;

  public Numeros(int[] _valores) {
    System.out.println("\nCriando um objeto da classe Vetor e inicializando o vetor de inteiros.\n");
    valores = _valores;
  }

  public void localizarMaior() {
    /* Deve imprimir o valor do maior elemento do vetor. */
  }

  public void localizarMenor() {
    /* Deve imprimir o valor do menor elemento do vetor. */
  }

  public void calcularMedia() {
    /* Deve imprimir a media de todos os elementos do vetor. */
  }
}
{% endhighlight %}

Agora crie a classe TesteVetor, para verificar a execução dos métodos:

{% highlight java %}
/**
 * Testa a classe Numeros.
 *
 * @author <<Nome>>
 */
public class TesteNumeros {
  public static void main(String[] args) {
    /* 1- Crie um vetor de inteiro com 10 valores. */
    /* 2- Crie um objeto da classe Numeros, passando o vetor de inteiros para o construtor. */

    nums.localizarMaior();
    nums.localizarMenor();
    nums.calcularMedia();
  }
}
{% endhighlight %}

2. Crie uma classe que representa uma conta bancaria que possua o número da conta e saldo. Está classe também deve executar os seguintes métodos:
- extrato (Mostra na tela o número e o saldo da conta)
- saque (Recebe como parâmetro um valor e retira este valor do saldo da conta)
- deposito (recebe como parâmetro um valor e adiciona este valor ao saldo da conta)

Ao final das operações saque e deposito, sua classe deve imprimir o número e o saldo da conta.

Crie uma classe para testar os métodos da classe conta bancaria.


3. Crie uma classe que representa uma calculadora, está calculadora deve ter os seguintes métodos:
- soma (recebe dois números e mostra o valor da soma)
- subtração (recebe dois números e mostra o valor da subtração entre eles)
- divisão (recebe dois números e mostra o valor da divisão entre eles)
- multiplicação (recebe dois números e mostra o valor da multiplicação entre eles)
- resto (recebe dois números e mostra o valor do resto da divisão entre esses dois números)

Crie uma classe para testar os métodos da classe calculadora.


4. Crie uma classe Televisor. Essa classe deve possuir três atributos:

{% highlight java %}
canal  // inicia em 1 e vai até 16
volume // inicia em 0 e vai até 10
ligado // inicia em desligado ou false
{% endhighlight %}

e a seguinte lista de métodos:

{% highlight java %}
aumentarVolume()	// Aumenta em 1 o volume
reduzirVolume()		// Diminui em 1 o volume
subirCanal()		// Aumenta em 1 o canal
descerCanal()		// Diminui em 1 o canal
ligarTelevisor()	// Liga a televisão
desligarTelevisor()	// Desliga a televisão
mostraStatus() 		// Dizer qual o canal, o volume e se o televisor está ligado
{% endhighlight %}

Nos métodos informe se é possível realizar a operação, por exemplo, se o volume estiver no 10 e chamar o método aumentarVolume() novamente imprima uma mensagem de aviso, etc.

Quando desligado, nosso televisor deve voltar o canal e o volume a seus valores iniciais e não deve realizar nenhuma operação.

Crie uma classe para testar a classe Televisao.