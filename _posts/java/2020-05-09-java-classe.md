---
layout: article
title: "Java - classe"
categories: java
author: sakurai
date: 2020-05-09 14:15:00
tags: [java, classe, class]
published: true
excerpt: Classe em Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Classe

Uma classe no Java representa um modelo ou forma do mundo real que se queira reproduzir no ambiente de desenvolvimento. Pensando desta forma é muito fácil entender e pensar sobre como se projetar um sistema com orientação a objetos.

Quando pensamos em desenvolver um sistema voltado a locação de veículos, por exemplo, a primeira classe que vem a tona é uma que possa representar a figura do carro no seu sistema. Daí temos, então, nossa primeira classe. Podemos também representar a Locação, Cliente e outros objetos.

Uma classe é composta basicamente de 3 itens:

**Nome da Classe**

Item responsável por identificar a classe. Este nome será utilizado toda a vez em que se for utilizar um objeto deste tipo. É importante ressaltar a utilização de letra maiúscula na primeira letra do nome da classe e as demais minúsculas. Caso o nome de sua classe seja composto por mais de uma palavra, coloque sempre a primeira letra de cada palavra em letra maiúscula. Isto não é uma obrigatoriedade do Java; é apenas uma boa prática de desenvolvimento que visa melhorar a legibilidade do código.

Ex: Carro, Pessoa, ContaCorrente, CaixaCorreio, etc.

**Atributos**

São valores que possam representar as propriedades e/ou estados possíveis que os objetos desta classe podem assumir. Por convenção, costuma-se escrever o atributo com letras minúsculas, a menos que ele seja composto por mais de uma palavra, a primeira palavra é toda em minúsculo e as demais começam com a primeira letra em maiúsculo e o restante da palavra em minúsculo.

Ex: idade, nome, listaMensagens, notaAlunoTurma, etc.

**Métodos**

São blocos de código que possam representar as funcionalidades que a classe apresentará. Assim como os atributos, costuma-se escrever o atributo com letras minúsculas, a menos que ele seja composto por mais de uma palavra. Neste caso, a mesma regra citada nos nomes de Atributos também é válida.

Ex: getPessoa, consultarDadosAluno, enviarMensagemEmail, etc.

Na UML representamos uma classe da seguinte forma:

<figure>
    <a href="/images/2020-05-09-java-estrutura-classe-uml.png"><img src="/images/2020-05-09-java-estrutura-classe-uml.png" alt="Representação de uma classe usando UML."></a>
</figure>

Em Java, utilizamos a palavra-chave **class** para declararmos uma classe. No exemplo a seguir, criamos uma classe chamada **NovaClasse**:

{% highlight java %}
/**
 * Classe utilizada para demonstrar a estrutura de uma classe.
 */
public class NovaClasse {
  /* Declaração dos atributos da classe. */

  public int atributo1;
  public float atributo2;
  public boolean atributo3;

  /* Declaração dos métodos da classe. */

  public void metodo1() {
    //Comandos
    System.out.println("Chamando o metodo 1.");
  }

  public void metodo2() {
    //Comandos
    System.out.println("Chamando o metodo 2.");
  }
}
{% endhighlight %}

Note que utilizamos a palavra-chave **class** seguida do nome da classe **NovaClasse**. A palavra-chave **public** informa que esta classe pode ser utilizada por qualquer outra classe dentro do projeto.

Este trecho de código precisa ser salvo em um arquivo com o nome NovaClasse.java, porque o nome do arquivo .java precisa ter o mesmo nome da classe pública.

Logo após a declaração da classe, criamos 3 atributos. Os atributos podem ser criados em qualquer lugar do programa. Também criamos dois métodos para esta classe, mais adiante vamos discutir como funcionam os métodos.

Essa mesma classe pode ser representada em UML da seguinte forma:

<figure>
    <a href="/images/2020-05-09-java-exemplo-classe-uml.png"><img src="/images/2020-05-09-java-exemplo-classe-uml.png" alt="Exemplo de uma classe em UML."></a>
</figure>