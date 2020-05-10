---
layout: article
title: "EJB - Stateless Session Bean"
categories: javaee
author: sakurai
date: 2011-03-19 11:48:00
tags: [javaee, ejb, sessionbean]
published: true
excerpt: Entenda o que é um Session Bean.
comments: true
image:
  teaser: teaser-ejb.png
ads: false
---

O Session Bean é um componente Java que guarda a lógica de negocio da aplicação, este tipo de componente é executado dentro de um Container EJB. Podemos criar um EJB e deixar que diversas aplicações o utilizem. O Stateless Session Bean tem o ciclo de vida que dura apenas o tempo de uma simples chamada de método.

Quando pedimos ao Container EJB um Stateless Session Bean, através do **lookup()**, ocorrem os seguintes passos:

* Cria uma instância do Stateless Session Bean.
* Chama o método que precisa.
* Se tiver algum retorno este retorno volta para quem o chamou.
* Depois o Container EJB o destrói.

O Stateless também não mantém o estado entre as chamadas de método, ou seja, caso tenha algum atributo dentro do EJB este atributo é perdido toda vez que o EJB é chamado.

O problema do Stateless Session Bean e que a cada requisição um novo bean pode ser criado, por isso normalmente é configurado no container EJB, para gerar um pool de instâncias do EJB.

Na figura abaixo temos um exemplo de pool de instancias do Stateless Session Bean, onde o container EJB já deixa previamente criado algumas instancias. Conforme as instancias precisem ser utilizadas, elas são invocadas pelo cliente, utilizadas e devolvidas para o pool.

<figure>
    <a href="/images/2011-03-19-ejb-sessionbean-01.png"><img src="/images/2011-03-19-ejb-sessionbean-01.png" alt="Pool de stateless session bean."></a>
    <figcaption>Pool de stateless session bean adaptado de BROSE, G.; SILVERMAN, M.; SRIGANESH, R. P. (2006, p. 129)</figcaption>
</figure>

Um Stateless Session Bean é formado por uma interface Remota, Local ou ambas e mais uma implementação destas interfaces.

## Anotação javax.ejb.Remote

A interface Remota é utilizada quando queremos fazer uma chamada a um EJB que está em um Container EJB diferente de onde está o cliente, para isto é necessário adicionar a annotation javax.ejb.Remote na interface remota.

Neste exemplo criamos uma interface chamada ServicoRemote e adicionamos a anotação @Remote para informa que ela será a interface remota de um EJB. Na interface de um EJB nós declaramos a assinatura dos métodos que a implementação desse EJB precisa ter.

{% highlight java %}
package br.universidadejava.ejb.exemplo;

import javax.ejb.Remote;

/**
 * Interface remota para o EJB de Serviço.
 */
@Remote
public interface ServicoRemote {
  /**
   * Método utilizado para abrir uma solicitação para um aluno.
   * @param tipoServico - Serviço que será aberto.
   * @param aluno - Aluno que solicitou o serviço.
   */
  public abstract void abrirServico(String tipoServico, Aluno aluno);
}
{% endhighlight %}

## Anotação javax.ejb.Local

A interface Local é utilizada quando queremos fazer uma chamada a um EJB que está no mesmo Container EJB onde está o cliente, para isto é necessário adicionar a annotation javax.ejb.Local na interface local.

Neste exemplo criamos uma interface chamada ServicoLocal e adicionamos a anotação @Local para informar que ela será a interface local de um EJB. Na interface de um EJB nós declaramos a assinatura dos métodos que a implementação desse EJB precisa ter.

{% highlight java %}
package br.universidadejava.ejb.exemplo;

import java.util.List;
import javax.ejb.Local;

/**
 * Interface local para o EJB de serviço.
 */
@Local
public interface ServicoLocal {
  /**
   * Método utilizado para buscar todos os serviços de um aluno.
   * @param aluno - Aluno que abriu os serviços.
   * @return Lista de serviços de um aluno.
   * @throws Exception
   */
  public abstract List<Servico> buscarServicos(Aluno aluno) throws Exception;
}
{% endhighlight %}

Normalmente utilizamos apenas uma dessas interfaces, isto vai de acordo com o escopo do projeto e a necessidade de uso do componente.

Anotação javax.ejb.Stateless

Também precisamos criar uma classe que ira implementar uma ou ambas interfaces, para está classe ser reconhecida pelo Container EJB, como um EJB Session Bean do tipo Stateless, é necessário adicionar a annotation javax.ejb.Stateless.

Neste exemplo criamos uma classe ServicoBean que implementa a interface ServicoRemote, note que adicionamos a anotação @Stateless para informar o tipo desse EJB (Stateless Session Bean):

{% highlight java %}
package br.universidadejava.ejb.exemplo;

import javax.ejb.Stateless;

/**
 * Stateless session bean que implementa as funcionalidades referentes a interface remota.
 */
@Stateless
public class ServicoBean implements ServicoRemote {
  /**
   * Método utilizado para abrir uma solicitação para um aluno.
   * @param tipoServico - Serviço que será aberto.
   * @param aluno - Aluno que solicitou o serviço.
   */
  public void abrirServico(String tipoServico, Aluno aluno) {
    /* Código que abre a solicitação de serviço para um aluno. */
  }
}
{% endhighlight %}

## Referências

[BROSE, G.; SILVERMAN, M.; SRIGANESH, R. P.] Mastering Enterprise Java Beans 3.0 – Rima Pastel Sriganesh, Gerald Brose, Micah Silverman – 2006 – Wiley.
