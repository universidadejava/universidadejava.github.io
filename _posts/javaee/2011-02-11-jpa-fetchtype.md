---
layout: article
title: "JPA - FetchType"
categories: javaee
author: sakurai
date: 2011-02-22 18:52:00
tags: [javaee, jpa]
published: true
excerpt: Veja como especificar se as entidades relacionadas devem ou não ser consultadas.
comments: true
image:
  teaser: teaser-jpa.png
ads: false
---

## javax.persistence.FetchType

Com o FetchType podemos definir a forma como serão trazidos os relacionamentos, podemos fazer de duas formas:

### FetchType.EAGER

Traz todas as entidades que estão relacionadas, ou seja, se a Entidade A possui um relacionamento com a Entidade B, então quando consultar a Entidade A, também será consultado suas referencias na Entidade B.

Exemplo de relacionamento EAGER onde ao consultar uma Página de um álbum já traz também todos as Fotos que estão relacionadas com aquela página:

{% highlight java %}
@Entity
public class Pagina {
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private Integer numero;
  @OneToMany(cascade=CascadeType.ALL, fetch=FetchType.EAGER)
  @JoinColumn(name="pagina_id")
  private List<Foto> fotos;
}
{% endhighlight %}

Nesse exemplo utilizamos a enum FetchType.EAGER no relacionamento um-para-um para informar que quando consultarmos a entidade Pagina, queremos consultar também a entidade Foto ao mesmo tempo. Por padrão quando não é especificado o FetchType ele é EAGER.


### FetchType.LAZY

Não traz as entidades que estão relacionadas, ou seja, se a Entidade A possui um relacionamento com a Entidade B, então quando consultar a Entidade A só serão retornadas as informações referentes a esta Entidade.

Exemplo de relacionamento LAZY onde o corpo da mensagem é consultado apenas quando houver a necessidade:

{% highlight java %}
@Entity
public class Mensagem {
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String assunto;
  @Temporal(TemporalType.DATE)
  private Date dataEnvio;
  @OneToOne(cascade=CascadeType.ALL, fetch=FetchType.LAZY)
  private MensagemCorpo mensagemCorpo;
}
{% endhighlight %}

Nesse exemplo utilizamos a enum FetchType.LAZY no relacionamento um-para-um para informar que quando consultarmos a entidade Mensagem, não queremos consultar a entidade MensagemCorpo ao mesmo tempo.
