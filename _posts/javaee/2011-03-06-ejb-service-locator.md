---
layout: article
title: "EJB - Utilizando o padrão de projeto Service Locator"
categories: javaee
author: sakurai
date: 2011-03-06 08:33:00
tags: [javaee, ejb]
published: true
excerpt: Aprenda a usar o padrão de projeto Service Locator, para localizar seus componentes EJB.
comments: true
image:
  teaser: teaser-ejb.png
ads: false
---

Para melhorarmos a forma de fazer lookup, vamos utilizar um padrão de projeto chamado **Service Locator**, que consiste em deixar genérica a forma como é feito o lookup do EJB.

O padrão Service Locator serve para separar a lógica envolvida no lookup das classes que precisam utilizar o EJB.

Abaixo criamos uma classe chamada **ServiceLocator**, está classe será responsável por fazer um lookup de forma genérica, note que a natureza desta classe é altamente coesa, ou seja seu objetivo é bem definido: Fazer o lookup do EJB.

{% highlight java %}
package br.universidadejava.util;

import java.util.Properties;
import javax.naming.InitialContext;

/**
 * Classe utilizada para fazer um lookup generico do EJB.
 */
public class ServiceLocator {

  /**
   * Propriedades do Glassfish.
   */
  private static Properties properties = null;

  /**
   * Monta um objeto Properties com as informações para localizar o glassfish.
   * @return
   */
  private static Properties getProperties() {
    if (properties == null) {
      properties = new Properties();
      properties.put("org.omg.CORBA.ORBInitialHost", "localhost");
      properties.put("org.omg.CORBA.ORBInitialPort", "3700");
    }
    return properties;
  }

  /**
   * Método que faz o lookup generico de um EJB.
   *
   * @param <T>
   * @param clazz - Classe que representa a interface do EJB que será feito
   * o lookup.
   * @return
   * @throws Exception
   */
  public static <T> T buscarEJB(Class<T> clazz) throws Exception {
    /* Cria o initial context que faz via JNDI a procura do EJB. */
    InitialContext ctx = new InitialContext(getProperties());

    /* Pega o nome completo da interface utilizando reflection, faz o lookup
    do EJB e o retorno do EJB. */
    return (T) ctx.lookup(clazz.getName());
  }
}
{% endhighlight %}

Agora nossa classe que utiliza o EJB, não precisa mais saber como fazer o lookup do EJB, precisa apenas chamar o método **buscarEJB** da classe **ServiceLocator** passando como parâmetro a **Class** da interface do EJB.

{% highlight java %}
package br.universidadejava.teste;

import br.universidadejava.ejb.UsuarioRemote;
import br.universidadejava.util.ServiceLocator;

/**
 * Classe utilizada para testar o EJB de HelloWorldRemote.
 *
 * @author Rafael Guimarães Sakurai
 */
public class TesteEJB {
    public static void main(String[] args) {
        try {
            HelloWorldRemote ejb = (HelloWorldRemote) ServiceLocator.buscarEJB(HelloWorldRemote.class);

            System.out.println(ejb.ola());
        } catch (Exception ex) {
            System.out.println(ex.getMessage());
        }
    }
}
{% endhighlight %}

Outra vantagem de utilizar o Service Locator para separar a lógica do lookup, é que podemos criar, por exemplo um Proxy para caso precisemos gravar algum log quando criar o EJB ou quando seus métodos são chamados.
