---
layout: article
title: "Java - Tipos primitivos"
categories: java
author: sakurai
date: 2011-06-15 15:14:00
tags: [java]
published: true
excerpt: Conheça os tipos primitivos do Java.
comments: true
image:
  teaser: 2011-06-15-teaser-java-tipos-primitivos.png
ads: false
---

## Variáveis

Uma variável é um objeto normalmente localizado na memória utilizado para representar valores, quando declaramos uma variável estamos associando seu nome (identificador) ao local da memória onde está armazenado sua informação, as variáveis em Java podem ser do tipo **primitivo** ou **objeto**:

* **Variáveis primitivas**: podem ser do tipo byte, short, int, long, float, double, char ou boolean.

* **Variáveis de referência**: usada para referenciar um objeto. Quando usamos uma variável de referência definimos qual o tipo do objeto ou um subtipo do tipo do objeto (veremos isso mais para frente).

Quando declaramos uma variável primitiva, estamos associando está a um espaço na memória que vai guardar o seu valor. No caso da variável de referência, podemos tê-la apontando para algum lugar vazio **(null)** ou para algum espaço da memória que guarda os dados de um objeto.

As variáveis primitivas e de referência são guardadas em locais diferentes da memória. Variáveis primitivas ficam em um local chamado **stack** e as variáveis de referência ficam em um local chamado **heap**.


## Tipos primitivos

A linguagem Java não é totalmente Orientada a Objetos, e isto se deve principalmente aos atributos do tipo primitivo, pois são tipos de dados que não representam classes, mas sim valores básicos.

Os tipos primitivos, assim como em várias outras linguagens tais como o C, existem para representar os tipos mais simples de dados, sendo eles dados **numérico**, **booleano** e **caractere**. Os tipos primitivos da linguagem Java são:

<figure>
    <a href="/images/2011-06-15-java-tipos-primitivos-01.png"><img src="/images/2011-06-15-java-tipos-primitivos-01.png" alt="Tipos primitivos do Java."></a>
</figure>
