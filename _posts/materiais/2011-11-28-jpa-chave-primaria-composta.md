---
layout: article
title: "JPA 2.0 - Especificar chave primaria composta"
categories: materiais
author: sakurai
date: 2011-11-28 09:30:00
tags: [javaee, jpa]
published: true
excerpt: Usando chave primaria composta no JPA.
comments: true
image:
  teaser: 2011-11-28-teaser-jpa-chave-primaria-composta.png
ads: true
---

A chave composta define que vários atributos serão utilizados para definir a chave de uma entidade, com isso, acabamos tendo uma restrição onde os atributos da chave composta não podem ser repetidos.

No JPA quando precisamos definir uma chave composta precisamos criar uma classe separada apenas com os atributos que fazem parte da chave composta e precisamos utilizar a anotaçãojavax.persistence.Embeddable.

Neste exemplo vamos criar uma entidade Telefone que possui uma chave composta pelos atributos DDD e número do telefone, pois não pode ter o mesmo número de telefone para mais de 1 cliente.

Para declarar a chave composta vamos criar uma classe chamada TelefonePK com os atributos DDD e numero:

{% highlight java %}
package br.universidadejava.jpa.exemplo.chave.composta.modelo;

import java.io.Serializable;
import javax.persistence.Embeddable;

/**
 * Esta classe representa a composição da chave do telefone, está chave é composta por DDD + numero do telefone.
 */
@Embeddable
public class TelefonePK implements Serializable {
  private static final long serialVersionUID = -637018809489152388L;

  private Short ddd;
  private String numero;

  public Short getDdd() { return ddd; }
  public void setDdd(Short ddd) { this.ddd = ddd; }
  public String getNumero() { return numero; }
  public void setNumero(String numero) { this.numero = numero; }

  @Override
  public String toString() {
    return "("+ getDdd() + ") " + getNumero();
  }
}
{% endhighlight %}

OBS: Sobrescrevi o método toString() para imprimir de forma mais amigável a chave composta.

Note que adicionamos a anotação @Embeddable na classe TelefonePK para informar que está classe será adicionado em outra entidade.

Para adicionarmos a chave composta na entidade, vamos criar um atributo do tipo da classe que possui a anotação @Embeddable e vamos adicionar a anotação javax.persistence.EmbeddedId neste atributo.

Na entidade Telefone vamos declarar um atributo chamado id do tipo TelefonePK para representar a chave composta:

{% highlight java %}
package br.universidadejava.jpa.exemplo.chave.composta.modelo;

import java.io.Serializable;
import javax.persistence.EmbeddedId;
import javax.persistence.Entity;

/**
 * Classe utilizada para representar o telefone de um cliente.
 * Nesta classe abordamos o uso da chave composta através da anotação @EmbeddedId.
 */
@Entity
public class Telefone implements Serializable {
  private static final long serialVersionUID = 5999236902534007386L;

  @EmbeddedId
  private TelefonePK id;
  private String cliente;

  public String getCliente() { return cliente; }
  public void setCliente(String cliente) { this.cliente = cliente; }
  public TelefonePK getId() { return id; }
  public void setId(TelefonePK id) { this.id = id; }
}
{% endhighlight %}

Exemplo de um CRUD utilizando a chave composta, a classe TelefoneDAO possui as operações de Salvar, Alterar, Consultar por chave composta e Apagar o telefone:

{% highlight java %}
package br.universidadejava.jpa.exemplo.chave.composta.dao;

import javax.persistence.EntityExistsException;
import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import br.universidadejava.jpa.exemplo.chave.composta.modelo.Telefone;
import br.universidadejava.jpa.exemplo.chave.composta.modelo.TelefonePK;

/**
 * Classe utilizada para testar as operações Salvar, Altera, Consultar por Id e Apagar o registro de um telefone.
 */
public class TelefoneDAO {
  public EntityManager getEntityManager() {
    EntityManagerFactory factory = null;
    EntityManager entityManager = null;
    try {
      factory = Persistence.createEntityManagerFactory("ExemplosJPAPU");
      entityManager = factory.createEntityManager();
    } finally {
      factory.close();
    }
    return entityManager;
  }

  public Telefone consultarPorId(TelefonePK id) {
    EntityManager entityManager = getEntityManager();
    Telefone telefone = null;
    try {
      telefone = entityManager.find(Telefone.class, id);
    } finally {
      entityManager.close();
    }
    return telefone;
  }

  public Telefone salvar(Telefone telefone) throws Exception {
    EntityManager entityManager = getEntityManager();
    try {
      entityManager.getTransaction().begin();
      entityManager.persist(telefone);
      entityManager.flush();
      entityManager.getTransaction().commit();
      /*Esta exceção pode ser lançada caso já exista um registro com a mesma chave composta. */
    } catch (EntityExistsException ex) {
      entityManager.getTransaction().rollback();
      throw new Exception("Este telefone já está registrado para outro cliente.");
    } catch (Exception ex) {
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
    return telefone;
  }

  public Telefone atualizar(Telefone telefone) throws Exception {
    EntityManager entityManager = getEntityManager();
    try {
      entityManager.getTransaction().begin();
      entityManager.merge(telefone);
      entityManager.flush();
      entityManager.getTransaction().commit();
    } catch (Exception ex) {
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
    return telefone;
  }

  public void apagar(TelefonePK id) {
    EntityManager entityManager = getEntityManager();
    try {
      entityManager.getTransaction().begin();
      Telefone telefone = entityManager.find(Telefone.class, id);
      entityManager.remove(telefone);
      entityManager.flush();
      entityManager.getTransaction().commit();
    } catch (Exception ex) {
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
  }
}
{% endhighlight %}

No método consultarPorId precisamos agora passar um atributo da chave composta TelefonePK para que possamos localizar um telefone.

No método salvar vamos receber o objeto Telefone que será salvo, note que estamos tratando a exceção javax.persistence.EntityExitsException que será lançada caso tentamos salvar duas entidades com o mesmo id, neste caso com o mesmo DDD e número de telefone.

Criamos agora um método atualizar separado apenas para atualizar o telefone, pois podemos criar um Telefone e alterar o nome do cliente, mas não podemos criar dois telefones com o mesmo DDD e número de telefone.

No método apagar precisamos receber um objeto que representa a chave composta TelefonePK, desta forma podemos localizar o objeto Telefone para que possamos apagá-lo.

Nesta classe TelefoneDAOTeste vamos testar todas as operações da classe TelefoneDAO declarada acima:

{% highlight java %}
package br.universidadejava.jpa.exemplo.chave.composta.dao;

import br.universidadejava.jpa.exemplo.chave.composta.modelo.Telefone;
import br.universidadejava.jpa.exemplo.chave.composta.modelo.TelefonePK;

/**
 * Classe utilizada para testar as operações da classe TelefoneDAO.
 */
public class TelefoneDAOTeste {
  public static void main(String[] args) throws Exception {
    /* Cria uma chave composta. */
    TelefonePK pk = new TelefonePK();
    pk.setDdd((short) 11);
    pk.setNumero("1111-1111");

    /* Cria um telefone. */
    Telefone tel = new Telefone();
    tel.setId(pk);
    tel.setCliente("Sakurai");

    TelefoneDAO dao = new TelefoneDAO();
    System.out.println("Salvando o telefone: " + pk);
    dao.salvar(tel);

    System.out.println("Consultando o telefone: " + pk);
    Telefone tel2 = dao.consultarPorId(pk);
    System.out.println("Cliente " + tel2.getCliente());

    try {
      System.out.println("Tentando salvar o mesmo número de telefone para outro cliente.");
      Telefone tel3 = new Telefone();
      tel3.setId(pk);
      tel3.setCliente("Rafael");
      dao.salvar(tel3);
    } catch (Exception ex) {
      System.out.println(ex.getMessage());
    }

    System.out.println("Alterando o cliente:");
    tel.setCliente("Rafael");
    dao.atualizar(tel);

    System.out.println("Apagando o registro do telefone:");
    dao.apagar(pk);
  }
}
{% endhighlight %}
