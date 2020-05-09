---
layout: article
title: "EJB - Criando um HelloWorld com EJB 3.0 na IDE NetBeans"
categories: javaee
author: sakurai
date: 2011-03-06 07:54:00
tags: [javaee, ejb]
published: true
excerpt: Aprenda a criar um hello world com EJB 3.0 na IDE NetBeans.
comments: true
image:
  teaser: 2011-03-06-teaser-ejb-helloworld-netbeans.png
ads: true
---

Nesse vídeo demonstro como criar um hello world com EJB 3.0.

<iframe width="560" height="315" src="https://www.youtube.com/embed/KC3fpJzc8L4" frameborder="0" allowfullscreen></iframe>

## Criando um projeto EJB no NetBeans

Vamos criar um **Novo Projeto**, na aba **Categorias** selecione **Java EE**, e na aba **Projetos** selecione **Módulo EJB**.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-01.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-01.png" alt="Novo Modulo EJB."></a>
</figure>

Clique em **Próximo >**.

Na tela de **Novo Módulo EJB**, no campo **Nome** do projeto digite **HelloWorldEJB** e informe o Local do projeto de sua preferência.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-02.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-02.png" alt="Novo Modulo EJB."></a>
</figure>

Clique em **Próximo >**.

Agora selecione o **Servidor Glassfish Server** que será publicado o projeto EJB e **Versão do Java EE** como, por exemplo: **Java EE 6** ou superior.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-03.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-03.png" alt="Novo Modulo EJB."></a>
</figure>

Clique em **Finalizar**.


## Criando um EJB Stateless Session Bean

Crie a interface **HelloWorldRemote** e adicione a assinatura do método ola().

{% highlight java %}
package br.universidadejava.ejb;

import javax.ejb.Remote;

@Remote
public interface HelloWorldRemote {
    public abstract String ola();
}
{% endhighlight %}

Nesta interface adicionamos a anotação **@Remote** para informar que ela será acessada remotamente.

Agora vamos implementar esta interface, crie uma classe chamada **HelloWorldBean** e implemente o método **ola()**.

{% highlight java %}
package br.universidadejava.ejb;

import javax.ejb.Stateless;

@Stateless
public class HelloWorldBean implements HelloWorldRemote {
    public String ola() {
        return "Ola mundo com EJB.";
    }
}
{% endhighlight %}

Nesta classe adicionamos a anotação **@Stateless** para informar que ela será um componente EJB do tipo Stateless Session Bean.

## Publicando o projeto EJB no Glassfish

Clique com o botão direito sobre o projeto **HelloWorldEJB** e clique em **Implantar**, assim o projeto EJB será publicado no Glassfish e também vai iniciar o Glassfish.

Depois que terminar de iniciar o Glassfish aparecera a seguinte mensagem no console de saída:

{% highlight java %}
INFO: Portable JNDI names for EJB HelloWorldBean : [java:global/HelloWorldEJB/HelloWorldBean!br.universidadejava.ejb.HelloWorldRemote, java:global/HelloWorldEJB/HelloWorldBean]
INFO: Glassfish-specific (Non-portable) JNDI names for EJB HelloWorldBean : [br.universidadejava.ejb.HelloWorldRemote#br.universidadejava.ejb.HelloWorldRemote, br.universidadejava.ejb.HelloWorldRemote]
INFO: HelloWorldEJB was successfully deployed in 11.478 milliseconds.
{% endhighlight %}

## Criando um Projeto Java Console para testar o EJB

No NetBeans crie um **Novo projeto**, na aba **Categorias** selecione **Java**, e na aba **Projetos** selecione **Aplicativo Java** e clique em **Próximo**.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-04.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-04.png" alt="Novo aplicativo Java."></a>
</figure>

Na janela **Novo Aplicativo Java**, deixe o **Nome do projeto** como **HelloWorld** e na **Localização do projeto**, defina a mesma pasta onde criamos o projeto EJB **HelloWorldEJB**.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-05.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-05.png" alt="Novo aplicativo Java."></a>
</figure>

Antes de começarmos a desenvolver o cliente do EJB, precisamos adicionar algumas bibliotecas (jar) dentro do projeto, essas bibliotecas serão utilizadas para fazer o lookup do EJB.

Clique com o botão direito no projeto **HelloWorld** e vá em **Propriedades**. Na tela de **Propriedades do projeto**, selecione a categoria **Bibliotecas** e clique no botão **Adicionar JAR / Pasta**.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-06.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-06.png" alt="Adicionar bibliotecas no projeto."></a>
</figure>

Na janela **Adicionar JAR / Pasta** navegue ate a pasta de instalação do Glassfish **/glassfish-vXX/glassfish/modules/**.

<figure>
    <a href="/images/2011-03-06-ejb-helloworld-netbeans-07.png"><img src="/images/2011-03-06-ejb-helloworld-netbeans-07.png" alt="Adicionar bibliotecas no projeto."></a>
</figure>

Selecione os seguinte JAR e clique em **Abrir**:

  **gf-client.jar** ou **gf-client-module.jar**

Agora precisamos adicionar uma referência entre os projetos, clique no botão **Adicionar projeto...** e selecione o projeto **HelloWorldEJB** e clique em **OK**.

Esta referência entre os projetos serve para podermos referenciar as classes e interfaces, como por exemplo a interface **HelloWorldRemote**.

Na pasta **Pacotes de códigos-fontes**, crie a classe **TesteEJB** no package **br.universidadejava.teste**, iremos utilizar está classe para testar o EJB que criamos.

{% highlight java %}
package br.universidadejava.teste;

import br.universidadejava.ejb.HelloWorldRemote;
import javax.naming.InitialContext;
import javax.naming.NamingException;

/**
 * Classe utilizada para testar o EJB de HelloWorldRemote.
 */
public class TesteEJB {
  public static void main(String[] args) {
    try {
      //Método que faz o lookup para encontrar o EJB de HelloWorldRemote.
      InitialContext ctx = new InitialContext();
      HelloWorldRemote ejb = (HelloWorldRemote) ctx.lookup
        ("br.universidadejava.ejb.HelloWorldRemote");

      System.out.println(ejb.ola());

    } catch (NamingException ex) {
      ex.printStackTrace();
      System.out.println("Não encontrou o EJB.");
    } catch (Exception ex) {
      ex.printStackTrace();
      System.out.println(ex.getMessage());
    }
  }
}
{% endhighlight %}

Criamos o método **main()** para testar o método **ola()** do **HelloWorldRemote**, neste método criamos um objeto do tipo **javax.naming.InitialContext** que possui o método lookup utilizado para obter uma instancia do EJB.

Repare que para pedir um EJB para o Glassfish fazemos o **lookup** passando o nome completo da classe remota do EJB, também podemos fazer o lookup especificando seu projeto e modulo **java:global[/app-name]/module-name/bean-name**.

**Testando o EJB:**

Agora execute a classe **TesteEJB** do projeto **HelloWorld** e teremos a seguinte saída no console.

{% highlight java %}
  Ola mundo com EJB.
{% endhighlight %}

Se o Glassfish estiver instalado em outra maquina ou a porta padrão **3700** for alterada é possível criar um arquivo de propriedades (java.util.Properties) para definir estas informações, como por exemplo:

{% highlight java %}
package br.universidadejava.teste;

import br.universidadejava.ejb.HelloWorldRemote;
import java.util.Properties;
import javax.naming.InitialContext;
import javax.naming.NamingException;

/**
 * Classe utilizada para testar o EJB de HelloWorldRemote.
 */
public class TesteEJB {
  public static void main(String[] args) {
    try {
      //Monta um objeto Properties com as informações para localizar o Glassfish.
      Properties props = new Properties();
      props.put("org.omg.CORBA.ORBInitialHost", "localhost");
      props.put("org.omg.CORBA.ORBInitialPort", "3700");

      //Método que faz o lookup para encontrar o EJB de HelloWorldRemote.
      InitialContext ctx = new InitialContext(getProperties());
      HelloWorldRemote ejb = (HelloWorldRemote) ctx.lookup
        ("br.universidadejava.ejb.HelloWorldRemote");

      System.out.println(ejb.ola());

    } catch (NamingException ex) {
      ex.printStackTrace();
      System.out.println("Não encontrou o EJB.");
    } catch (Exception ex) {
      ex.printStackTrace();
      System.out.println(ex.getMessage());
    }
  }
}
{% endhighlight %}
