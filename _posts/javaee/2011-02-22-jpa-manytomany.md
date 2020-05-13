---
layout: article
title: "JPA - Muitos-para-Muitos (ManyToMany)"
categories: javaee
author: sakurai
date: 2011-02-22 08:43:00
tags: [javaee, jpa]
published: true
excerpt: Veja como especificar o relacionamento de Muitos-para-Muitos (ManyToMany) entre as entidades.
comments: true
image:
  teaser: 2011-02-22-teaser-jpa-manytomany.png
ads: false
---

## Relacionamento Muitos-para-Muitos (ManyToMany)

<iframe width="420" height="315" src="https://www.youtube.com/embed/GRyNWIEZ6MQ" frameborder="0" allowfullscreen></iframe>

Este relacionamento informa que muitos registros de uma entidade estão relacionados com muitos registros de outra entidade. No exemplo a seguir um Projeto possui muitos Funcionarios e muitos Funcionarios estão relacionados a um Projeto:

Script do banco de dados:

{% highlight sql %}
CREATE TABLE Projeto (
  id int NOT NULL auto_increment,
  nome varchar(200),
  PRIMARY KEY (id)
);

CREATE TABLE Funcionario (
  id int NOT NULL auto_increment,
  nome varchar(200),
  PRIMARY KEY (id)
);

CREATE TABLE Projeto_Funcionario (
  projeto_id int,
  funcionario_id int
);
{% endhighlight %}

Modelo UML:

<figure>
    <a href="/images/2011-02-22-jpa-manytomany-01.png"><img src="/images/2011-02-22-jpa-manytomany-01.png" alt="Exemplo de relacionamento @ManyToMany."></a>
</figure>

Um Projeto tem vários funcionarios e um Funcionario participa de vários projetos, portanto temos um relacionamento bidirecional de muitos-para-muitos.

Código fonte das classes com o relacionamento:

A entidade **Projeto** possui um relacionamento de **muitos-para-muitos** com a entidade **Funcionario**, para definir esta associação utilizamos a anotação **javax.persistence.ManyToMany**, note que utilizamos a propriedade **mappedBy** da anotação ManyToMany para informar que o Projeto é o dono do relacionamento.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.List;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToMany;

/**
 * Classe utilizada para representar um projeto.
 */
@Entity
public class Projeto implements Serializable {
  private static final long serialVersionUID = 1081869386060246794L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String nome;
  @ManyToMany(mappedBy="projetos", cascade = CascadeType.ALL)
  private List<Funcionario> desenvolvedores;

  public List<Funcionario> getDesenvolvedores() { return desenvolvedores; }
  public void setDesenvolvedores(List<Funcionario> desenvolvedores) {
    this.desenvolvedores = desenvolvedores;
  }
  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public String getNome() { return nome; }
  public void setNome(String nome) { this.nome = nome; }
}
{% endhighlight %}

### javax.persistence.ManyToMany

Esta anotação define uma associação com outra entidade que tenha a multiplicidade de muitos-para-muitos.

Propriedades | Descrição
cascade | As operações que precisam ser refletidas no alvo da associação.
fetch | Informa se o alvo da associação precisa ser obtido apenas quando for necessário ou se sempre deve trazer.
mappedBy | Informa o atributo que é dono do relacionamento.
targetEntity | A classe entity que é alvo da associação.

A entidade **Funcionario** possui um relacionamento de **muitos-para-muitos** com a entidade **Projeto**, para definir esta associação utilizamos a anotação **javax.persistence.ManyToMany**.

Para informar que vamos utilizar a tabela PROJETO_FUNCIONARIO para realizar a associação entre PROJETO e FUNCIONARIO utilizamos a anotação **javax.persistence.JoinTable**.

Para informar que a coluna PROJETO_ID da tabela PROJETO_FUNCIONARIO é a coluna chave estrangeira para a tabela PROJETO e para informar que a coluna FUNCIONARIO_ID da tabela PROJETO_FUNCIONARIO é a chave estrangeira para a tabela FUNCIONARIO utilizamos a anotação javax.persistence.JoinColumn.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.List;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.JoinTable;
import javax.persistence.ManyToMany;

/**
 * Classe utilizada para representar um Funcionario.
 */
@Entity
public class Funcionario implements Serializable {
  private static final long serialVersionUID = -9109414221418128481L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String nome;
  @ManyToMany(cascade = CascadeType.ALL)
  @JoinTable(name="PROJETO_FUNCIONARIO",
             joinColumns={@JoinColumn(name="PROJETO_ID")},
             inverseJoinColumns={@JoinColumn(name="FUNCIONARIO_ID")})
  private List<Projeto> projetos;

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
  public List<Projeto> getProjetos() {
    return projetos;
  }
  public void setProjetos(List<Projeto> projetos) {
    this.projetos = projetos;
  }
}
{% endhighlight %}
