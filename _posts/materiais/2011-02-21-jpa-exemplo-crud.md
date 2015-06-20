---
layout: article
title: "JPA - Exemplo de DAO (CRUD)"
categories: materiais
author: sakurai
date: 2011-02-21 08:53:00
tags: [javaee, jpa]
published: true
excerpt: Exemplo de DAO (CRUD) utilizando JPA.
comments: true
image:
  teaser: teaser-jpa.png
ads: true
---

Nesse exemplo vamos salvar, alterar, consultar por id e apagar um registro de pessoa, utilizaremos o banco de dados MySQL.

> Lembre de adicionar as bibliotecas do Hibernate e driver do MySQL.

Vamos criar uma Entity para representar a tabela Pessoa abaixo:

{% highlight sql %}
CREATE DATABASE exemplos;

USE exemplos;

CREATE TABLE Pessoa (
  id INT NOT NULL AUTO_INCREMENT,
  nome VARCHAR(100) NOT NULL,
  dataNasc DATE NOT NULL,
  email VARCHAR(150),
  primary key(id)
);
{% endhighlight %}

Criando a entity para representar a tabela Pessoa:

{% highlight java %}
package br.universidadejava.jpa.exemplo1.modelo;

import java.util.Date;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

/**
 * Classe utilizada para representar uma pessoa.
 */
@Entity
public class Pessoa {
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  @Column(nullable = false)
  private String nome;
  @Temporal(TemporalType.DATE)
  @Column(name = "dataNasc", nullable = false)
  private Date dataNascimento;
  private String email;

  public Date getDataNascimento() {
    return dataNascimento;
  }
  public void setDataNascimento(Date dataNascimento) {
    this.dataNascimento = dataNascimento;
  }
  public String getEmail() {
    return email;
  }
  public void setEmail(String email) {
    this.email = email;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public String getNome() {
    return nome;
  }
  public void setNome(String nome) {
    this.nome = nome;
  }
}
{% endhighlight %}

Vamos criar o arquivo persistence.xml .

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">
  <persistence-unit name="ExemplosJPAPU" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <class>br.universidadejava.jpa.exemplo1.modelo.Pessoa</class>
    <properties>
      <property name="hibernate.connection.username" value="usuario"/>
      <property name="hibernate.connection.password" value="senha"/>
      <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver"/>
      <property name="hibernate.connection.url" value="jdbc:mysql://localhost:3306/exemplos"/>
      <property name="hibernate.cache.provider_class" value="org.hibernate.cache.NoCacheProvider"/>
    </properties>
  </persistence-unit>
</persistence>
{% endhighlight %}

Vamos criar uma classe PessoaDAO que possui os métodos para manipular (salvar, atualizar, apagar e consultar por id) um objeto Pessoa.

{% highlight java %}
package br.universidadejava.jpa.exemplo1.dao;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import br.universidadejava.jpa.exemplo.modelo.Pessoa;

/**
 * Classe utilizada para fazer realizar as operações de banco de dados sobre a entity Pessoa.
 */
public class PessoaDAO {
  /**
   * Método utilizado para obter o entity manager.
   * @return
   */
  private EntityManager getEntityManager() {
    EntityManagerFactory factory = null;
    EntityManager entityManager = null;
    try {
      //Obtém o factory a partir da unidade de persistência.
      factory = Persistence.createEntityManagerFactory("ExemplosJPAPU");
      //Cria um entity manager.
      entityManager = factory.createEntityManager();
      //Fecha o factory para liberar os recursos utilizado.
    } finally {
      factory.close();
    }
    return entityManager;
  }

  /**
   * Método utilizado para salvar ou atualizar as informações de uma pessoa.
   * @param pessoa
   * @return
   * @throws java.lang.Exception
   */
  public Pessoa salvar(Pessoa pessoa) throws Exception {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      System.out.println("Salvando a pessoa.");
      // Verifica se a pessoa ainda não está salva no banco de dados.
      if(pessoa.getId() == null) {
        //Salva os dados da pessoa.
        entityManager.persist(pessoa);
      } else {
        //Atualiza os dados da pessoa.
        pessoa = entityManager.merge(pessoa);
      }
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } finally {
      entityManager.close();
    }
    return pessoa;
  }

  /**
   * Método que apaga a pessoa do banco de dados.
   * @param id
   */
  public void excluir(Long id) {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      // Consulta a pessoa na base de dados através do seu ID.
      Pessoa pessoa = entityManager.find(Pessoa.class, id);
      System.out.println("Excluindo os dados de: " + pessoa.getNome());
      // Remove a pessoa da base de dados.
      entityManager.remove(pessoa);
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } finally {
      entityManager.close();
    }
  }

  /**
   * Consulta o pessoa pelo ID.
   * @param id
   * @return o objeto Pessoa.
   */
  public Pessoa consultarPorId(Long id) {
    EntityManager entityManager = getEntityManager();
    Pessoa pessoa = null;
    try {
      //Consulta uma pessoa pelo seu ID.
      pessoa = entityManager.find(Pessoa.class, id);
    } finally {
      entityManager.close();
    }
    return pessoa;
  }
}
{% endhighlight %}

O método **salvar** recebe o objeto Pessoa que será salvo, neste exemplo usaremos este método salvar uma nova pessoa ou atualizar os dados de uma nova pessoa.

Mas como sabemos quando temos que salvar e quando tem que atualizar, basta olhar o atributo id da classe Pessoa, se o **id** for null significa que é um novo objeto que ainda não foi salvo no banco de dados, então utilizaremos o método **persist** da EntityManager para salvá-lo, caso o id tenha algum valor então significa que o objeto já foi salvo anteriormente portanto ele deve ser atualizado então utilizaremos o método **merge** da EntityManager para atualiza-lo.

Note que como vamos salvar ou atualizar os dados, precisamos criar uma transação, com o método **getTransaction()** do EntityManager obtemos um objeto EntityTransaction com ele podemos iniciar a transação através do método **begin()**, finalizar a transação com sucesso através do método **commit()** ou desfazer as alterações em caso de erro com o método **rolback()**. Este mesmo conceito de transação será utilizado no método excluir.

O método **excluir** não precisa receber todos os dados da Pessoa, recebendo apenas o seu ID através do parametro **Long id**, podemos utilizar o método **find** do EntityManager para consultar os dados da Pessoa, depois com o objeto Pessoa consultado podemos usar o método **remove** do EntityManager para apagar os dados da Pessoa.

O método **consultarPorId** recebe um objeto Long chamado **id**, com o ID da tabela Pessoa, utilizando o método **find** do EntityManager passamos a classe da entidade **Pessoa.class** e seu **id** para que possamos consultar os dados da Pessoa.

Vamos criar uma classe PessoaDAOTeste para testarmos os métodos da classe PessoaDAO:

{% highlight java %}
package br.universidadejava.jpa.exemplo1.teste;

import java.util.Calendar;
import java.util.GregorianCalendar;
import br.universidadejava.jpa.exemplo1.dao.PessoaDAO;
import br.universidadejava.jpa.exemplo1.modelo.Pessoa;

/**
 * Classe utilizada para testar os métodos do PessoaDAO.
 */
public class PessoaDAOTeste {
  public static void main(String[] args) throws Exception {
    Pessoa pessoa = new Pessoa();
    pessoa.setNome("Rafael Sakurai");
    Calendar data = new GregorianCalendar();
    data.set(Calendar.YEAR, 1983);
    data.set(Calendar.MONTH, 11);
    data.set(Calendar.DAY_OF_MONTH, 26);
    pessoa.setDataNascimento(data.getTime());
    pessoa.setEmail("rafael.sakurai@metodista.br");

    PessoaDAO dao = new PessoaDAO();
    System.out.println("Salvando a pessoa: " + pessoa.getNome());
    pessoa = dao.salvar(pessoa);

    pessoa.setNome("Rafael Guimarães Sakurai");
    pessoa = dao.salvar(pessoa);
    System.out.println("Alterando a pessoa: " + pessoa.getNome());

    Pessoa pessoa2 = dao.consultarPorId(pessoa.getId());
    System.out.println("Consultando: " + pessoa2.getNome());

    System.out.println("Removendo a pessoa: " + pessoa.getId());
    dao.excluir(pessoa.getId());
  }
}
{% endhighlight %}

Neste teste vamos criar um objeto pessoa e salva-lo, depois vamos altera o nome da pessoa, vamos consultar a pessoa pelo id e no final vamos apagar o registro da pessoa.
