---
layout: article
title: "Java Virtual Machine"
categories: java
author: sakurai
date: 2011-06-13 18:33:00
tags: [java, jvm]
published: true
excerpt: Introdução sobre a JVM.
comments: true
image:
  teaser: 2011-06-13-teaser-introducao-jvm.png
ads: false
---

A *Java Virtual Machine* é uma maquina imaginaria que é implementada através da  emulação em software, existe uma JVM diferente para cada Sistema Operacional (SO) e uma vez que sua aplicação é criada, a mesma pode ser executada em diversos SO através da JVM sem ter que ser recompilado.

O código que é executado pela JVM são os **bytecodes**, quando um arquivo **.java** é criado, o mesmo precisa ser compilado para **.class** essa compilação converte os códigos Java para bytecodes e a JVM se encarrega de executar os bytecodes e fazer a integração com o SO.

Fases da programação Java:

<figure>
    <a href="/images/2011-06-13-introducao-jvm-01.png"><img src="/images/2011-06-13-introducao-jvm-01.png" alt="Fases da programação Java."></a>
</figure>
