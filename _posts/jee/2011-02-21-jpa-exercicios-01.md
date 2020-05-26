---
layout: article
title: "JPA - Lista de exercícios 01"
categories: jee
author: sakurai
date: 2011-02-21 10:53:00
tags: [javaee, jpa]
published: true
excerpt: Exercícios de JPA para praticar.
comments: true
image:
  teaser: teaser-jpa.png
ads: false
---

## Exercício 1

Neste exercício vamos abordar como criar uma aplicação CRUD (salvar, alterar, consultar e excluir) do Livro utilizando o Java Persistence API.

Crie o seguinte banco de dados e a tabela de Livro:

{% highlight sql %}
CREATE DATABASE jpaexercicio1;

USE jpaexercicio1;

CREATE TABLE Livro (
  id int NOT NULL AUTO_INCREMENT,
  titulo varchar(200) NOT NULL,
  autor varchar(200) NOT NULL,
  isbn varchar(50) NOT NULL,
  paginas int NOT NULL,
  preco double(10,2) NOT NULL,
  primary key(id)
);
{% endhighlight %}

Crie um Projeto Java chamado ExercicioJPA1, adicione as bibliotecas Hibernate JPA e MySQL JDBC Driver e crie:

Dado a seguinte classe para representar a tabela Livro, adicione as anotações do JPA necessário.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.modelo;

import java.io.Serializable;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

public class Livro implements Serializable {
  private static final long serialVersionUID = 2405106626392673061L;

  private Long id;
  private String titulo;
  private String autor;
  private String isbn;
  private Integer paginas;
  private Double preco;

  public String getAutor() {
    return autor;
  }
  public void setAutor(String autor) {
    this.autor = autor;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public String getIsbn() {
    return isbn;
   }
  public void setIsbn(String isbn) {
    this.isbn = isbn;
  }
  public Integer getPaginas() {
    return paginas;
  }
  public void setPaginas(Integer paginas) {
    this.paginas = paginas;
  }
  public Double getPreco() {
    return preco;
  }
  public void setPreco(Double preco) {
    this.preco = preco;
  }
  public String getTitulo() {
    return titulo;
  }
  public void setTitulo(String titulo) {
    this.titulo = titulo;
  }
}
{% endhighlight %}

Crie uma classe LivroDAO com os seguintes métodos:

* getEntityManager() – que cria um EntityManager
* salvar() – chame o método do EntityManager que realize a operação de salvar ou alterar um livro.
* consultarPorId() – chame o método do EntityManager que realize a operação de consultar passando o atributo da chave primaria.
* excluir() – chame o método do EntityManager que realize a operação de remover um livro.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.dao;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import br.universidadejava.jpa.exercicio1.modelo.Livro;

public class LivroDAO {
  /**
   * Método utilizado para obter o entity manager.
   * @return
   */
  private EntityManager getEntityManager() {
    //TODO - Adicionar código para criar uma EntityManager, utilize a unidade de persistência criada no próximo passo.
  }

  /**
   * Método utilizado para salvar ou atualizar as informações de um livro.
   * @param livro
   * @return
   * @throws java.lang.Exception
   */
  public Livro salvar(Livro livro) throws Exception {
    //TODO - Adicionar código para montar o método que salva um Livro.
  }

  /**
   * Método que exclui o livro do banco de dados.
   * @param id
   */
  public void excluir(Long id) {
    //TODO - Adicionar código para montar o método que remove um Livro a partir do seu ID.
  }

  /**
   * Consulta o livro pelo ID.
   * @param id
   * @return
   */
  public Livro consultarPorId(Long id) {
    //TODO - Adicionar código para montar o método que consulta o Livro por ID.
  }
}
{% endhighlight %}

Vamos criar um arquivo persistence.xml dentro da pasta META-INF para guardar as configurações do banco de dados. Neste arquivo também vamos informar a Entity Livro.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

  <persistence-unit name="ExercicioJPA1PU" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <class>br.universidadejava.jpa.exercicio1.modelo.Livro</class>
    <properties>
      <property name="hibernate.connection.username" value="usuario"/>
      <property name="hibernate.connection.password" value="senha"/>
      <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver"/>
      <property name="hibernate.connection.url" value="jdbc:mysql://localhost:3306/jpaexercicio1"/>
      <property name="hibernate.cache.provider_class" value="org.hibernate.cache.NoCacheProvider"/>
      <property name="hibernate.show_sql" value="true"/>
    </properties>
  </persistence-unit>
</persistence>
{% endhighlight %}

Para testar as operações sobre a entidade Livro, vamos criar uma classe LivroTeste onde utilizamos o LivroDAO que fará uso da EntityManager para gerenciar a entity Livro.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.teste;

import br.universidadejava.jpa.exercicio1.dao.LivroDAO;
import br.universidadejava.jpa.exercicio1.modelo.Livro;

public class LivroTeste {
  public static void main(String[] args) {
    try {
      //Montar um objeto Livro.
      Livro livro = new Livro();
      livro.setAutor("Rafael Guimarães Sakurai");
      livro.setIsbn("111-11-1111-111-1");
      livro.setPaginas(439);
      livro.setPreco(30.90);
      livro.setTitulo("Guia de estudos para certificação SCJA.");

      LivroDAO dao = new LivroDAO();
      //Chama o método do DAO que salva o Livro no banco de dados.
      livro = dao.salvar(livro);
      System.out.println("ID do livro salvo: " + livro.getId());

      /*
       * TODO - Teste a consulta, alteração e exclusão do livro.
       */

    } catch (Exception ex) {
      ex.printStackTrace();
    }
  }
}
{% endhighlight %}

## Exercício 2

Crie uma aplicação Swing ou Console utilizando JPA para fazer o CRUD (salvar, alterar, consultar e excluir) da tabela Produto a seguir:

{% highlight sql %}
CREATE TABLE Produto (
  id int NOT NULL auto_increment,
  nome varchar(200) NOT NULL,
  preco double(10,2) NOT NULL,
  dataValidade date,
  qtdEstoque int,
  primary key(id)
);
{% endhighlight %}
