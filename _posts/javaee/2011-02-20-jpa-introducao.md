---
layout: article
title: "JPA - Introdução"
categories: javaee
author: sakurai
date: 2011-02-20 05:40:00
tags: [javaee, jpa]
published: true
excerpt: Introdução a Java Persistence API.
comments: true
image:
  teaser: teaser-jpa.png
ads: true
---

Java Persistence API é um framework para camada de persistência dos dados, pode ser utilizado no contexto Java EE (Java Enterprise Edition) e Java SE (Java Standard Edition). Utilizando este framework temos uma facilidade maior para desenvolver a camada de comunicação entre o Java e o banco de dados.

<iframe width="420" height="315" src="https://www.youtube.com/embed/N4qNeKBDzy0" frameborder="0" allowfullscreen></iframe>

Algumas das facilidades que temos ao utilizar JPA são:

* Conversão de registros do banco de dados em objetos Java.
* Não precisa criar códigos SQL para salvar, alterar ou remover registros do banco de dados.
* A aplicação não fica presa a um banco de dados, podemos trocá-los sem muito problema.

<figure>
    <a href="/images/2011-02-20-jpa-introducao-01.png"><img src="/images/2011-02-20-jpa-introducao-01.png" alt="JPA"></a>
</figure>

Também aumenta a produtividade dos desenvolvedores que utilizam o banco de dados, deixando de forma transparente a utilização do banco de dados, principalmente por não deixar a programação Java presa a um tipo especifico de banco de dados.

Para que possamos trazer informações do banco de dados e converte-las em classes Java, acaba sendo um pouco trabalhoso, pois temos que verificar campo a campo com qual atributo ele corresponde, às vezes é necessário fazer uma conversão do tipo de dado declarado no banco de dados com o tipo de dado utilizado na classe. O mesmo processo ocorre quando temos objetos Java na memória e queremos guardar suas informações no banco de dados.

O JPA utiliza o conceito de mapeamento objeto/relacional (ORM – Object / Relational Mapping) para fazer um mapeamento entre a base de dados relacional e os objetos Java, ou seja, o próprio framework faz o relacionamento entre os atributos das classes Java com a tabela do banco de dados.

<figure>
    <a href="/images/2011-02-20-jpa-introducao-02.png"><img src="/images/2011-02-20-jpa-introducao-02.png" alt="Mapeamento objeto / relacional"></a>
</figure>

No exemplo acima o JPA irá gerar uma instância da classe Produto, para cada linha da tabela Produto, e também atribui os valores das propriedades da classe Produto de acordo com os valores das colunas da tabela.

<figure>
    <a href="/images/2011-02-20-jpa-introducao-03.png"><img src="/images/2011-02-20-jpa-introducao-03.png" alt="Mapeamento objeto / relacional"></a>
</figure>
