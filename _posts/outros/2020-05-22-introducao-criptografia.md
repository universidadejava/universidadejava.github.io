---
layout: article
title: "Introdução a criptografia"
categories: outros
author: sakurai
date: 2020-05-22 23:07:00
tags: [java, criptografia]
published: true
excerpt: Conheça uma breve introdução sobre criptografia implementando uma cifra de substituição.
comments: true
image:
  teaser: 2020-05-22-teaser-introducao-criptografia.png
ads: false
---

> "Criptografia é o estudo dos princípios e técnicas pelas quais a informação pode ser transformada da sua forma original para outra ilegível."

Wikipédia: [https://pt.wikipedia.org/wiki/Criptografia](https://pt.wikipedia.org/wiki/Criptografia).


## Criptografia na história

> "Cifra de César é um tipo de cifra de substituição na qual cada letra do texto é substituída por outra, que se apresenta no alfabeto abaixo dela um número fixo de vezes."

Wikipédia: [https://pt.wikipedia.org/wiki/Cifra_de_C%C3%A9sar](https://pt.wikipedia.org/wiki/Cifra_de_C%C3%A9sar).

### Implementando a cifra de substituição

Este tipo de cifra consiste em mover os caracteres pelo alfabeto. O método `public static String criptografar(String msg, int chave)` recebe uma String contendo a mensagem que será cifrada e também um número inteiro que representa quanto vamos mudar cada um dos caracteres da mensagem.

{% highlight java %}
public static String criptografar(String msg, int chave) {
    String msgCript = "";
    for(int i = 0; i < msg.length(); i++) {
        msgCript += (char) (msg.charAt(i) + chave);
    }
    return msgCript;
}
{% endhighlight %}

O método `public static String descriptografar(String msgCript, int chave)` é similar ao método `criptografar`, a única diferença é que subtraimos o valor da chave. Lembrando que o mesmo número de chave usado para criptografar deve ser usado também para descriptografar.

{% highlight java %}
public static String descriptografar(String msgCript, int chave) {
    String msg = "";
    for(int i = 0; i < msgCript.length(); i++) {
        msg += (char) (msgCript.charAt(i) - chave);
    }
    return msg;
}
{% endhighlight %}

O método a seguir apresenta um exemplo de como criptografar e descriptografar usando a cifra de substituição:

{% highlight java %}
public static void main(String[] args) {
    String msg = "Olá, tudo bom?";
    int chave = 3;

    String msgCifrada = criptografar(msg, chave);
    System.out.println("Msg criptografada: " + msgCifrada);
    String textoPuro = descriptografar(msgCifrada, chave);
    System.out.println("Msg original: " + textoPuro);
}
{% endhighlight %}

O resultado da mensagem criptografada e descriptografada:

{% highlight java %}
Mensagem criptografada: Roä/#wxgr#erpB
Mensagem descriptografada: Olá, tudo bom?
{% endhighlight %}

## Conteúdos relacionados

- [Criptografia simetrica usando chave privada](http://www.universidadejava.com.br/outros/criptografia-simetrica/)
- [Criptografia assimétrica utilizando um par de chaves publica/privada](http://www.universidadejava.com.br/outros/criptografia-assimetrica/)