---
layout: article
title: "JPA - Introdução"
categories: materiais
author: sakurai
date: 2011-02-20 05:40:00
tags: [java, javaee, jpa, banco de dados]
published: true
excerpt: Introdução a Java Persistence API.
comments: true
image:
  teaser: teaser-jpa.png
ads: false
---

O Java Persistence API é um framework para camada de persistência dos dados, que fornece uma camada de comunicação entre a aplicação escrita em Java e o banco de dados.

<iframe width="420" height="315" src="https://www.youtube.com/embed/N4qNeKBDzy0" frameborder="0" allowfullscreen></iframe>

Algumas facilidades que o JPA oferece são:

* Conversão de registros do banco de dados em objetos Java;
* Não precisa criar códigos SQL para salvar, alterar ou remover registros do banco de dados;
* A aplicação não fica presa a um banco de dados sendo simples a troca.

<figure>
    <a href="/images/2011-02-20-jpa-introducao-01.png"><img src="/images/2011-02-20-jpa-introducao-01.png" alt="JPA"></a>
</figure>

Também aumenta a produtividade dos desenvolvedores que utilizam o banco de dados, deixando de forma transparente a sua utilização, principalmente por não deixar a programação Java vinculada a um tipo específico de banco de dados.

Para trazer as informações do banco de dados e convertê-las em classes Java, acaba sendo um pouco trabalhoso. Quando é usado [JDBC](http://www.universidadejava.com.br/java/java-jdbc/) puro há a necessidade de realizar o mapeamento entre os atributos e colunas do banco de dados, às vezes é necessário fazer uma conversão do tipo de dado declarado no banco de dados com o tipo de dado utilizado na classe. O mesmo processo ocorre quando os objetos Java são salvos no banco de dados.

O JPA utiliza o conceito de **mapeamento objeto / relacional (ORM – Object / Relational Mapping)** para fazer ponte entre a base de dados relacional e os objetos Java. A figura a seguir mostra o próprio framework faz o relacionamento entre os atributos das classes Java com a tabela do banco de dados.

<figure>
    <a href="/images/2011-02-20-jpa-introducao-02.png"><img src="/images/2011-02-20-jpa-introducao-02.png" alt="Mapeamento objeto / relacional"></a>
</figure>

O JPA cria uma instância da classe Produto para cada linha da tabela Produto, como mostrado na figura a seguir, e também atribui os valores das propriedades da classe Produto de acordo com os valores das colunas da tabela. Por padrão o JPA realizará o mapeamento da classe e atributos com o mesmo nome.

<figure>
    <a href="/images/2011-02-20-jpa-introducao-03.png"><img src="/images/2011-02-20-jpa-introducao-03.png" alt="Mapeamento objeto / relacional"></a>
</figure>


### Conteúdos relacionados

- [Criando entidades com JPA](http://www.universidadejava.com.br/jee/jpa-entity/)
- [Exemplo de CRUD com JPA](http://www.universidadejava.com.br/jee/jpa-exemplo-crud/)
- [Criando consultas com JPA](http://www.universidadejava.com.br/jee/jpa-query/)
- [Adicionando relaciomento de Um para Um entre entidades](http://www.universidadejava.com.br/jee/jpa-onetoone/)