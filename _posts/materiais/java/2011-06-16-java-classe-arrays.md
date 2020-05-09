---
layout: article
title: "Java - Classe java.util.Arrays"
categories: java
author: sakurai
date: 2011-06-16 21:34:00
tags: [java]
published: true
excerpt: A classe Arrays possui uma série de métodos estáticos que nos ajudam a trabalhar mais facilmente com vetores.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

Dentro do pacote java.util encontramos uma classe chamada **Arrays**. Esta classe possui uma série de métodos estáticos que nos ajudam a trabalhar mais facilmente com vetores. Dentre seus principais métodos podemos evidenciar os seguintes:

## binarySearch

Este método recebe sempre 2 parâmetros sendo um deles o vetor e outro o elemento que se deseja buscar dentro dele e utiliza o algoritmo da busca binária para localizar o elemento dentro do vetor.

## sort

Realizar a ordenação de um vetor utilizando um algoritmo do tipo Quick Sort. Este tipo de algoritmo também será discutido mais a diante. Por este método receber o vetor por parâmetro (que lembrando, vetores são objetos) ele o ordena e não retorna valor algum.

## asList

Converte o vetor em uma coleção do tipo lista.

## copyOf

Cria uma cópia de um vetor. Pode-se copiar o vetor completamente ou apenas parte dele.
