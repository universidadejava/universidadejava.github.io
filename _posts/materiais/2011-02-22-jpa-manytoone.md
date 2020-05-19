---
layout: article
title: "JPA - Muitos-para-Um (ManyToOne)"
categories: javaee
author: sakurai
date: 2011-02-22 08:15:00
tags: [javaee, jpa, manytoone, relacionamento]
published: true
excerpt: Veja como especificar o relacionamento de Muitos-para-Um (ManyToOne) entre as entidades.
comments: true
image:
  teaser: 2011-02-22-teaser-jpa-manytoone.png
ads: false
---

## Relacionamento Muitos-para-Um (ManyToOne)

Este relacionamento informa que existem muitos registros de uma entidade associados a um registro de outra entidade.

<iframe width="420" height="315" src="https://www.youtube.com/embed/B5wArXmXy9M" frameborder="0" allowfullscreen></iframe>

Exemplo de relacionamento Um-para-Muitos e Muitos-para-Um bidirecional, no qual uma Pessoa possui vários Telefones:

Script do banco de dados:

{% highlight sql %}
CREATE TABLE Pessoa (
  id int NOT NULL auto_increment,
  nome varchar(200),
  cpf varchar(11),
  PRIMARY KEY (id)
);

CREATE TABLE Telefone (
  id int NOT NULL auto_increment,
  tipo varchar(200) NOT NULL,
  numero int NOT NULL,
  pessoa_id int,
  PRIMARY KEY (id)
);
{% endhighlight %}

Modelo UML:

<figure>
    <a href="/images/2011-02-22-jpa-manytoone-01.png"><img src="/images/2011-02-22-jpa-manytoone-01.png" alt="Exemplo de relacionamento @ManyToOne."></a>
</figure>

Neste exemplo definimos que uma Pessoa possui uma lista de Telefones e um Telefone está associado a uma Pessoa, portanto temos um relacionamento **bidirecional**.

Código fonte das classes com o relacionamento:

Na entidade **Pessoa** definimos que uma pessoa possui vários telefones através do atributo **List\<Telefone\> telefones** e adicionamos a anotação **javax.persistence.OneToMany** para informar que o relacionamento de Pessoa para Telefone é de Um-para-Muitos, note que nesta anotação definimos a propriedade **mappedBy** como **"pessoa"** que é para informar que o atributo com o nome **pessoa** na entity **Telefone** que é dona do relacionamento.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.List;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToMany;

/**
 * Classe utilizada para representar uma Pessoa.
 */
@Entity
public class Pessoa implements Serializable {
  private static final long serialVersionUID = -1905907502453138175L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String nome;
  private String cpf;
  @OneToMany(mappedBy = "pessoa", cascade = CascadeType.ALL)
  private List<Telefone> telefones;

  public String getCpf() {
    return cpf;
  }
  public void setCpf(String cpf) {
    this.cpf = cpf;
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
  public List<Telefone> getTelefones() {
    return telefones;
  }
  public void setTelefones(List<Telefone> telefones) {
    this.telefones = telefones;
  }
}
{% endhighlight %}

Na entidade Telefone definimos o atributo **Pessoa pessoa** e adicionamos a anotação **javax.persistence.ManyToOne** para definir que o relacionamento de Telefone para Pessoa é de Muitos-para-Um.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToOne;

/**
 * Classe utilizada para representar um Telefone.
 */
@Entity
public class Telefone implements Serializable {
  private static final long serialVersionUID = 7526502149208345058L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String tipo;
  private Integer numero;
  @ManyToOne
  private Pessoa pessoa;

  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public Integer getNumero() {
    return numero;
  }
  public void setNumero(Integer numero) {
    this.numero = numero;
  }
  public Pessoa getPessoa() {
    return pessoa;
  }
  public void setPessoa(Pessoa pessoa) {
    this.pessoa = pessoa;
  }
  public String getTipo() {
    return tipo;
  }
  public void setTipo(String tipo) {
    this.tipo = tipo;
  }
}
{% endhighlight %}

### javax.persistence.ManyToOne

Esta anotação define uma associação com outra entidade que tenha a multiplicidade de muitos-para-um.

Propriedades | Descrição
------------ | ---------
cascade | As operações que precisam ser refletidas no alvo da associação.
fetch | Informa se o alvo da associação precisa ser obtido apenas quando for necessário ou se sempre deve trazer.
optional | Informa se a associação é opcional.
targetEntity | A classe entity que é alvo da associação.

Também podemos adicionar uma tabela para realizar o relacionamento unidirecional de um-para-muitos e o relacionamento muitos-para-muitos, normalmente utilizamos está alternativa como uma forma de normalizar os dados evitando duplicar o conteúdo dos registros.

Nesse exemplo queremos utilizar a entidade Telefone com as entidades Pessoa e Aluno, ou seja, Pessoa possui uma lista de Telefones e Aluno possui uma lista de Telefones, mas o telefone não sabe para quem ele está associado. Este tipo de relacionamento é unidirecional de um-para-muitos.

<figure>
    <a href="/images/2011-02-22-jpa-manytoone-02.png"><img src="/images/2011-02-22-jpa-manytoone-02.png" alt="Exemplo de relacionamento com JoinTable."></a>
</figure>

Na base de dados iremos criar as tabelas Pessoa, Telefone e Aluno, também iremos criar duas tabelas de associação chamadas Pessoa_Telefone e Aluno_Telefone:

{% highlight sql %}
CREATE TABLE Pessoa (
  id int NOT NULL auto_increment,
  nome varchar(200),
  cpf varchar(11),
  PRIMARY KEY (id)
);

CREATE TABLE Aluno (
  id int NOT NULL auto_increment,
  nome varchar(200),
  matricula int,
  PRIMARY KEY (id)
);

CREATE TABLE Telefone (
  id int NOT NULL auto_increment,
  tipo varchar(200) NOT NULL,
  numero int NOT NULL,
  PRIMARY KEY (id)
);

CREATE TABLE Pessoa_Telefone (
  pessoa_id int,
  telefone_id int
);

CREATE TABLE Aluno_Telefone (
  aluno_id int,
  telefone_id int
);
{% endhighlight %}

Na entidade **Pessoa** definimos que uma pessoa possui vários telefones através do atributo **List\<Telefone\> telefones** e adicionamos a anotação **javax.persistence.OneToMany** para informar que o relacionamento de Pessoa para Telefone é de Um-para-Muitos.

Para informar que vamos utilizar a tabela PESSOA_TELEFONE para realizar a associação entre as tabelas PESSOA e TELEFONE utilizamos a anotação **javax.persistence.JoinTable**.

Para informar que a coluna PESSOA_ID da tabela PESSOA_TELEFONE é a coluna chave estrangeira para a tabela PESSOA e para informar que a coluna TELEFONE_ID da tabela PESSOA_TELEFONE é a chave estrangeira para a tabela TELEFONE utilizamos a anotação **javax.persistence.JoinColumn**.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.List;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.JoinTable;
import javax.persistence.OneToMany;

/**
 * Classe utilizada para representar uma Pessoa.
 */
@Entity
public class Pessoa implements Serializable {
  private static final long serialVersionUID = -1905907502453138175L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String nome;
  private String cpf;
  @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
  @JoinTable(name="PESSOA_TELEFONE",
             joinColumns={@JoinColumn(name = "PESSOA_ID")},
             inverseJoinColumns={@JoinColumn(name = "TELEFONE_ID")})
  private List<Telefone> telefones;

  public String getCpf() {
    return cpf;
  }
  public void setCpf(String cpf) {
    this.cpf = cpf;
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
  public List<Telefone> getTelefones() {
    return telefones;
  }
  public void setTelefones(List<Telefone> telefones) {
    this.telefones = telefones;
  }
}
{% endhighlight %}

### javax.persistence.JoinTable

Esta anotação é utilizada para definir uma tabela que será utilizada na associação de um-para-muitos ou de muitos-para-muitos.

<iframe width="420" height="315" src="https://www.youtube.com/embed/6LqBB2cV28Y" frameborder="0" allowfullscreen></iframe>

Propriedades | Descrição
------------ | ---------
catalog | O catalogo da tabela.
inverseJoinColumns | Chave estrangeira para realizar a associação com a tabela que não é dona do relacionamento.
joinColumns | Chave estrangeira para realizar a associação com a tabela que é dona do relacionamento.
name | Nome da tabela de associação.
schema | Esquema da tabela.
uniqueConstraints | Regras que podem ser adicionadas na tabela.

Na entidade Aluno definimos que um aluno possui vários telefones através do atributo **List\<Telefone\> telefones** e adicionamos a anotação **javax.persistence.OneToMany** para informar que o relacionamento de Aluno para Telefone é de Um-para-Muitos.

Para informar que vamos utilizar a tabela ALUNO_TELEFONE para realizar a associação entre as tabelas ALUNO e TELEFONE utilizamos a anotação **javax.persistence.JoinTable**.

Para informar que a coluna ALUNO_ID da tabela ALUNO _TELEFONE é a coluna chave estrangeira para a tabela ALUNO e para informar que a coluna TELEFONE_ID da tabela ALUNO _TELEFONE é a chave estrangeira para a tabela TELEFONE utilizamos a anotação **javax.persistence.JoinColumn**.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import java.util.List;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.JoinTable;
import javax.persistence.OneToMany;

/**
 * Classe utilizada para representar uma entidade Aluno.
 */
@Entity
public class Aluno import Serializable {
  private static final long serialVersionUID = -4292880217218734067L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String nome;
  private Long matricula;
  @OneToMany(cascade = CascadeType.ALL, fetch = FetchType.EAGER)
  @JoinTable(name="ALUNO_TELEFONE",
             joinColumns={@JoinColumn(name = "ALUNO_ID")},
             inverseJoinColumns={@JoinColumn(name = "TELEFONE_ID")})
  private List<Telefone> telefones;

  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public Long getMatricula() {
    return matricula;
  }
  public void setMatricula(Long matricula) {
    this.matricula = matricula;
  }
  public String getNome() {
    return nome;
  }
  public void setNome(String nome) {
    this.nome = nome;
  }
  public List<Telefone> getTelefones() {
    return telefones;
  }
  public void setTelefones(List<Telefone> telefones) {
    this.telefones = telefones;
  }
}
{% endhighlight %}

Agora vamos declarar a entidade **Telefone**, note que esta entidade não conhece as associações que são criadas para ela.

{% highlight java %}
package br.universidadejava.jpa.exemplo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

/**
 * Classe utilizada para representar um Telefone.
 */
@Entity
public class Telefone implements Serializable {
  private static final long serialVersionUID = 7526502149208345058L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String tipo;
  private Integer numero;

  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public Integer getNumero() {
    return numero;
  }
  public void setNumero(Integer numero) {
    this.numero = numero;
  }
  public String getTipo() {
    return tipo;
  }
  public void setTipo(String tipo) {
    this.tipo = tipo;
  }
}
{% endhighlight %}

Para testar o cadastro de Aluno e Telefone vamos criar uma classe AlunoDAO onde iremos salvar, alterar, consultar por id e apagar os registro do aluno e telefone.

{% highlight java %}
package br.universidadejava.jpa.exemplo.dao;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;
import br.universidadejava.jpa.exemplo.modelo.Aluno;

/**
 * Classe DAO para manipular as informações do Aluno no banco de dados.
 */
public class AlunoDAO {
  private EntityManager getEntityManager() {
    EntityManagerFactory factory = null;
    EntityManager entityManager = null;
    try {
      //Obtem o factory a partir da unidade de persistencia.
      factory = Persistence.createEntityManagerFactory("UnidadeDePersistencia");
      //Cria um entity manager.
      entityManager = factory.createEntityManager();
    } finally {
      factory.close();
    }
    return entityManager;
  }

  public Aluno consultarPorId(Long id) {
    EntityManager entityManager = getEntityManager();
    Aluno aluno = null;
    try {
      aluno = entityManager.find(Aluno.class, id);
    } finally {
      entityManager.close();
    }
    return aluno;
  }

  public Aluno salvar(Aluno aluno) {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      System.out.println("Salvando as informações do aluno.");
      // Verifica se o aluno ainda não está salvo no banco de dados.
      if(aluno.getId() == null) {
        entityManager.persist(aluno);
      } else {
        aluno = entityManager.merge(aluno);
      }
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } catch(Exception ex) {
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
    // Retorna o aluno salvo.
    return aluno;
  }

  public void apagar(Long id) {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      // Consulta o aluno na base de dados através do seu ID.
      Aluno aluno = entityManager.find(Aluno.class, id);
      System.out.println("Excluindo o cliente: " + aluno.getNome());
      // Remove o aluno da base de dados.
      entityManager.remove(aluno);
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } catch(Exception ex) {
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
  }
}
{% endhighlight %}

Vamos criar a classe **AlunoDAOTeste** que utilizará a classe AlunoDAO para salvar, alterar, consultar por id e apagar os registros de aluno e telefone:

{% highlight java %}
package br.universidadejava.jpa.exemplo.dao;

import java.util.ArrayList;
import java.util.List;
import br.universidadejava.jpa.exemplo.modelo.Aluno;
import br.universidadejava.jpa.exemplo.modelo.Telefone;

/**
 * Classe utilizada para testar as operações do banco de dados referente ao Aluno.
 */
public class AlunoDAOTeste {
  public static void main(String[] args) {
    AlunoDAO dao = new AlunoDAO();

    //Cria uma aluno.
    Aluno aluno = new Aluno();
    aluno.setNome("Rafael");
    aluno.setMatricula(123456L);

    //Cria o telefone residencial do aluno.
    Telefone telefone = new Telefone();
    telefone.setTipo("RES");
    telefone.setNumero(12345678);
    //Cria o telefone celular do aluno.
    Telefone telefone2 = new Telefone();
    telefone2.setTipo("CEL");
    telefone2.setNumero(87654321);

    //Cria uma lista de telefones e guarda dentro do aluno.
    List<Telefone> telefones = new ArrayList<Telefone>();
    telefones.add(telefone);
    telefones.add(telefone2);
    aluno.setTelefones(telefones);

    System.out.println("Salva as informações do aluno.");
    aluno = dao.salvar(aluno);

    System.out.println("Consulta o aluno que foi salvo.");
    Aluno alunoConsultado = dao.consultarPorId(aluno.getId());
    System.out.println(aluno.getNome());
    for(Telefone tel : aluno.getTelefones()) {
      System.out.println(tel.getTipo() + " - " + tel.getNumero());
    }

    //Cria o telefone comercial do aluno.
    Telefone telefone3 = new Telefone();
    telefone3.setTipo("COM");
    telefone3.setNumero(55554444);
    //Adiciona o novo telefone a lista de telefone do aluno.
    alunoConsultado.getTelefones().add(telefone3);

    System.out.println("Atualiza as informações do aluno.");
    alunoConsultado = dao.salvar(alunoConsultado);
    System.out.println(alunoConsultado.getNome());
    for(Telefone tel : alunoConsultado.getTelefones()) {
      System.out.println(tel.getTipo() + " - " + tel.getNumero());
    }

    System.out.println("Apaga o registro do aluno.");
    dao.apagar(alunoConsultado.getId());
  }
}
{% endhighlight %}

Quando executamos a classe **AlunoDAOTeste** temos a seguinte saída no console:

Salva as informações do aluno:

{% highlight java %}
Hibernate: insert into Aluno (matricula, nome) values (?, ?)
Hibernate: insert into Telefone (numero, tipo) values (?, ?)
Hibernate: insert into Telefone (numero, tipo) values (?, ?)
Hibernate: insert into ALUNO_TELEFONE (ALUNO_ID, TELEFONE_ID) values (?, ?)
Hibernate: insert into ALUNO_TELEFONE (ALUNO_ID, TELEFONE_ID) values (?, ?)
{% endhighlight %}

Consulta o aluno que foi salvo:

{% highlight java %}
Hibernate: select aluno0_.id as id21_1_, aluno0_.matricula as matricula21_1_, aluno0_.nome as nome21_1_, telefones1_.ALUNO_ID as ALUNO1_3_, telefone2_.id as TELEFONE2_3_, telefone2_.id as id18_0_, telefone2_.numero as numero18_0_, telefone2_.tipo as tipo18_0_ from Aluno aluno0_ left outer join ALUNO_TELEFONE telefones1_ on aluno0_.id=telefones1_.ALUNO_ID left outer join Telefone telefone2_ on telefones1_.TELEFONE_ID=telefone2_.id where aluno0_.id=?
Rafael
RES - 12345678
CEL - 87654321
{% endhighlight %}

Atualiza as informações do aluno:

{% highlight java %}
Hibernate: select aluno0_.id as id38_1_, aluno0_.matricula as matricula38_1_, aluno0_.nome as nome38_1_, telefones1_.ALUNO_ID as ALUNO1_3_, telefone2_.id as TELEFONE2_3_, telefone2_.id as id35_0_, telefone2_.numero as numero35_0_, telefone2_.tipo as tipo35_0_ from Aluno aluno0_ left outer join ALUNO_TELEFONE telefones1_ on aluno0_.id=telefones1_.ALUNO_ID left outer join Telefone telefone2_ on telefones1_.TELEFONE_ID=telefone2_.id where aluno0_.id=?
Hibernate: insert into Telefone (numero, tipo) values (?, ?)
Hibernate: delete from ALUNO_TELEFONE where ALUNO_ID=?
Hibernate: insert into ALUNO_TELEFONE (ALUNO_ID, TELEFONE_ID) values (?, ?)
Hibernate: insert into ALUNO_TELEFONE (ALUNO_ID, TELEFONE_ID) values (?, ?)
Hibernate: insert into ALUNO_TELEFONE (ALUNO_ID, TELEFONE_ID) values (?, ?)
Rafael
RES - 12345678
CEL - 87654321
COM - 55554444
{% endhighlight %}

Apaga o registro do aluno:

{% highlight java %}
Hibernate: select aluno0_.id as id55_1_, aluno0_.matricula as matricula55_1_, aluno0_.nome as nome55_1_, telefones1_.ALUNO_ID as ALUNO1_3_, telefone2_.id as TELEFONE2_3_, telefone2_.id as id52_0_, telefone2_.numero as numero52_0_, telefone2_.tipo as tipo52_0_ from Aluno aluno0_ left outer join ALUNO_TELEFONE telefones1_ on aluno0_.id=telefones1_.ALUNO_ID left outer join Telefone telefone2_ on telefones1_.TELEFONE_ID=telefone2_.id where aluno0_.id=?
Hibernate: delete from ALUNO_TELEFONE where ALUNO_ID=?
Hibernate: delete from Telefone where id=?
Hibernate: delete from Telefone where id=?
Hibernate: delete from Telefone where id=?
Hibernate: delete from Aluno where id=?
{% endhighlight %}


### Conteúdos relacionados

- [Utilizando relacionamento de Um para Um com JPA](http://www.universidadejava.com.br/javaee/jpa-onetoone/)
- [Utilizando relacionamento de Um para Muitos com JPA](http://www.universidadejava.com.br/javaee/jpa-onetomany/)
- [Utilizando relacionamento de Muitos para Muitos com JPA](http://www.universidadejava.com.br/javaee/jpa-manytomany/)
- [Criando consultas com JPA](http://www.universidadejava.com.br/javaee/jpa-query/)