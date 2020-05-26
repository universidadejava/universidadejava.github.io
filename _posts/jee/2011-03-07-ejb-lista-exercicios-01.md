---
layout: article
title: "EJB - Lista de exercícios 01"
categories: jee
author: sakurai
date: 2011-03-07 16:09:00
tags: [javaee, ejb]
published: true
excerpt: EJB na prática - lista de exercícios 01.
comments: true
image:
  teaser: teaser-ejb.png
ads: false
---

A melhor forma de aprender EJB é praticando, então segue alguns exercícios de EJB.

## Exercício 1

Crie um projeto EJB chamado **CalculadoraEJB**, dentro dele crie o seguinte componente:
    - Crie um componente EJB para realizar as operações de uma calculadora (soma, subtração, divisão e multiplicação).

Crie um projeto Java chamado CalculadoraCliente que dever ter uma interface Swing para utilizar as operações do Calculadora EJB.


## Exercício 2

Crie um projeto EJB chamado **LivrariaEjb**, dentro dele crie o seguinte componente EJB:
    - Crie um EJB para gerenciar (salvar, alterar, consultar (por autor, titulo ou isbn) e excluir) livros.
    - Também crie um método para efetuar a venda de livros.

Crie um projeto chamado Livraria para testar o EJB criado, este projeto pode ser Console, Swing ou Web (utilizando Servlet).

OBS: Se a aplicação de teste for Console ou Desktop, utilize o Service Locator para separar a lógica que busca os EJBs.
