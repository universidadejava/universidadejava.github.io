---
layout: article
title: "Exercícios com laços de repetições"
categories: java
author: sakurai
date: 2020-05-09 12:06:00
tags: [java, exercícios, laço, repetição, for, while, foreach, do while]
published: true
excerpt: Alguns exercícios para você estudar laços de repetições.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Exercícios de laços de repetições

1. Escreva um programa que leia um número positivo inteiro e imprimir a quantidade de dígitos que este número possui, por exemplo:

Número: 13
Dígitos: 2

Número: 321
Dígitos: 3


2. Escreva um programa que leia um numero positivo inteiro entre 1 e 99999999999, depois este número deve ser decomposto e seus dígitos devem ser somado, de forma que enquanto o número tiver mais que um digito ele deve continuar sendo decomposto, por exemplo:

Número: 59765123
Decompondo: 5 + 9 + 7 + 6 + 5 + 1 + 2 + 3 
Soma dos números: 38

Decompondo: 3 + 8
Soma dos números: 11

Decompondo: 1 + 1
Soma dos números: 2


3. Escreva um programa que leia um número positivo inteiro na base 10 e imprima seu valor na base 2, por exemplo:

Base10: 123
Base2: 01111011


4. Escreva um programa que leia um número positivo inteiro e calcule o valor do Fibonacci desse numero.

Sequência de Fibonacci: 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89...


5. Escreva um programa que leia 10 números positivos inteiros entre 1 e 20 e armazene seus valores em um vetor. Depois informe qual o menor número e qual o maior número e a quantidade de vezes que o menor e o maior número aparecem no vetor.


6. Qual a saída da compilação e execução da classe abaixo:

{% highlight java %}
public class Teste {
  public static void main(String[] args) {
    int num = 2;

    switch(num) {
      1:
        System.out.println("1");
      2:
        System.out.println("2");
      3:
        System.out.println("3");
      4:
        System.out.println("4");
        break;
      default:
        System.out.println("default");
    }
  }
}
{% endhighlight %}

a) Imprime “2”
b) Imprime “234”
c) Imprime “234default”
d) Erro de compilação


7. Durante uma partida de golfe surgiu a duvida para saber qua a velocidade em metros por segundo que um bolinha de golfe alcança, desenvolva um programa que pergunte ao usuário qual a distancia que a bolinha de golfe percorreu e quanto tempo ela demorou para percorrer a distancia, para calcular e imprimir sua velocidade em metros por segundo. Utilize a formula velocidade = (distancia * 1000) / (tempo * 60).


8. Desenvolva um programa que some o valor de todos os produtos comprados por um cliente, de acordo com a categoria do cliente pode ser fornecido um desconto:
D - Cliente Diamante - 10% de desconto
O - Cliente Ouro - 5% de desconto
N - Cliente Normal - Sem desconto
No final deve ser impresso o valor total que o cliente deve pagar.


9. Desenvolva um programa que calcule a área total de uma casa. Seu programa deve perguntar quantos retângulos formam a casa e os lados de cada retângulo, no final deve imprimir o tamanho total da área da casa.

Dica: Para conhecer a área de um retângulo, basta multiplicar os lados, por exemplo: A Sala possui 4m x 4,5m então tem uma área de 18 metros. 