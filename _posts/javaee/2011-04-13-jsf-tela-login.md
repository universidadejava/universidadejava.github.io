---
layout: article
title: "JSF - Criando uma tela de login"
categories: javaee
author: sakurai
date: 2011-04-13 20:10:00
tags: [java]
published: true
excerpt: Neste exemplo vamos criar uma simples tela na qual o usuário pode informar seu login e senha e clicar no botão Entrar para acessar o sistema.
comments: true
image:
  teaser: 2011-04-13-teaser-jsf-tela-login.png
ads: false
---

Neste exemplo vamos criar uma simples tela na qual o usuário pode informar seu `login` e `senha` e clicar no botão `Entrar` para acessar o sistema, conforme imagem a seguir:

<figure>
    <a href="/images/2011-04-13-jsf-tela-login-01.png"><img src="/images/2011-04-13-jsf-tela-login-01.png" alt="Tela de login com JSF."></a>
</figure>

Nesse vídeo demonstro como criar a tela de login utilizando JSF e EJB.

<iframe width="420" height="315" src="https://www.youtube.com/embed/fxQst-VcKQo" frameborder="0" allowfullscreen></iframe>

## Crie uma aplicação web Java chamada LoginWeb.

Vamos alterar a página `index.xhtml` para desenhar a tela, nela vamos usar os seguintes componentes do JSF `form` (monta um formulário), `outputText` (representa um texto na tela), `inputText` (representa um campo de entrada de dados na tela), `inputSecret` (representa um campo de entrada de dados para informações que não devem ser visivelmente lidas), `panelGrid` (monta uma tabela para organizar os componentes) e `commandButton` (representa um botão na tela).

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
    <h:head>
        <title>Login Web</title>
    </h:head>
    <h:body>
      <h:form id="formLogin">
        <h1><h:outputText id="bemVindo" value="Bem vindo(a) ao sistema."/></h1>
        <h:panelGrid id="gridLoginSenha" columns="2">
          <h:outputText id="loginLabel" value="Login:"/>
          <h:inputText id="login" value=""/>

          <h:outputText id="senhaLabel" value="Senha:"/>
          <h:inputSecret id="senha" value=""/>
        </h:panelGrid>
        <h:commandButton id="botao" value="Entrar"/>
      </h:form>
    </h:body>
</html>
{% endhighlight %}

Se você executar a aplicação é possível ver como ficou a tela `index.xhtml`, mas até o momento não colocamos nenhuma ação nela, note que não tem nenhum código Java nesta tela, apenas tags do JSF.

Vamos criar uma classe `Usuario` para representar os dados de `login` e `senha` apresentados na tela:

{% highlight java %}
package pbc.jsf.modelo;

public class Usuario {
  private String login;
  private String senha;

  public String getLogin() {
    return login;
  }

  public void setLogin(String login) {
    this.login = login;
  }

  public String getSenha() {
    return senha;
  }

  public void setSenha(String senha) {
    this.senha = senha;
  }
}
{% endhighlight %}

Vamos criar também uma classe `LoginMB` que será o `ManagedBean` responsável pelas ações e atributos da tela de login.

Esta classe é uma classe Java normal, a unica coisa a mais que colocamos nela é a anotação `@ManagedBean` que serve para informar para o JSF que agora esta classe é um `ManagedBean`.

{% highlight java %}
package pbc.jsf.managedbean;

import javax.faces.bean.ManagedBean;
import pbc.jsf.modelo.Usuario;

@ManagedBean
public class LoginMB {
  private Usuario usuario = new Usuario();

  public String doEfetuarLogin() {
    if("sakurai".equals(usuario.getLogin()) &&
       "123".equals(usuario.getSenha())) {
      /* Se escrever o login e senha correto então vai para a tela principal do sistema. */
      return "principal";
    }

    //Caso erre o login ou senha fica na mesma tela.
    return null;
  }

  public Usuario getUsuario() {
    return usuario;
  }

  public void setUsuario(Usuario usuario) {
    this.usuario = usuario;
  }
}
{% endhighlight %}

Crie uma página `principal.xhtml` apenas para simular o acesso do usuário dentro da aplicação.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
    <h:head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
        <title>Login Web</title>
    </h:head>
    <h:body>
        <h1>Bem Vindo</h1>
    </h:body>
</html>
{% endhighlight %}

Agora que criamos o ManagedBean vamos alterar a página `index.xhtml` para adicionar os atributos `login` e `senha`, também vamos colocar a ação do botão:

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
    <h:head>
        <title>Login Web</title>
    </h:head>
    <h:body>
      <h:form id="formLogin">
        <h1><h:outputText id="bemVindo" value="Bem vindo(a) ao sistema."/></h1>
        <h:panelGrid id="gridLoginSenha" columns="2">
          <h:outputText id="loginLabel" value="Login:"/>
          <h:inputText id="login" value="#{loginMB.usuario.login}"/>

          <h:outputText id="senhaLabel" value="Senha:"/>
          <h:inputSecret id="senha" value="#{loginMB.usuario.senha}"/>
        </h:panelGrid>
        <h:commandButton id="botao" value="Entrar" action="#{loginMB.doEfetuarLogin}"/>
      </h:form>
    </h:body>
</html>
{% endhighlight %}

Agora se executar a aplicação novamente, ela já está funcionando, os valores digitados nos campos login e senha, são colocados dentro dos atributos do objeto `Usuario` e o ação de clicar no botão de `Entrar` vai chamar o método `doEfetuarLogin` do `ManagedBean`, se os dados forem digitados corretamente a aplicação irá redirecionar o usuário para a página `principal.xhtml`.

Para melhorar um pouco mais vamos definir que os campos login e senha são obrigatórios, para isso altere na página `index.xhtml` os `h:inputText` e coloque um label para especificar o nome do campo que aparece na tela e defina como `true` a propriedade `required`, também vamos adicionar um componente do faces `h:messages` que irá apresentar todas as mensagens na tela:

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
    <h:head>
        <title>Login Web</title>
    </h:head>
    <h:body>
      <h:form id="formLogin">
       <h1><h:outputText id="bemVindo" value="Bem vindo(a) ao sistema."/></h1>
        <h:panelGrid id="gridLoginSenha" columns="2">
          <h:outputText id="loginText" value="Login:"/>
          <h:inputText id="login" label="Login"
            value="#{loginMB.usuario.login}" required="true"/>

          <h:outputText id="senhaText" value="Senha:"/>
          <h:inputSecret id="senha" label="Senha"
            value="#{loginMB.usuario.senha}" required="true"/>
        </h:panelGrid>
        <h:messages id="msgErro" style="color: red"/>
        <h:commandButton id="botao" value="Entrar" action="#{loginMB.doEfetuarLogin}"/>
      </h:form>
    </h:body>
</html>
{% endhighlight %}

Ao clicar no botão `Entrar` da tela será apresentado a seguinte mensagem:

<figure>
    <a href="/images/2011-04-13-jsf-tela-login-02.png"><img src="/images/2011-04-13-jsf-tela-login-02.png" alt="Tela de login com JSF."></a>
</figure>

Também podemos adicionar uma validação de login ou senha inválido. Vamos enviar uma mensagem de erro caso o login ou senha digitado são inválidos, para isto vamos utilizar um objeto `javax.faces.application.FacesMessage` e adicionar a mensagem no `FacesContext` (contexto atual do JSF), esta mensagem será apresentada depois na tela através do componente `h:messages`.

{% highlight java %}
package pbc.jsf.managedbean;

import javax.faces.application.FacesMessage;
import javax.faces.bean.ManagedBean;
import javax.faces.context.FacesContext;
import pbc.jsf.modelo.Usuario;

@ManagedBean
public class LoginMB {
  private Usuario usuario = new Usuario();

  public String doEfetuarLogin() {
    if("sakurai".equals(usuario.getLogin())
            && "123".equals(usuario.getSenha())) {
      return "principal";
    } else {
      /* Cria uma mensagem. */
      FacesMessage msg = new FacesMessage("Usuário ou senha inválido!");
      /* Obtém a instancia atual do FacesContext e adiciona a mensagem de erro nele. */
      FacesContext.getCurrentInstance().addMessage("erro", msg);
      return null;
    }
  }

  public Usuario getUsuario() {
    return usuario;
  }

  public void setUsuario(Usuario usuario) {
    this.usuario = usuario;
  }
}
{% endhighlight %}

Ao digitar um usuário ou senha inválido será apresentado a seguinte mensagem.

<figure>
    <a href="/images/2011-04-13-jsf-tela-login-03.png"><img src="/images/2011-04-13-jsf-tela-login-03.png" alt="Tela de login com JSF."></a>
</figure>

### Conteúdos relacionados

- [Criando uma aplicação com EJB e JPA](http://www.universidadejava.com.br/javaee/criando-aplicacao-ejb-jpa/)
- [Criando um template de página com JSF](http://www.universidadejava.com.br/javaee/jsf-template/)
- [Bibliotecas de tags básicas do JSF](http://www.universidadejava.com.br/javaee/jsf-tags-html/)
- [Chamando um serviço web REST com JSF](http://www.universidadejava.com.br/javaee/webservice-rest-jsf/)