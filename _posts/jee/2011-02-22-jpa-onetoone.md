---
layout: article
title: "JPA - Um-para-Um (OneToOne)"
categories: jee
author: sakurai
date: 2011-02-22 05:53:00
tags: [javaee, jpa, onetoone, relacionamento]
published: true
excerpt: Veja como especificar o relacionamento de Um-para-Um (OneToOne) entre as entidades.
comments: true
image:
  teaser: 2011-02-22-teaser-jpa-onetoone.png
ads: false
---

## Relacionamento Um-para-Um (OneToOne)

Este relacionamento informa que há apenas um registro da [entidade](http://www.universidadejava.com.br/javaee/jpa-entity/) relacionado com um registro de outra entidade.

<iframe width="420" height="315" src="https://www.youtube.com/embed/Sh-Y-beMxns" frameborder="0" allowfullscreen></iframe>

Exemplo de relacionamento Um-para-Um unidirecional, no qual temos uma Mensagem dividida em duas tabelas:

Script do banco de dados:

{% highlight sql %}
CREATE TABLE Mensagem (
  id int NOT NULL auto_increment,
  assunto varchar(200),
  dataEnvio date,
  mensagemcorpo_id int,
  PRIMARY KEY (id)
);

CREATE TABLE MensagemCorpo (
  id int(6) NOT NULL auto_increment,
  descricao varchar(1000),
  PRIMARY KEY (id)
);
{% endhighlight %}

Modelo UML:

<figure>
    <a href="/images/2011-02-22-jpa-onetoone-01.png"><img src="/images/2011-02-22-jpa-onetoone-01.png" alt="Exemplo de relacionamento @OneToOne."></a>
</figure>

Neste exemplo definimos que uma **Mensagem** possui uma **MensagemCorpo**, então destaforma a Mensagem sabe qual é seu MensagemCorpo, mas o contrario não existe, a MensagemCorpo não tem a necessidade de conhecer qual a Mensagem está associado a ele, ou seja, temos um relacionamento **unidirecional**.

Primeiramente podemos mostrar apenas uma listagem de Mensagens, mas não tem necessidade por enquanto de mostrar o conteúdo de todas as mensagens e depois caso eu queira ler o conteúdo da mensagem podemos através dela chegar até seu corpo utilizando o atributo do tipo MensagemCorpo, ou seja, encontrar o conteúdo da mensagem.

Código fonte das classes com o relacionamento:

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.Date;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToOne;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;

/**
 * Classe utilizada para representar uma Mensagem.
 */
@Entity
public class Mensagem implements Serializable {
  private static final long serialVersionUID = 1912492882356572322L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String assunto;
  @Temporal(TemporalType.DATE)
  private Date dataEnvio;
  @OneToOne(cascade=CascadeType.ALL)
  private MensagemCorpo mensagemCorpo;

  public String getAssunto() { return assunto; }
  public void setAssunto(String assunto) { this.assunto = assunto; }
  public Date getDataEnvio() { return dataEnvio; }
  public void setDataEnvio(Date dataEnvio) { this.dataEnvio = dataEnvio; }
  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public MensagemCorpo getMensagemCorpo() { return mensagemCorpo; }
  public void setMensagemCorpo(MensagemCorpo mensagemCorpo) {
    this.mensagemCorpo = mensagemCorpo;
  }
}
{% endhighlight %}

Na classe Mensagem utilizamos a anotação **javax.persistence.OneToOne** para definir o relacionamento de um-para-um entre as classes Mensagem e MensagemCorpo.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
/**
 * Classe utilizada para representar o corpo de uma mensagem.
 */
@Entity
public class MensagemCorpo implements Serializable {
  private static final long serialVersionUID = 986589124772488369L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String descricao;

  public String getDescricao() { return descricao; }
  public void setDescricao(String descricao) { this.descricao = descricao; }
  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
}
{% endhighlight %}

A classe MensagemCorpo é uma entidade normal que não conhece a classe Mensagem.

Note que não precisamos criar nenhuma referencia para informar que o atributo **mensagemCorpo** da classe Mensagem referencia a coluna **mensagemcorpo_id** da tabela Mensagem.

O JPA possui alguns padrões para facilitar o mapeamento entre a classe Java e a tabela do banco de dados, quando criamos uma coluna de chave estrangeira seguindo o padrão **nometabela_chaveprimaria**, o mapeamento é feito automaticamente pelo JPA, ou seja, o atributo **MensagemCorpo mensagemCorpo** é automaticamente associado com a coluna **mensagemcorpo_id**.

A classe MensagemDAO ficará da seguinte forma:

{% highlight java %}
package br.universidadejava.jpa.exemplo.dao;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import pbc.jpa.exemplo.modelo.Mensagem;

public class MensagemDAO {

  /**
   * Método utilizado para obter o entity manager.
   * @return
   */
  private EntityManager getEntityManager() {
    EntityManagerFactory factory = null;
    EntityManager entityManager = null;
    try {
      //Obtém o factory a partir da unidade de persistência.
      factory = Persistence.createEntityManagerFactory("MensagemPU");
      //Cria um entity manager.
      entityManager = factory.createEntityManager();
      //Fecha o factory para liberar os recursos utilizado.
    } catch(Exception ex) {
      System.out.println("ERRO");
      ex.printStackTrace();
    } finally {
      factory.close();
    }
    return entityManager;
  }

  /**
   * Método utilizado para salvar ou atualizar as informações de uma mensagem.
   * @param mensagem
   * @return
   * @throws java.lang.Exception
   */
  public Mensagem salvar(Mensagem mensagem) throws Exception {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      System.out.println("Salvando a mensagem.");
      // Verifica se a mensagem ainda não está salva no banco de dados.
      if (mensagem.getId() == null) {
        //Salva os dados da mensagem.
        entityManager.persist(mensagem);
      } else {
        //Atualiza os dados da mensagem.
        mensagem = entityManager.merge(mensagem);
      }
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } finally {
      entityManager.close();
    }
    return mensagem;
  }

  /**
   * Método que apaga a mensagem e mensagem corpor do banco de dados.
   * @param id
   */
  public void excluir(Long id) {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      // Consulta a mensagem na base de dados através do seu ID.
      Mensagem mensagem = entityManager.find(Mensagem.class, id);
      System.out.println("Excluindo os dados de: " + mensagem.getAssunto());
      // Remove a mensagem e a mensagem corpor da base de dados.
      entityManager.remove(mensagem);
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } finally {
      entityManager.close();
    }
  }

  /**
   * Consulta o mensagem pelo ID.
   * @param id
   * @return o objeto Mensagem.
   */
  public Mensagem consultarPorId(Long id) {
    EntityManager entityManager = getEntityManager();
    Mensagem mensagem = null;
    try {
      //Consulta uma mensagem pelo seu ID.
      mensagem = entityManager.find(Mensagem.class, id);
    } finally {
      entityManager.close();
    }
    return mensagem;
  }
}
{% endhighlight %}

No exemplo a seguir estamos chamando apenas o método salvar da classe MensagemDAO:

{% highlight java %}
package br.universidadejava.jpa.exemplo.teste;

import java.util.Date;
import br.universidadejava.jpa.exemplo.dao.MensagemDAO;
import br.universidadejava.jpa.exemplo.modelo.Mensagem;
import br.universidadejava.jpa.exemplo.modelo.MensagemCorpo;

public class MensagemTeste {
  public static void main(String[] args) {
    try {
      // Cria a mensagem.
      Mensagem msg = new Mensagem();
      msg.setAssunto("Exemplo de relacionamento OneToOne");
      msg.setDataEnvio(new Date());

      // Cria o corpo (conteúdo) da mensagem.
      MensagemCorpo corpo = new MensagemCorpo();
      corpo.setDescricao("O relacionamento @OneToOne informa que há apenas um registro da entidade relacionado com um registro de outra entidade, neste exemplo uma Mensagem tem apenas um MensagemCorpo.");
      msg.setMensagemCorpo(corpo);

      MensagemDAO dao = new MensagemDAO();
      // Salva a mensagem no banco de dados.
      msg = dao.salvar(msg);
      System.out.println("Salvo a mensagem: " + msg.getId() + " - " + msg.getAssunto());
    } catch (Exception ex) {
      ex.printStackTrace();
    }
  }
}
{% endhighlight %}

Ao salvar a mensagem no banco de dados, também será salvo automaticamente a mensagem corpo nas devidas tabelas:

{% highlight java %}
Salvando a mensagem.

Hibernate: insert into MensagemCorpo (descricao, id) values (?, ?)
Hibernate: insert into Mensagem (assunto, dataEnvio, mensagemCorpo_id, id) values (?, ?, ?, ?)

Salvo a mensagem: 1 - Exemplo de relacionamento OneToOne
{% endhighlight %}

### javax.persistence.OneToOne

Esta anotação define uma associação com outra entidade que tenha a multiplicidade de um-para-um.

Propriedades | Descrição
------------ | ----------
cascade | As operações que precisam ser refletidas no alvo da associação.
fetch | Informa se o alvo da associação precisa ser obtido apenas quando for necessário ou se sempre deve trazer.
mappedBy | Informa o atributo que é dono do relacionamento.
optional | Informa se a associação é opcional.
targetEntity | A classe entity que é alvo da associação.

Quando precisamos especificar um mapeamento que não é padrão do JPA, podemos utilizar a anotação **javax.persistence.JoinColumn**, por exemplo se a tabela Mensagem e MensagemCorpo fossem:

{% highlight sql %}
CREATE TABLE Mensagem (
  id int NOT NULL auto_increment,
  assunto varchar(200),
  dataEnvio date,
  ID_MENSAGEMCORPO int,
  PRIMARY KEY (id)
);

CREATE TABLE MensagemCorpo (
  MC_ID int(6) NOT NULL auto_increment,
  descricao varchar(200),
  PRIMARY KEY (MC_ID)
);
{% endhighlight %}

Poderíamos utilizar o JoinColumn para criar a associação:

{% highlight java %}
@OneToOne(cascade=CascadeType.ALL)
@JoinColumn(name = "ID_MENSAGEMCORPO", referencedColumnName = "MC_ID")
private MensagemCorpo mensagemCorpo;
{% endhighlight %}

### javax.persistence.JoinColumn

Esta anotação é utilizada para especificar a coluna utilizada na associação com outra entity.


Propriedades | Descrição
------------ | ----------
columnDefinition | Definição do tipo da coluna.
insertable | Informa se a coluna é incluída no SQL de INSERT.
name | Informa o nome da coluna de chave estrangeira.
nullable | Informa se a coluna pode ser null.
referencedColumnName | Nome da coluna que é referenciada pela coluna da chave estrangeira.
table | Nome da tabela que contém a coluna.
unique | Informa se a propriedade é chave única.
updatable | Informa se a coluna é incluída no SQL de UPDATE.


### Conteúdos relacionados

- [Utilizando relacionamento de Um para Muitos com JPA](http://www.universidadejava.com.br/jee/jpa-onetomany/)
- [Utilizando relacionamento de Muitos para Um com JPA](http://www.universidadejava.com.br/jee/jpa-manytoone/)
- [Exemplo de CRUD com JPA](http://www.universidadejava.com.br/jee/jpa-exemplo-crud/)
- [Criando consultas com JPA](http://www.universidadejava.com.br/jee/jpa-query/)