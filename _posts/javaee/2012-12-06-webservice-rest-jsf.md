---
layout: article
title: "Web Service REST - Chamando um web service REST com JSF"
categories: javaee
author: sakurai
date: 2012-12-06 09:07:00
tags: [webservice, rest, jsf]
published: true
excerpt: Vamos criar uma aplicação web com JSF que consome um Web Service REST.
comments: true
image:
  teaser: 2012-12-06-teaser-webservice-rest-jsf.png
ads: false
---

O serviço web (web service) é uma forma de integração bem utilizada atualmente para realizar a integração entre aplicações. O Web Service REST é uma das formas de criar um serviço web, que utilizada muito o protocolo HTTP para realizar essa integração entre as aplicações.

O Web Service REST pode disponibilizar através do protocolo HTTP métodos para manipulação de informações, por exemplo: ao fazer uma requisição HTTP através do método GET para a URL http://localhost:8080/CinemaREST/servico/filmes é possível obter um recurso, que nesse caso é uma lista de filmes. Se chamar essa URL através de um navegador podemos verificar o retorno desse Web Service REST.

<figure>
    <a href="/images/2012-12-06-webservice-rest-jsf-01.png"><img src="/images/2012-12-06-webservice-rest-jsf-01.png" alt=""></a>
</figure>

Para consumir um Web Service REST, existem diversas implementações possíveis, uma delas é através da API Jersey, que é a implementação de referência do JavaEE.

Crie uma aplicação web chamada **CinemaJSF**, que utiliza o framework do JavaServer Faces para criação das páginas WEB.

Vamos alterar uma página inicial **index.xhtml**, para que ela utilize um managed bean para consumir esse web serviço, a página ficará assim:

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
  <h:head>
    <title>Meu Cinema</title>
  </h:head>
  <h:body>
    <h1>Filmes em cartaz</h1>
    <h:outputText value="#{cinemaMB.filmesEmCartaz}"/>
  </h:body>
</html>
{% endhighlight %}

O outputText irá chamar o método **getFilmesEmCartaz()** da managed bean **CinemaMB**, que chama o web service REST que traz todos os filmes em cartaz.

Para utilizar a API do Jersey dentro da aplicação, clique com o botão direito do mouse em cima do nome do projeto e escolha o item Propriedades. Na tela de propriedades acesse a categoria Bibliotecas e adicione a biblioteca Jersey 1.8 (JAX-RS RI) através do menu Adicionar Biblioteca.

<figure>
    <a href="/images/2012-12-06-webservice-rest-jsf-02.png"><img src="/images/2012-12-06-webservice-rest-jsf-02.png" alt=""></a>
</figure>

Vamos criar a managed bean **CinemaMB** com a implementação do método:

{% highlight java %}
package br.metodista.managedbean;
import com.sun.jersey.api.client.Client;
import com.sun.jersey.api.client.WebResource;
import javax.faces.bean.ManagedBean;

@ManagedBean
public class CinemaMB {
  public String getFilmesEmCartaz() {
    Client c = Client.create();
    WebResource wr = c.resource("http://localhost:8080/CinemaREST/servico/filmes");
    return wr.get(String.class);
  }
}
{% endhighlight %}

Com a classe Client é possível obter um resource web (recurso web) através da

URL do web service REST, e com esse recurso é possível chamar os métodos que o web service REST suporta, como: get, post, put, delete, etc.

Ao chamar o método wr.get(String.class), estamos esperando que a chamada para esse serviço devolva uma String, nesse exemplo essa String vem no formato JSON (JavaScript Object Notation), mas poderia ser uma String simples, um formato XML, etc. Ao executar a aplicação CinemaJSF teremos a seguinte tela:

<figure>
    <a href="/images/2012-12-06-webservice-rest-jsf-03.png"><img src="/images/2012-12-06-webservice-rest-jsf-03.png" alt=""></a>
</figure>

Para converter esse JSON em objeto Java, podemos usar uma API simples do Google chamada **gson** (https://code.google.com/p/google-gson) que realiza essa conversão de maneira fácil. Primeiro vamos criar a classe Filme que irá representar cada filme da lista:

{% highlight java %}
package br.metodista.modelo;

public class Filme {
  private Long id;
  private String filme;
  private String sinopse;
  private String genero;
  private Integer duracao;
  private String trailer;

  public Filme() {
  }

  public Long getId() {
    return id;
  }
  public void setId(Long id) {
    this.id = id;
  }
  public String getFilme() {
    return filme;
  }
  public void setFilme(String filme) {
    this.filme = filme;
  }
  public String getSinopse() {
    return sinopse;
  }
  public void setSinopse(String sinopse) {
    this.sinopse = sinopse;
  }
  public String getGenero() {
    return genero;
  }
  public void setGenero(String genero) {
    this.genero = genero;
  }
  public Integer getDuracao() {
    return duracao;
  }
  public void setDuracao(Integer duracao) {
    this.duracao = duracao;
  }
  public String getTrailer() {
    return trailer;
  }
  public void setTrailer(String trailer) {
    this.trailer = trailer;
  }
}
{% endhighlight %}

Adicione a bibioteca do gson no projeto, clique com o botão direito do mouse em cima do nome do projeto e escolha o item Propriedades. Na tela de propriedades acesse a categoria Bibliotecas e adicione a biblioteca **gson-2.2.2.jar** através do menu Adicionar JAR / Pasta.

<figure>
    <a href="/images/2012-12-06-webservice-rest-jsf-04.png"><img src="/images/2012-12-06-webservice-rest-jsf-04.png" alt=""></a>
</figure>

Vamos alterar o CinemaMB para utilizar a API do gson e converter o JSON em uma lista de filmes:

{% highlight java %}
package br.metodista.managedbean;

import br.metodista.modelo.Filme;
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import com.sun.jersey.api.client.Client;
import com.sun.jersey.api.client.WebResource;
import java.util.List;
import javax.faces.bean.ManagedBean;

@ManagedBean
public class CinemaMB {
  public List<Filme> getFilmesEmCartaz() {
    Client c = Client.create();
    WebResource wr = c.resource("http://localhost:8080/CinemaREST/servico/filmes");
    String json = wr.get(String.class);
    Gson gson = new Gson();
    return gson.fromJson(json, new TypeToken<List<Filme>>(){}.getType());
  }
}
{% endhighlight %}

Agora vamos mudar a página index.xhtml para mostrar uma tabela com os filmes:

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core">
  <h:head>
    <title>Meu Cinema</title>
  </h:head>
  <h:body>
    <h1>Filmes em cartaz</h1>
    <h:outputText value=""/>
    <h:dataTable value="#{cinemaMB.filmesEmCartaz}" var="f" width="100%">
      <h:column>
        <f:facet name="header">
          <h:outputText value="Titulo"/>
        </f:facet>
        <h:outputText value="#{f.filme}"/>
      </h:column>
      <h:column>
        <f:facet name="header">
          <h:outputText value="Genero"/>
        </f:facet>
        <h:outputText value="#{f.genero}"/>
      </h:column>
      <h:column>
        <f:facet name="header">
          <h:outputText value="Duração"/>
        </f:facet>
        <h:outputText value="#{f.duracao} min"/>
      </h:column>
      <h:column>
        <f:facet name="header">
          <h:outputText value="Sinopse"/>
        </f:facet>
        <h:outputText value="#{f.sinopse}"/>
      </h:column>
    </h:dataTable>
  </h:body>
</html>
{% endhighlight %}

A tela ficará com a seguinte aparência:

<figure>
    <a href="/images/2012-12-06-webservice-rest-jsf-05.png"><img src="/images/2012-12-06-webservice-rest-jsf-05.png" alt=""></a>
</figure>
