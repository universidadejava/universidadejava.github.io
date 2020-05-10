---
layout: article
title: "Java - Declarando constantes"
categories: java
author: sakurai
date: 2011-06-16 17:34:00
tags: [java]
published: true
excerpt: Entenda como declarar e usar uma variável constante no Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

Assim como outras linguagens de programação, em Java também é possível se trabalhar com constantes. Para tanto, basta utilizar o modificador de acesso **final**.

Dependendo do escopo e da utilização desta sua variável, é possível combina-la com outros modificadores de acesso, por exemplo. Uma vez que a variável foi declarada como uma constante, é obrigatório a atribuição de um valor inicial para a mesma.

Observe no exemplo a seguir que tanto um atributo como uma variável interna a um método podem ser declarados como constantes.

{% gist 484fdcdc3fe2ae8a525d ExemploConstantes.java %}

Tendo em vista que tais variáveis foram declaradas como constantes, seu valor não pode ser alterado após a sua declaração. Observe que a tentativa de atribuir um valor a qualquer uma destas variáveis irá gerar um erro de compilação.

{% gist 484fdcdc3fe2ae8a525d ExemploConstantes2.java %}
