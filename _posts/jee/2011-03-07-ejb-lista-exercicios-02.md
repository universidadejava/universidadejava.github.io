---
layout: article
title: "EJB - Lista de exercícios 02"
categories: jee
author: sakurai
date: 2011-03-07 16:11:00
tags: [javaee, ejb]
published: true
excerpt: EJB na prática - lista de exercícios 02.
comments: true
image:
  teaser: teaser-ejb.png
ads: false
---

A melhor forma de aprender EJB é praticando, então segue alguns exercícios de EJB.

## Exercício 1

O restaurante UniversidadeJavaFood tem um cardápio, dentro do cardápio existem diversos itens como, por exemplo: entradas, pratos quentes, bebidas, sobremesas, etc. Para cada item eu tenho as informações nome, descrição, preço.

Quando o cliente vem ao restaurante ele pode fazer o pedido de um ou muitos itens do cardápio, enquanto o pedido não estiver pronto, o cliente pode cancelá-lo a qualquer momento.

OBS: Não é necessário representar o cardápio. Os itens do cardápio podem ser previamente carregados na base de dados.

Crie as classes de negocio para representar o pedido com os itens do cardápio, adicione as anotações de JPA.

Crie uma classe DAO onde temos as operações de fazer pedido ou cancelamento.

Crie um EJB que possui a lógica de negocio para fazer o pedido ou cancelamento de algum item.


## Conteúdos relacionados

- [Interceptando os EJBs para criar objetos DAOs](http://www.universidadejava.com.br/jee/ejb-interceptando/)
- [Criando um HelloWorld com EJB 3.0 na IDE NetBeans](http://www.universidadejava.com.br/jee/ejb-helloworld-netbeans/)
- [Criando uma aplicação com EJB + JPA](http://www.universidadejava.com.br/jee/criando-aplicacao-ejb-jpa/)
- [Web Services com EJB 3.0](http://www.universidadejava.com.br/jee/webservice-com-ejb/)