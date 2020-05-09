---
layout: article
title: "JPA - Estratégia de SEQUENCE para gerar ID"
categories: javaee
author: sakurai
date: 2011-02-22 04:12:00
tags: [javaee, jpa]
published: true
excerpt: No banco de dados Oracle podemos utilizar uma estratégia de SEQUENCE para obter o ID da entidade.
comments: true
image:
  teaser: teaser-jpa.png
ads: true
---

Quando utilizamos o banco de dados Oracle por exemplo, podemos utilizar uma estratégia de SEQUENCE para obter o ID da entidade.

Vamos criar um exemplo de entidade que utiliza uma SEQUENCE para gerar o id referente a chave primaria da tabela, abaixo vamos criar a sequência USUARIO_SEQ:

{% highlight sql %}
CREATE SEQUENCE USUARIO_SEQ INCREMENT BY 1 START WITH 1 NOCACHE NOCYCLE;
{% endhighlight %}

A seguir vamos criar a entidade Usuario que irá utilizar está sequence para gerar o id.

{% highlight java %}
package br.universidadejava.jpa.exemplo.sequence.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.SequenceGenerator;
import javax.persistence.Table;

@Entity
@Table(name = "USUARIO")
@SequenceGenerator(name = "USU_SEQ", sequenceName = "USUARIO_SEQ", initialValue = 1, allocationSize = 1)
public class Usuario implements Serializable {
  private static final long serialVersionUID = -4023522856316087762L;

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "USU_SEQ")
  private Long id;
  private String nome;

  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public String getNome() { return nome; }
  public void setNome(String nome) { this.nome = nome; }
}
{% endhighlight %}

A anotação **@SequenceGenerator** vai indicar qual a sequence do banco, qual o valor inicial e a quantidade que essa sequence é incrementada.

Propriedades | Descrição
------------ | ---------
name | informa o apelido da sequence, exemplo “USU_SEQ”.
sequenceName | informa o nome da sequence criada no Banco de Dados, exemplo: “USUARIO_SEQ”.
initialValue | informa o valor inicial da sequence, exemplo 1.
allocationSize | informa a quantidade que será incrementada na sequence, o default é 50, neste exemplo definimos que será incrementado de 1 em 1.

A anotação **@GeneratedValue** nesse caso está usando a estratégia de SEQUENCE, então ele vai procurar a sequência que tem o apelido “USU_SEQ” e faz um consulta no banco para pegar seu resultado:

{% highlight sql %}
SELECT USUARIO_SEQ.NEXTVAL FROM DUAL;
{% endhighlight %}

Propriedades | Descrição
------------ | ---------
strategy | informa o tipo de estrategia que será utilizado, exemplo GenerationType.SEQUENCE.
generator | informa o apelido da sequence, exemplo “USU_SEQ”.
