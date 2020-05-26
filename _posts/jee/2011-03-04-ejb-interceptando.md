---
layout: article
title: "Interceptando os EJBs para criar objetos DAOs"
categories: jee
author: sakurai
date: 2011-03-04 09:38:00
tags: [javaee, ejb]
published: true
excerpt: Aprenda a interceptar chamadas aos componentes EJB.
comments: true
image:
  teaser: 2011-03-04-teaser-ejb-interceptando.png
ads: false
---

Quando utilizamos EJBs estamos utilizando componentes que possuem seu ciclo de vida gerenciados pelo Container EJB, as vezes desejamos executar algo antes da criação ou depois da criação do EJB, como por exemplo quando terminar de criar o objeto do EJB, gostaríamos de criar os objetos das classes DAOs de forma generica para qualquer EJB, sem ter que ficar declarando em cada EJB como criar os DAOs.

Os Interceptadores são objetos que executam durante o ciclo de vida do EJB, neste exemplo vamos criar um interceptador para servir como uma fabrica de objetos DAO nos EJBs.

As operações salvar, atualizar, apagar, consultar por id e consultar todos de um DAO são muito similares, o que muda é apenas a entidade utilizada pelo DAO, para facilitar a criação das classes DAO vamos criar uma classe DAOBase que possui essas funcionalidades criadas de forma genérica, ou seja, independente da entidade as operações básicas serão executadas.

<figure>
    <a href="/images/2011-03-04-ejb-interceptando-01.png"><img src="/images/2011-03-04-ejb-interceptando-01.png" alt="Criando um DAO generico."></a>
</figure>

A primeira coisa que precisamos fazer é criar uma interface base para as entidade onde definimos que toda entidade deve ter uma método para obter o objeto ID da entidade.

Portanto todas as entidades precisam implementar esta interface.

{% highlight java %}
package uj.exemplo.infra.dao;

import java.io.Serializable;

/**
 * Classe básica para criação das Entity's.
 */
public interface EntidadeBase {
  /**
   * @return o Id da Entity.
   */
  public Serializable getId();
}
{% endhighlight %}

Exemplo de entidade:

{% highlight java %}
package uj.exemplo.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import uj.exemplo.infra.dao.EntidadeBase;

/**
 * Entidade que representa um Produto e demonstra o uso da interface EntidadeBase.
 */
@Entity
public class Produto implements Serializable, EntidadeBase {
  private static final long serialVersionUID = 4772286290044296438L;

  @Id
  @GeneratedValue(strategy = GenerationType.AUTO)
  private Long id;
  private String descricao;
  private Double preco;

  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public String getDescricao() { return descricao; }
  public void setDescricao(String descricao) { this.descricao = descricao; }
  public Double getPreco() { return preco; }
  public void setPreco(Double preco) { this.preco = preco; }
}
{% endhighlight %}

Vamos criar uma classe DAOBase que possui os métodos genéricos, onde as operações de salvar, atualizar, remover, consultar por id e consultar todos são iguais para todas as entidades:

{% highlight java %}
package uj.exemplo.infra.dao;

import java.util.List;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceException;
import javax.persistence.Query;

/**
 * Classe base para todas as demais classes de acesso aos dados, possui os
 * métodos genericos para todas os demais DAO's.
 *
 * @param <E>
 */
public class DAOBase<E extends EntidadeBase> {
  /**
   * EntityManager que controla as Entity's da aplicação.
   */
  private EntityManager entityManager;

  /**
   * Construtor.
   * @param em - EntityManager que controla a conexão com o banco de dados.
   */
  protected DAOBase(final EntityManager em) {
    this.entityManager = em;
  }

  /**
   * @return the entityManager
   */
  protected EntityManager getEntityManager() {
    return this.entityManager;
  }

  /**
   * Persiste um Entity na base de dados.
   *
   * @param e - Entity que será persistido.
   * @return o Entity persistido.
   * @throws Exception caso ocorra algum erro.
  */
  public E salvar(final E e) throws Exception {
    try {
      //Verifica se a Entity já existe para fazer Merge.
      if (e.getId() != null) {
        //Verifica se a Entity não está gerenciavel pelo EntityManager.
        if (!this.entityManager.contains(e)) {
          //Busca a Entity da base de dados, baseado no Id.
          if (this.entityManager.find(e.getClass(), e.getId()) == null) {
            throw new Exception("Objeto não existe!");
          }
        }
        return this.entityManager.merge(e);
      } else { // Se a Entity não existir persiste ela.
        this.entityManager.persist(e);
      }
      //Retorna a Entity persistida.
      return e;
    } catch (PersistenceException pe) {
      pe.printStackTrace();
      throw pe;
    }
  }

  /**
   * Exclui um Entity da base de dados.
   *
   * @param e - Entity que será excluida.
   * @param k - Id da Entity que será excluida.
   * @throws Exception caso ocorra algum erro.
   */
  public void excluir(final Class<E> e, final Long k) throws Exception {
    try {
      E entity = this.consultarPorId(e, k);
      //Remove a Entity da base de dados.
      this.entityManager.remove(entity);
    } catch (PersistenceException pe) {
      pe.printStackTrace();
      throw pe;
    }
  }

  /**
   * Consulta um Entity pelo seu Id.
   *
   * @param e - Classe da Entity.
   * @param l - Id da Entity.
   * @return - a Entity da classe.
   */
  public E consultarPorId(final Class<E> e, final Long l) {
    //Procura uma Entity na base de dados a partir da classe e do seu ID.
    return this.entityManager.find(e, l);
  }

  public List<E> consultarTodos(final Class<E> e) {
    Query query = this.entityManager.createQuery("SELECT e FROM " + e.getSimpleName() + " e");
    return (List<E>) query.getResultList();
  }
}
{% endhighlight %}

Todas as classes DAO agora devem extender a classe DAOBase dessa forma elas receberam a implementação genérica desses métodos.

Exemplo de DAO que é uma subclasse da DAOBase:

{% highlight java %}
package uj.exemplo.dao;

import javax.persistence.EntityManager;
import uj.exemplo.infra.dao.DAOBase;
import uj.exemplo.modelo.Produto;

/**
 * Classe DAO que é uma subclasse da classe DAOBase.
 */
public class ProdutoDAO extends DAOBase<Produto> {
  public ProdutoDAO(EntityManager em) {
    super(em);
  }
}
{% endhighlight %}

Agora que montamos um DAO generico para qualquer entidade, vamos criar o interceptador para o EJB.

<figure>
    <a href="/images/2011-03-04-ejb-interceptando-02.png"><img src="/images/2011-03-04-ejb-interceptando-02.png" alt="Interceptando um componente EJB."></a>
</figure>

Vamos criar uma anotação chamada DAOAnnotation, está anotação será utilizada pelo interceptador para verificar quais os atributos que são DAO devem ser instanciados, iremos utilizar está anotação na declaração dos atributos DAO  dentro do EJB.

{% highlight java %}
package uj.exemplo.infra.annotation;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Annotation responsável por criar uma instancia do DAO.
 */
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface DAOAnnotation {
}
{% endhighlight %}

Vamos criar agora uma fabrica de DAO, está fabrica cria uma instancia do DAO via Reflection.

{% highlight java %}
package uj.exemplo.infra.dao;

import java.lang.reflect.Constructor;
import javax.persistence.EntityManager;

/**
 * Essa Factory gera instancias automática do <code>DAO</code>.
 * Utilizando o <code>DAOAnnotation</code> nos atributos dos EJBs, pega qual a
 * classe do <code>DAO</code> gera uma nova instancia.
 */
public final class FabricaDAO {

  /**
   * Construtor privado.
   */
  private FabricaDAO() {
  }

  /**
   * Método responsável por criar uma instancia do <code>DAO</code>.
   *
   * @param entityManager - <code>EntityManager</code> que é usado no construtor do <code>DAO</code>.
   * @param clazz - Classe do DAO que será criado.
   * @return Instancia do DAO.
   * @throws Exception - Exceção lançada caso ocorra algum problema ao instanciar o <code>DAO</code>.
   */
  public static DAOBase instanciarDAO(final EntityManager entityManager, final Class<? extends DAOBase> clazz) throws Exception {
    /* Usando Reflection para pegar o construtor do DAO que recebe um EntityManager como parametro. */
    Constructor construtor = clazz.getConstructor(EntityManager.class);
    //Cria uma instancia do DAO passando o EntityManager como parámetro.
    return (DAOBase) construtor.newInstance(entityManager);
  }
}
{% endhighlight %}

Agora vamos criar o interceptador de EJB chamado InjetaDAO, este interceptador possui um método que é executado após o objeto EJB ser criado e antes dele ser entregue para quem fez seu lookup, este método vai verificar via Reflection quais atributos declarados no EJB possuem a anotação @DAOAnnotation, esses atributos serão instanciados e um objeto EntityManager será passado para eles via construtor.

{% highlight java %}
package uj.exemplo.infra.dao;

import java.lang.reflect.Field;
import javax.annotation.PostConstruct;
import javax.interceptor.InvocationContext;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import uj.exemplo.infra.annotation.DAOAnnotation;

/**
 * Classe usada para injetar o <code>DAO</code> nos atributos dos EJBs.
 */
public class InjetaDAO {
  /**
   * Construtor.
   */
  public InjetaDAO() {
  }

  /**
   * EntityManager será repassando ao <code>DAO</code>.
   */
  @PersistenceContext(unitName = "ExemploPU")
  private EntityManager entityManager;

  /**
   * Este método é usado após o EJB ser criado, e dentro do EJB procura os <code>DAO</code>s que precisa instanciar.
   * @param invocationContext - Alvo que será adicionado o <code>DAO</code>.
   * @throws Exception - Exceção lançada caso ocorra algum problema quando
   * adicionar o <code>DAO</code>.
  */
  @PostConstruct
  public void postConstruct(final InvocationContext invocationContext) throws Exception {
    //Pega o alvo
    Object target = invocationContext.getTarget();
    //Pega a classe alvo.
    Class classe = target.getClass();
    //Procura os atributos da classe.
    Field[] fields = classe.getDeclaredFields();
    //Verifica se algum dos campos da classe possui o DAOAnnotation.
    for (Field field : fields) {
      if (field.isAnnotationPresent(DAOAnnotation.class)) {
        /* Quando encontrar algum atributo, com DAOAnnotation, gera uma instancia do DAO.*/
        this.injetaDAO(field, target, this.entityManager);
      }
    }
  }

  /**
   * Método usado para gerar uma instancia do <code>DAO</code> e atribui-la ao atributo.
   *
   * @param field - Atributo que vai receber o <code>DAO</code>.
   * @param target - Classe alvo.
   * @param entityManager - <code>EntityManager</code> que será usado na instância do <code>DAO</code>.
   * @throws Exception - Exceção lançada caso ocorra algum problema quando adicionar o <code>DAO</code>.
   */
  private void injetaDAO(final Field field, final Object target, final
    EntityManager entityManager) throws Exception {
    //Pega a classe do DAO que sera instanciado.
    Class clazz = field.getType();
    //Gera uma instancia do DAO.
    DAOBase dao = FabricaDAO.instanciarDAO(entityManager, clazz);
    //Verifica se o atributo esta acessível.
    boolean acessivel = field.isAccessible();
    //Se o atributo nao e acessível, deixa ele como acessível.
    if (!acessivel) {
      field.setAccessible(true);
    }
    //Seta o DAO no atributo.
    field.set(target, dao);
    //Se o atributo nao e acessivel, volta ao valor original.
    if (!acessivel) {
      field.setAccessible(acessivel);
    }
  }
}
{% endhighlight %}

Agora precisamos adicionar as anotações no EJB para que o interceptador possa ser chamado:

{% highlight java %}
package uj.exemplo.ejb;

import java.util.List;
import javax.ejb.Remote;
import uj.exemplo.modelo.Produto;

/**
 * Interface Remota do EJB com operações basicas de um CRUD de Produto.
 */
@Remote
public interface ProdutoRemote {
  public Produto salvar(Produto produto) throws Exception;
  public void excluir(Long id) throws Exception;
  public Produto consultarPorId(Long id);
  public List<Produto> consultarTodos();
}
{% endhighlight %}

Implementação do EJB:

{% highlight java %}
package uj.exemplo.ejb;

import java.util.List;
import javax.ejb.Stateless;
import javax.interceptor.Interceptors;
import uj.exemplo.dao.ProdutoDAO;
import uj.exemplo.infra.annotation.DAOAnnotation;
import uj.exemplo.infra.dao.InjetaDAO;
import uj.exemplo.modelo.Produto;

/**
 * EJB do tipo Stateless que utiliza a injeção do DAO.
 */
@Stateless
@Interceptors(InjetaDAO.class)
public class ProdutoBean implements ProdutoRemote {
  @DAOAnnotation
  private ProdutoDAO dao;

  public Produto salvar(Produto produto) throws Exception {
    return dao.salvar(produto);
  }

  public void excluir(Long id) throws Exception {
    dao.excluir(Produto.class, id);
  }

  public Produto consultarPorId(Long id) {
    return dao.consultarPorId(Produto.class, id);
  }

  public List<Produto> consultarTodos() {
    return dao.consultarTodos(Produto.class);
  }
}
{% endhighlight %}

Note que desta forma na implementação do EJB não é mais necessario criar o DAO manualmente, também não precisa obter o EntityManager, pois ele é adicionado ao DAO no interceptador.
