---
layout: article
title: "Criando uma aplicação com EJB + JPA"
categories: javaee
author: sakurai
date: 2011-03-25 12:13:00
tags: [javaee, ejb, jpa]
published: true
excerpt: Criando uma aplicação com EJB e JPA.
comments: true
image:
  teaser: 2011-03-25-teaser-criando-aplicacao-ejb-jpa.png
ads: false
---

Vamos desenvolver uma aplicação utilizando EJB 3.0 + JPA 1.0 para efetuar emprestimos de livro, o projeto em questão possui um cadastro de livro, um cadastro de pessoa e podemos emprestar livros para as pessoas.

## Criando o Projeto EJB

Tendo este escopo vamos criar um Projeto EJB no NetBeans:

Criando um **Projeto EJB**

Na área **Projetos**, clique com o botão direito do mouse, e em seguida clique em **Novo projeto...**

Na tela **Novo projeto**, selecione na aba **Categorias** a opção **Java EE**, na aba **Projetos** a opção **Módulo EJB** e clique em **Próximo**.

Na tela de **Novo Módulo EJB**, vamos definir os seguintes campos:

* **Nome do projeto:** EmprestimoEJB
* **Localização do projeto:** (Escolha o local para salvar o projeto no micro)

Clique em **Próximo**.

Na tela de **Novo Módulo EJB**, vamos definir os seguintes campos:

* **Servidor:** GlassFish Server 3.1
* **Versão do Java EE:** JavaEE 6

Clique em **Finalizar**.

Desta forma criamos um Projeto EJB chamado **EmprestimoEJB** que será publicado dentro do servidor de aplicação web **GlassFish**.

## Criando as classes de negocio

Devido ao escopo do projeto teremos inicialmente três classes (**Livro**, **Pessoa** e **Emprestimo**).

Diagrama de classes:

<figure>
    <a href="/images/2011-03-25-criando-aplicacao-ejb-jpa-01.png"><img src="/images/2011-03-25-criando-aplicacao-ejb-jpa-01.png" alt="Diagrama de classes."></a>
</figure>

Com base no diagrama de classes, vamos criar essas classes dentro do nosso projeto EmprestimoEJB:

Clique com o botão direito do mouse sobre **Pacotes de código fonte**, depois selecione a opção **Novo**, depois selecione **Classe Java...**

Na tela de **Novo Classe Java**, vamos definir os seguintes valores:

* **Nome da classe:** Livro
* **Pacote:** br.universidadejava.emprestimo.modelo (utilizamos o pacote para separar os arquivos Java dentro da aplicação).

Clique em **Finalizar**.

Repita este mesmo processo para as classes **Pessoa** e **Emprestimo**.

Seguindo o modelo UML, vamos adicionar nas classes os atributos, métodos get / set e anotações referentes ao JPA, depois nossas classes ficaram da seguinte forma:

### Classe Livro

{% highlight java %}
package br.universidadejava.emprestimo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

/**
* Classe utilizada para representar um Livro.
*/
@Entity
public class Livro implements Serializable {
  /* Serial Version UID. */
  private static final long serialVersionUID = 6775900088322385451L;
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String titulo;
  private String autor;
  private boolean emprestado;

  public Livro() {
    this.emprestado = false;
  }

  public String getAutor() { return autor; }
  public void setAutor(String autor) { this.autor = autor; }
  public boolean isEmprestado() { return emprestado; }
  public void setEmprestado(boolean emprestado) { this.emprestado = emprestado; }
  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public String getTitulo() { return titulo; }
  public void setTitulo(String titulo) { this.titulo = titulo; }
}
{% endhighlight %}

### Classe Pessoa

{% highlight java %}
package br.universidadejava.emprestimo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

/**
* Classe utilizada para representar uma Pessoa.
*/
@Entity
public class Pessoa implements Serializable {
  /* Serial Version UID */
  private static final long serialVersionUID = 5486103235574819424L;
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String nome;

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

### Classe Emprestimo

{% highlight java %}
package br.universidadejava.emprestimo.modelo;

import java.io.Serializable;
import java.util.Date;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToOne;
import javax.persistence.Temporal;

/**
* Classe utilizada para representar o Emprestimo de um Livro feito por uma Pessoa.
*/
@Entity
public class Emprestimo implements Serializable {
  /* Serial Version UID */
  private static final long serialVersionUID = 4621324705834774752L;
  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  @ManyToOne
  private Livro livro;
  @ManyToOne
  private Pessoa pessoa;
  @Temporal(javax.persistence.TemporalType.DATE)
  private Date dataEmprestimo;
  @Temporal(javax.persistence.TemporalType.DATE)
  private Date dataDevolucao;

  public Date getDataDevolucao() {
    return dataDevolucao;
  }
  public void setDataDevolucao(Date dataDevolucao) {
    this.dataDevolucao = dataDevolucao;
  }
  public Date getDataEmprestimo() {
    return dataEmprestimo;
  }
  public void setDataEmprestimo(Date dataEmprestimo) {
    this.dataEmprestimo = dataEmprestimo;
  }
  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public Livro getLivro() {
    return livro;
  }
  public void setLivro(Livro livro) {
    this.livro = livro;
  }
  public Pessoa getPessoa() {
    return pessoa;
  }
  public void setPessoa(Pessoa pessoa) {
    this.pessoa = pessoa;
  }
}
{% endhighlight %}

## Criando as classes DAOs

Vamos criar a camada de persistência dos dados, ou seja, vamos crias as classes **DAO**, que utilizam o **EntityManager** para executar as operações no **Banco de Dados**. Estas classes devem ser criadas no pacote **br.universidadejava.emprestimo.dao**.

### Classe EmprestimoDAO

{% highlight java %}
package br.universidadejava.emprestimo.dao;

import javax.persistence.EntityManager;
import br.universidadejava.emprestimo.modelo.Emprestimo;

/**
 * Classe utilizada para realizar as operações com o bando de dados.
 */
public class EmprestimoDAO {
  private EntityManager entityManager;

  /**
   * Construtor da classe DAO que chama os métodos do EntityManager.
   * @param entityManager
   */
  public EmprestimoDAO(EntityManager entityManager) {
    this.entityManager = entityManager;
  }

  /**
   * Método para salvar ou atualizar o emprestimo.
   * @param emprestimo
   * @return
   * @throws java.lang.Exception
   */
  public Emprestimo salvar(Emprestimo emprestimo) throws Exception {
    System.out.println("Emprestando o livro " + emprestimo.getLivro().getTitulo() + " para a pessoa " + emprestimo.getPessoa().getNome());
    /* Verifica se o emprestimo ainda não está salvo no banco de dados. */
    if(emprestimo.getId() == null) {
      /* Salva o emprestimo no banco de dados. */
      this.entityManager.persist(emprestimo);
    } else {
      /* Verifica se o emprestimo não está no estado managed. */
      if(!this.entityManager.contains(emprestimo)) {
        /* Se o emprestimo não está no estado managed verifica se ele existe na base. */
        if (consultarPorId(emprestimo.getId()) == null) {
          throw new Exception("Livro não existe!");
        }
      }
      /* Faz uma atualização do emprestimo. */
      return entityManager.merge(emprestimo);
    }

    /* Retorna o emprestimo que foi salvo, este retorno ocorre para modemos ter o id que foi salvo. */
    return emprestimo;
  }

  /**
   * Método que exclui o Emprestimo do banco de dados.
   * @param id
   */
  public void excluir(Long id) {
    /* Consulta o emprestimo na base de dados através de seu ID. */
    Emprestimo emprestimo = consultarPorId(id);
    System.out.println("Excluindo o emprestimo: " + emprestimo.getId());
    /* Remove o emprestimo da base de dados. */
    entityManager.remove(emprestimo);
  }

  /**
   * Método que consulta um Emprestimo através do Id.
   * @param id
   * @return
   */
  public Emprestimo consultarPorId(Long id) {
    return entityManager.find(Emprestimo.class, id);
  }
}
{% endhighlight %}

Note que agora não precisamos mais criar a **EntityManager** manualmente, podemos deixar para quem for utilizar o DAO (no nosso caso o EJB) passar uma referência para o EntityManager.

Os métodos do DAO que fazem manipulação no banco de dados agora não precisam mais iniciar e finalizar uma transação, pois como serão chamados a partir de um EJB, os métodos do EJB que chamam os métodos do DAO já possuem transação por padrão.

### Classe LivroDAO

{% highlight java %}
package br.universidadejava.emprestimo.dao;

import javax.persistence.EntityManager;
import br.universidadejava.emprestimo.modelo.Livro;

/**
 * Classe utilizada para realizar as operações com o bando de dados.
 */
public class LivroDAO {
  private EntityManager entityManager;

  /**
   * Construtor da classe DAO que chama os métodos do EntityManager.
   * @param entityManager
   */
  public LivroDAO(EntityManager entityManager) {
    this.entityManager = entityManager;
  }

  /**
   * Método para salvar ou atualizar o livro.
   * @param livro
   * @return
   * @throws java.lang.Exception
   */
  public Livro salvar(Livro livro) throws Exception {
    System.out.println("Salvando o livro: " + livro.getTitulo());
    /* Verifica se o livro ainda não está salvo no banco de dados. */
    if(livro.getId() == null) {
      /* Salva o livro no banco de dados. */
      this.entityManager.persist(livro);
    } else {
      /* Verifica se o livro não está no estado managed. */
      if(!this.entityManager.contains(livro)) {
        /* Se o livro não está no estado managed verifica se ele existe na base. */
        if (entityManager.find(Livro.class, livro.getId()) == null) {
          throw new Exception("Livro não existe!");
        }
      }

      /* Faz uma atualização do livro que estava gravado na base de dados. */
      return entityManager.merge(livro);
    }
    /* Retorna o livro que foi salvo, este retorno ocorre para modemos ter o id que foi salvo. */
    return livro;
  }

  /**
   * Método que exclui o livro do banco de dados.
   * @param id
   */
  public void excluir(Long id) {
    /* Consulta o livro na base de dados através de seu ID. */
    Livro livro = entityManager.find(Livro.class, id);
    System.out.println("Excluindo o livro: " + livro.getTitulo());
    /* Remove o livro da base de dados. */
    entityManager.remove(livro);
  }

  /**
   * Método que consulta o livro pelo ID.
   * @param id
   * @return
   */
  public Livro consultarPorId(Long id) {
    return entityManager.find(Livro.class, id);
  }
}
{% endhighlight %}

### Classe PessoaDAO

{% highlight java %}
package br.universidadejava.emprestimo.dao;

import javax.persistence.EntityManager;
import br.universidadejava.emprestimo.modelo.Pessoa;

/**
 * Classe utilizada para realizar as operações com o bando de dados.
 */
public class PessoaDAO {
  private EntityManager entityManager;

  /**
   * Construtor da classe DAO que chama os métodos do EntityManager.
   * @param entityManager
   */
  public PessoaDAO(EntityManager entityManager) {
    this.entityManager = entityManager;
  }

  /**
   * Método para salvar ou atualizar a pessoa.
   * @param pessoa
   * @return
   * @throws java.lang.Exception
   */
  public Pessoa salvar(Pessoa pessoa) throws Exception{
    System.out.println("Salvando o pessoa: " + pessoa.getNome());
    /* Verifica se a pessoa ainda não está salva no banco de dados. */
    if(pessoa.getId() == null) {
      /* Salva a pessoa no banco de dados. */
      this.entityManager.persist(pessoa);
    } else {
      /* Verifica se a pessoa não está no estado managed. */
      if(!this.entityManager.contains(pessoa)) {
        /* Se a pessoa não está no estado managed verifica se ele existe na base. */
        if (entityManager.find(Pessoa.class, pessoa.getId()) == null) {
          throw new Exception("Livro não existe!");
        }
      }
      /* Faz uma atualização da pessoa que estava gravado na base de dados. */
      return entityManager.merge(pessoa);
    }
    /* Retorna a pessoa que foi salva, este retorno ocorre para modemos ter o id que foi salvo. */
    return pessoa;
  }

  /**
   * Método que exclui a pessoa do banco de dados.
   * @param id
   */
  public void excluir(Long id) {
    /* Consulta a pessoa na base de dados através de seu ID. */
    Pessoa pessoa = entityManager.find(Pessoa.class, id);
    System.out.println("Excluindo a pessoa: " + pessoa.getNome());
    /* Remove a pessoa da base de dados. */
    entityManager.remove(pessoa);
  }

  /**
   * Consulta a pessoa por ID.
   * @param id
   * @return
   */
  public Pessoa consultarPorId(Long id) {
    return entityManager.find(Pessoa.class, id);
  }
}
{% endhighlight %}

## Criando os componentes EJB Session Bean Stateless

Vamos criar os componentes EJB que possuem a lógica de negocio. Lembrando que cada EJB é formado por uma interface e uma classe.

Crie uma interface chamada **EmprestimoRemote** e uma classe chamada **EmprestimoBean** no pacote **br.universidadejava.emprestimo.ejb**.

### Interface EmprestimoRemote

{% highlight java %}
package br.universidadejava.emprestimo.ejb;

import javax.ejb.Remote;
import br.universidadejava.emprestimo.modelo.Emprestimo;

/**
 * Interface que possui os métodos que o EJB de Emprestimo precisa implementar.
 */
@Remote
public interface EmprestimoRemote {
  public Emprestimo salvar(Emprestimo emprestimo) throws Exception;
  public void excluir(Long id);
  public Emprestimo consultarPorId(Long id);
}
{% endhighlight %}

Nós estamos declarando uma interface chamada **EmprestimoRemote** e adicionamos a anotação **javax.ejb.Remote** que irá informar que este componente EJB pode ser acessado remotamente ou forá do seu container.

Na interface nós declaramos todos os métodos que o componente EJB poderá disponibilizar.

### Classe EmprestimoBean

{% highlight java %}
package br.universidadejava.emprestimo.ejb;

import javax.ejb.Stateless;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import br.universidadejava.emprestimo.dao.EmprestimoDAO;
import br.universidadejava.emprestimo.modelo.Emprestimo;

/**
 * EJB de Emprestimo com a lógica de negocio.
 */
@Stateless
public class EmprestimoBean implements EmprestimoRemote {
  @PersistenceContext(unitName = "EmprestimoPU")
  private EntityManager em;

  public Emprestimo salvar(Emprestimo emprestimo) throws Exception {
    EmprestimoDAO dao = new EmprestimoDAO(em);
    return dao.salvar(emprestimo);
  }

  public void excluir(Long id) {
    EmprestimoDAO dao = new EmprestimoDAO(em);
    dao.excluir(id);
  }

  public Emprestimo consultarPorId(Long id) {
    EmprestimoDAO dao = new EmprestimoDAO(em);
    return dao.consultarPorId(id);
  }
}
{% endhighlight %}

A classe **EmprestimoBean** implementa a interface EmprestimoRemote, portanto ela implementa todos os métodos declarados na interface. Também adicionamos a anotação **javax.ejb.Stateless** para informar que este componente é do tipo **Stateless Session Bean** e que não irá manter o valor dos seus atributos (estado).

Note que declaramos um atributo chamado em do tipo **EntityManager** a adicionamos a anotação **javax.persistence.PersistenceContext**, dessa forma o componente EJB recebe do Container EJB uma instancia do tipo EntityManager com a conexão com o banco de dados.

Quando utilizamos **@PersistenceContext** especificamos qual o nome da unidade de persistência queremos obter (O nome da unidade de persistência é especificado no arquivo persistence.xml).

Vamos agora criar o EJB para o Livro e Pessoa:

### Interface LivroRemote

{% highlight java %}
package br.universidadejava.emprestimo.ejb;

import javax.ejb.Remote;
import br.universidadejava.emprestimo.modelo.Livro;

/**
 * Interface que possui os métodos que o EJB de Livro precisa implementar.
 */
@Remote
public interface LivroRemote {
  public Livro salvar(Livro livro) throws Exception;
  public void excluir(Long id);
  public Livro consultarPorId(Long id);
}
{% endhighlight %}

### Classe LivroBean

{% highlight java %}
package br.universidadejava.emprestimo.ejb;

import javax.ejb.Stateless;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import br.universidadejava.emprestimo.dao.LivroDAO;
import br.universidadejava.emprestimo.modelo.Livro;

/**
 * EJB de Livro com a lógica de negocio.
 * @author Rafael Guimarães Sakurai
 */
@Stateless
public class LivroBean implements LivroRemote {
  @PersistenceContext(unitName = "EmprestimoPU")
  private EntityManager em;

  public Livro salvar(Livro livro) throws Exception {
    LivroDAO dao = new LivroDAO(em);
    return dao.salvar(livro);
  }

  public void excluir(Long id) {
    LivroDAO dao = new LivroDAO(em);
    dao.excluir(id);
  }

  public Livro consultarPorId(Long id) {
    LivroDAO dao = new LivroDAO(em);
    return dao.consultarPorId(id);
  }
}
{% endhighlight %}

### Interface PessoaRemote

{% highlight java %}
package br.universidadejava.emprestimo.ejb;

import javax.ejb.Remote;
import br.universidadejava.emprestimo.modelo.Pessoa;

/**
 * Interface que possui os métodos que o EJB de Pessoa precisa implementar.
 */
@Remote
public interface PessoaRemote {
  public Pessoa salvar(Pessoa pessoa) throws Exception;
  public void excluir(Long id);
  public Pessoa consultarPorId(Long id);
}
{% endhighlight %}

### Classe PessoaBean

{% highlight java %}
package br.universidadejava.emprestimo.ejb;

import javax.ejb.Stateless;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import br.universidadejava.emprestimo.dao.PessoaDAO;
import br.universidadejava.emprestimo.modelo.Pessoa;

/**
 * EJB de Pessoa com a lógica de negocio.
 */
@Stateless
public class PessoaBean implements PessoaRemote {
  @PersistenceContext(unitName = "EmprestimoPU")
  private EntityManager em;

  public Pessoa salvar(Pessoa pessoa) throws Exception {
    PessoaDAO dao = new PessoaDAO(em);
    return dao.salvar(pessoa);
  }

  public void excluir(Long id) {
    PessoaDAO dao = new PessoaDAO(em);
    dao.excluir(id);
  }

  public Pessoa consultarPorId(Long id) {
    PessoaDAO dao = new PessoaDAO(em);
    return dao.consultarPorId(id);
  }
}
{% endhighlight %}

Até o momento criamos a seguinte estrutura, note que as interfaces e classes são bem coesas, ou seja, todas elas tem um propósito especifico, na camada de EJB temos apenas lógica de negocio, na camada de persistência temos apenas classes que fazem o trabalho com o banco de dados.

A seguir temos as classes e interfaces criadas até o momento:

<figure>
    <a href="/images/2011-03-25-criando-aplicacao-ejb-jpa-02.png"><img src="/images/2011-03-25-criando-aplicacao-ejb-jpa-02.png" alt="Diagrama de classes."></a>
</figure>

## Criando a base de dados

{% highlight sql %}
create database emprestimo;
use emprestimo;

create table livro (
  id int(5) not null auto_increment,
  titulo varchar(100) not null,
  autor varchar(100) not null,
  emprestado int(1) not null,
  primary key(id)
);

create table pessoa (
  id int(5) not null auto_increment,
  nome varchar(100) not null,
  primary key(id)
);

create table emprestimo (
  id int(5) not null auto_increment,
  livro_id int(5) not null,
  pessoa_id int(5) not null,
  dataemprestimo date,
  datadevolucao date,
  primary key(id)
);
{% endhighlight %}

OBS: Esse script é para o banco de dados MySQL se quiser pode criar o script para Oracle ou outro banco de dados.

## Criando o pool de conexões no servidor web Glassfish

Quando utilizamos o JPA para fazer a persistência com o banco de dados, precisamos fazer as seguintes configurações:

* Adicionar driver do banco de dados
* Criar um data source com pool de conexão no Glassfish
* Criar arquivo persistence.xml

Adicionar do driver do banco de dados:

Coloque o driver do banco de dados na pasta de instalação do Glassfish:
    **..\glassfish-vx\domains\domain1\lib**

Criar um data source com pool de conexão no Glassfish

Inicie o Glassfish e entre na url: **http://localhost:4848/login.jsf**, abrira a tela de administração.

Na tela de administração do Glassfish, selecione o item **Recursos -> JDBC -> JDBC Connection Pools**

Na tela de JDBC Connection Pools, clique no botão Novo...

Na tela **Novo grupo de conexões JDBC (Etapa 1 de 2)** (Novo Pool de Conexão JDBC), configure as seguintes opções:

* **Pool Name:** Emprestimo
* **Tipo de Recurso:** javax.sql.ConnectionPoolDataSource
* **Fornecedor do Banco de Dados:** MySQL

Se estivermos trabalhando com outra base de dados é só escolher no combo de **Fornecedor do Banco de Dados**.

Depois clique em **Avançar**.

Na tela de **Novo grupo de conexões JDBC (Etapa 2 de 2)**, desça até a parte de **Adicionar Propriedades**:

Clique no botão **Selecionar todos**, depois clique em **Excluir Propriedades**.

Depois adicione as seguintes propriedades:

* **user:** nome do usuário no banco de dados
* **password:** senha do usuário no banco de dados
* **url:** caminho para encontrar o banco de dados.

<figure>
    <a href="/images/2011-03-25-criando-aplicacao-ejb-jpa-03.png"><img src="/images/2011-03-25-criando-aplicacao-ejb-jpa-03.png" alt="Configurando o pool de conexões."></a>
</figure>

Quando utilizamos o banco de dados :

* MySQL utilizamos a URL:

  * jdbc:mysql://ip:porta/nomeDaBase

  * Exemplo: jdbc:mysql://localhost:3306/emprestimo

* Oracle utilizamos a URL:

  * jdbc:oracle:thin:@ip:porta:nomeDaBase

  * Exemplo: jdbc:oracle:thin:@localhost:1521:emprestimo

Volte para o topo da página e clique em **Finalizar**.

Clique no pool de conexão que acabamos de criar **Emprestimo**, e clique no botão **Ping** para verificar se o Glassfish encontra o banco de dados.

<figure>
    <a href="/images/2011-03-25-criando-aplicacao-ejb-jpa-04.png"><img src="/images/2011-03-25-criando-aplicacao-ejb-jpa-04.png" alt="Configurando o pool de conexões."></a>
</figure>

Deve aparecer na tela a mensagem **Ping executado com êxito**, portanto conseguiu encontrar no banco de dados.

Agora vamos criar o recurso JDBC que será utilizado por nosso projeto EJB:

Clique na opção **Recursos -> JDBC -> JDBC Resources**

Na tela de **Recursos JDBC** clique no botão **Novo...**

Na tela de **Novo Recursos JDBC** configure os itens:

* **Nome JNDI:** jdbc/Emprestimo
* **Nome do grupo:** Emprestimo

Depois clique no botão **OK**.

Pronto, criamos o pool de conexão no Glassfish.

## Criando a unidade de persistencia

Criar arquivo persistence.xml

Agora vamos criar uma **Unidade de Persistência**, que utiliza o **Pool de Conexão** que acabamos de criar no Glassfish:

Clique com o botão direito sobre o projeto **EmprestimoEJB**, selecione a opção Novo e depois selecione **Unidade de persistência...**

Na tela de **Novo Unidade de persistência**, configure os seguintes itens:

* **Nome da unidade de persistência:** EmprestimoPU
* **Provedor de persistência:** Hibernate JPA (1.0)
* **Fontes de dados:** jdbc/Emprestimo
* **Estratégia de geração de tabela:** Nenhum

Depois clique no botão **Finalizar**.

Abra o arquivo **persistence.xml** dentro da pasta **Arquivos de configuração** e escreva o seguinte código:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
  http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

  <persistence-unit name="EmprestimoPU" transaction-type="JTA">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <jta-data-source>jdbc/Emprestimo</jta-data-source>
    <exclude-unlisted-classes>false</exclude-unlisted-classes>
    <properties>
      <property name="hibernate.show_sql" value="true"/>
    </properties>
  </persistence-unit>
</persistence>
{% endhighlight %}

Note que o nome da unidade de persistência **EmprestimoPU** que acabamos de criar, é o mesmo nome que utilizamos na anotação **@PersistenceContext**, pois é através dessa unidade de persistência que o **Container EJB** criar um **EntityManager**.

## Publicando a aplicação no servidor web Glassfish

Feito isto nossa aplicação EmprestimoEJB já está pronta, com a seguinte estrutura:

<figure>
    <a href="/images/2011-03-25-criando-aplicacao-ejb-jpa-05.png"><img src="/images/2011-03-25-criando-aplicacao-ejb-jpa-05.png" alt="Estrutura do projeto."></a>
</figure>

Agora para publicar a aplicação EmprestimoEJB no Glassfish, clique com o botão direito do mouse em cima do projeto e clique em Implantar.

Está pronto!
