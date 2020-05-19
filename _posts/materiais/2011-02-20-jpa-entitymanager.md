---
layout: article
title: "JPA - EntityManager"
categories: javaee
author: sakurai
date: 2011-02-20 06:42:00
tags: [java, javaee, jpa, entity, manager]
published: true
excerpt: Gerenciando as entidades no JPA com o EntityManager.
comments: true
image:
  teaser: teaser-jpa.png
ads: false
---

## EntityManager

O EntityManager é um serviço responsável por gerenciar as entidades, através dele é possível gerenciar o ciclo de vida das entidades, operação de sincronização com a base de dados (inserir, atualizar ou remover), consultar entidades e outros.

Quando uma Entity está associada a um EntityManager, dizemos que esta entidade está no contexto gerenciavel, onde todas as operações realizadas no objeto da Entity é refletido no banco de dados. Todas as identidades das entidades são únicas e para cada registro no banco de dados haverá apenas uma referencia no contexto do EntityManager.

O EntityManager pode ser gerenciado de duas formas:

* Gerenciado pelo Container
* Gerenciado pela Aplicação

### EntityManager gerenciado pela Aplicação

Para gerenciar o EntityManager pela aplicação precisamos fazer os seguintes passos:

1) Através da classe **javax.persistence.Persistence** podemos utilizar o método **createEntityManagerFactory()** que recebe como parâmetro o nome da unidade de persistência que contem as informações sobre o banco de dados para criar uma **javax.persistence.EntityManagerFactory**.

{% highlight java %}
EntityManagerFactory factory = Persistence.createEntityManagerFactory("UnitName");
{% endhighlight %}

2) Através do javax.persistence.EntityManagerFactory podemos utilizar o método createEntityManager() para obtermos uma javax.persistence.EntityManager.

{% highlight java %}
EntityManager entityManager = factory.createEntityManager();
{% endhighlight %}

Exemplo:

{% highlight java %}
package br.universidadejava.jpa.exemplo.dao;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

public class DAO {

  public EntityManager getEntityManager() {
    EntityManagerFactory factory = Persistence.createEntityManagerFactory("ExemplosJPAPU");
    EntityManager entityManager = factory.createEntityManager();
    factory.close();

    return entityManager;
  }

}
{% endhighlight %}

Dentro do método **getEntityManager()** criamos um **EntityManagerFactory** da unidade de persistência **ExemplosJPAPU** (veja o material sobre Unidade de Persistência) através da classe **Persistence**, depois criamos um **EntityManager** a partir da factory. Quando terminamos de utilizar a **EntityManagerFactory** é importante chamar o método **close()** para liberar os recursos da factory.

### EntityManager gerenciado pelo Container

Quando trabalhamos com aplicações Java EE, podemos deixar o container EJB injetar a [unidade de persistência](http://www.universidadejava.com.br/javaee/jpa-unidade-persistencia/) através da anotação **javax.persistence.PersistenceContext**.

{% highlight java %}
package br.universidadejava.jpa.exemplo.ejb;

import javax.ejb.Stateless;
import javax.persistence.EntityManager;

/**
 * EJB utilizado para demonstrar o uso da injeção de dependência do EntityManager.
 */
@Stateless
public class ExemploBean implements ExemploRemote {

  @PersistenceContext(unitName = "ExemplosJPAPU")
  private EntityManager entityManager;

}
{% endhighlight %}

Neste exemplo temos um Session Bean Stateless que utiliza a anotação **@PersistenceContext** para adicionar uma unidade de persistência no EntityManager, repare que não é preciso criar manualmente a EntityManager, pois o container EJB se encarrega de fazer isso e também atribui seu objeto para a variável com a anotação. A anotação **@PersistenceContext** possui o atributo **unitName** que podemos utilizar para informar qual o nome da unidade de persistência neste exemplo **ExemplosJPAPU** que foi definido na tag **\<persistence-unit\>** do arquivo **persistence.xml**.

## Interface EntityManager

A interface **javax.persistence.EntityManager** possui a assinatura de métodos para manipular as entidades, executar consultas e outros:

### public void persist(Object entity);

Faz uma nova instância gerenciável e persistivel.

{% highlight java %}
EntityTransaction transaction = entityManager.getTransaction();
transaction.begin();
entityManager.persist(pessoa);
transaction.commit();
{% endhighlight %}

Utilizamos o método **persist** da EntityManager para salvar a Entity Pessoa no banco de dados.

Note que criamos uma **javax.persitence.EntityTransaction** para criar uma transação, pois os métodos persist(), merge() e remove() precisam de um transação para realizar suas operações no banco de dados.

### public <T> T merge(T entity);

Junta o estado da Entity com o estado persistido.

{% highlight java %}
EntityTransaction transaction = entityManager.getTransaction();
transaction.begin();
entityManager.merge(pessoa);
transaction.commit();
{% endhighlight %}

Utilizamos o método **merge** da EntityManager para atualizar a Entity Pessoa no banco de dados.

### public void remove(Object entity);

Remove a instância da Entity do banco de dados.

{% highlight java %}
EntityTransaction transaction = entityManager.getTransaction();
transaction.begin();
Pessoa pessoa = entityManager.find(Pessoa.class, id);
entityManager.remove(pessoa);
transaction.commit();
{% endhighlight %}

Primeiro utilizamos o valor do **id** para pesquisar (find) a Entity no banco de dados, quando fazemos esta pesquisa e estamos dentro da transação a Entity fica no estado persistivel dentro da EntityManager, então podemos utilizar o método **remove** para remover a Entity Pessoa do banco de dados.

### public boolean contains(Object entity);

Checa se a instância da Entity está em um estado persistivel.

{% highlight java %}
boolean contem = entityManager.contains(pessoa);
{% endhighlight %}

### public <T> T find(Class<T> entityClass, Object primaryKey);

Procura um registro no banco de dados através da entity e id da tabela, caso não encontre retorna null.

{% highlight java %}
Pessoa pessoa = entityManager.find(Pessoa.class, id);
{% endhighlight %}

Utilizamos o método find da EntityManager que pesquisa uma Entity Pessoa pela sua classe e a chave primaria.

### public <T> T getReference(Class<T> entityClass, Object primaryKey);

Procura um registro no banco de dados através da entity e id da tabela, caso não encontre retorna uma exceção **javax.persistence.EntityNotFoundException**.

### public void flush();

Os métodos persist(), merge() e remove() aguardam finalização da transação para sincronizar as entidades com o banco de dados, o método flush() força essa sincronização no momento em que é chamado.

### public void refresh(Object entity);

Verifica se houve alguma alteração no banco de dados para sincronizar com a entidade.

### public void clear();

Remove as entidades que estão no estado gerenciável dentro da EntityManager.

### public void close();

Fecha a conexão do EntityManager.

### public boolean isOpen();

Verifica se o EntityManager esta com a conexão aberta.

### public EntityTransaction getTransaction();

Obtém uma **javax.persistence.EntityTransaction** que é uma transação com o banco de dados.


## Ciclo de vida da Entity

Uma entidade do banco de dados pode possuir quatro estados diferentes que fazem parte do seu ciclo de vida: novo, gerenciável, desacoplado e removido:

<figure>
    <a href="/images/2011-02-20-jpa-entitymanager-01.png"><img src="/images/2011-02-20-jpa-entitymanager-01.png" alt="Ciclo de vida da Entity."></a>
    <figcaption>Ciclo de vida da entidade adaptado de BROSE, G.; SILVERMAN, M.; SRIGANESH, R. P. (2006, p.180)</figcaption>
</figure>

**Novo (new)**

A Entity foi criada, mas ainda não foi persistida no banco de dados. Mudanças no estado da Entity não são sincronizadas com o banco de dados.

**Gerenciado (managed)**

A Entity foi persistida no banco de dados e encontra-se em um estado gerenciável. Mudanças no estado da Entity são sincronizados com o banco de dados assim que um transação for finalizada com sucesso.

**Removido (removed)**

A Entity foi agendada para ser removida da base de dados, essa entidade será removida fisicamente do banco de dados quando a transação for finalizada com sucesso.

**Desacoplado (detached)**

A Entity foi persistida no banco de dados, mas encontra-se em um estado que não está associado ao contexto persistivel, ou seja, alterações em seu estado não são refletidos na base de dados.

## Referências

[BROSE, G.; SILVERMAN, M.; SRIGANESH, R. P.] Mastering Enterprise JavaBeans 3.0 – Rima Pastel Sriganesh, Gerald Brose, Micah Silverman – 2006 – Wiley – Disponível


### Conteúdos relacionados

- [Alguns exercícios para você praticar com JPA](http://www.universidadejava.com.br/javaee/jpa-exercicios-01/)
- [Exemplo de CRUD com JPA](http://www.universidadejava.com.br/javaee/jpa-exemplo-crud/)
- [Utilizando relacionamento de Um para Muitos com JPA](http://www.universidadejava.com.br/javaee/jpa-onetomany/)
- [Criando consultas com JPA](http://www.universidadejava.com.br/javaee/jpa-query/)