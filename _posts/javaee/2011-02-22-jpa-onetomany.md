---
layout: article
title: "JPA - OneToMany"
categories: javaee
author: sakurai
date: 2011-02-22 08:06:00
tags: [javaee, jpa]
published: true
excerpt: Veja como especificar o relacionamento de Um-para-Muitos entre as entidades.
comments: true
image:
  teaser: 2011-02-22-teaser-jpa-onetomany.png
ads: false
---

## Relacionamento Um-para-Muitos

Este relacionamento informa que o registro de uma entidade está relacionado com vários registros de outra entidade.

<iframe width="420" height="315" src="https://www.youtube.com/embed/B5wArXmXy9M" frameborder="0" allowfullscreen></iframe>

Exemplo de relacionamento Um-para-Muitos unidirecional, no qual uma Aula pode ter várias Tarefas:

Script do banco de dados:

{% highlight sql %}
CREATE TABLE Aula (
  id int NOT NULL auto_increment,
  titulo varchar(45),
  data date,
  PRIMARY KEY (id)
);

CREATE TABLE Tarefa (
  id int NOT NULL auto_increment,
  titulo varchar(45) NOT NULL,
  descricao varchar(45) NOT NULL,
  aula_id int,
  PRIMARY KEY (id)
);
{% endhighlight %}

Modelo UML:

<figure>
    <a href="/images/2011-02-22-jpa-onetomany-01.png"><img src="/images/2011-02-22-jpa-onetomany-01.png" alt="Exemplo de relacionamento @OneToMany."></a>
</figure>

Neste exemplo definimos que uma **Aula** possui uma lista de **Tarefa**, ou seja, a aula pode não ter tarefa, pode ter apenas uma tarefa ou pode ter varias tarefas, uma Tarefa não precisa saber de qual Aula ela está associada, portanto temos um relacionamento **unidirecional**.

Código fonte das classes com o relacionamento:

Na classe Aula utilizamos a anotação **javax.persistence.OneToMany** no atributo **tarefas**, para informar que uma Aula está associada com várias tarefas.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.Date;
import java.util.List;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.OneToMany;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

/**
 * Classe utilizada para representar uma aula.
 */
@Entity
public class Aula implements Serializable {
  private static final long serialVersionUID = -6745032908099856302L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String titulo;
  @Temporal(TemporalType.DATE)
  private Date data;
  @OneToMany(cascade = CascadeType.ALL)
  @JoinColumn(name="aula_id")
  private List<Tarefa> tarefas;

  public Date getData() {
    return data;
  }
  public void setData(Date data) {
    this.data = data;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public String getTitulo() {
    return titulo;
  }
  public void setTitulo(String titulo) {
    this.titulo = titulo;
  }
  public List<Tarefa> getTarefas() {
    return tarefas;
  }
  public void setTarefas(List<Tarefa> tarefas) {
    this.tarefas = tarefas;
  }
}
{% endhighlight %}

### javax.persistence.OneToMany

Esta anotação define uma associação com outra entidade que tenha a multiplicidade de um-para-muitos.

Propriedades | Descrição
------------ | ---------
cascade | As operações que precisam ser refletidas no alvo da associação.
fetch | Informa se o alvo da associação precisa ser obtido apenas quando for necessário ou se sempre deve trazer.
mappedBy | Informa o atributo que é dono do relacionamento.
targetEntity | A classe entity que é alvo da associação.

A classe Tarefa é uma entidade normal que não conhece a classe Aula.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToOne;

/**
 * Classe utilizada para representar as tarefas aplicadas em uma aula.
 */
@Entity
public class Tarefa implements Serializable {
  private static final long serialVersionUID = 2952630017127173988L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String titulo;
  private String descricao;

  public String getDescricao() {
    return descricao;
  }
  public void setDescricao(String descricao) {
    this.descricao = descricao;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public String getTitulo() {
    return titulo;
  }
  public void setTitulo(String titulo) {
    this.titulo = titulo;
  }
}
{% endhighlight %}
