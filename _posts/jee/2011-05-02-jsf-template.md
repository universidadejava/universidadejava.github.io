---
layout: article
title: "JSF - Template (Facelets)"
categories: jee
author: sakurai
date: 2011-05-02 22:45:00
tags: [java]
published: true
excerpt: Templates são layouts de páginas que podem ser reaproveitadas.
comments: true
image:
  teaser: 2011-05-02-teaser-jsf-template.png
ads: false
---

## Introdução ao Facelets

Templates são layouts de páginas que podem ser reaproveitadas, podemos criar uma página padrão (template) para o sistema e depois apenas alterar as áreas dessa página que for necessária, por exemplo: podemos criar um template com um cabeçalho, menu e conteúdo (como imagem a seguir) e podemos definir que estas três áreas podem ser alteradas por qualquer outra página que siga este template.

<figure>
    <a href="/images/2011-05-02-jsf-template-01.png"><img src="/images/2011-05-02-jsf-template-01.png" alt="Template."></a>
</figure>

Na versão 2.0 do JSF foi incorporado o projeto Facelets que possui tags para criação de templates, para utilizá-lo basta importar a biblioteca de tags http://java.sun.com/jsf/facelets, exemplo:

{% highlight xml %}
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets">

</html>
{% endhighlight %}

Normalmente a tag do facelets ganha um alias (apelido) chamado `ui`, para através dele usar as tags desta biblioteca. O facelets possui as seguintes bibliotecas: `component`, `composition`, `debug`, `decorate`, `define`, `fragment`, `include`, `insert`, `param`, `remove` e `repeat`.

## ui:insert

Para delimitar uma área do template que pode ser alterada pelas páginas que utilizarem este template iremos utilizar a tag `insert`, exemplo:

{% highlight xml %}
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets">
  <h:head>
    <title>JSF 2.0 - Exemplo de template</title>
  </h:head>
  <h:body>
    <ui:insert name="cabecalho">
      <h1>Template Web</h1>
    </ui:insert>
    <ui:insert name="conteudo">
      Aula de JSF 2.0 - criando templates de página
    </ui:insert>
    <ui:insert name="rodape">
      Prof. Rafael Guimarães Sakurai
    </ui:insert>
  </h:body>
</html>
{% endhighlight %}

Neste exemplo criamos uma página com três áreas que podem ser alteradas, para cada área que definimos com a tag `insert` precisamos definir também um nome para que possamos informar na página que irá usar este template qual a parte da página deve ser alterada.

## ui:composition

Quando criamos uma página que irá seguir um template precisamos especificar qual será o arquivo que possui o template, para isto iremos utilizar a tag `composition`, exemplo:

{% highlight xml %}
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets">
  <ui:composition template="/principal.xhtml">

  </ui:composition>
</html>
{% endhighlight %}

Quando utilizamos um template não há necessidade de especificar novamente as tags `h:head`, `h:body` ou qualquer outra tag que já tem no template, pois está página agora terá as mesmas tags para montar seu layout.

## ui:define

Dentro da página que segue o template podemos alterar qualquer área especificada pelas tags `insert`, para isto iremos utilizar a tag `define` e informar qual o nome da tag `insert` deve ser alterado, exemplo:

{% highlight xml %}
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets">
  <ui:composition template="/principal.xhtml">
    <ui:define name="conteudo">
      <h1>Cadastro de Usuário</h1>
    </ui:define>
  </ui:composition>
</html>
{% endhighlight %}

Exemplo:

Vamos criar uma aplicação simples que possui uma tela principal, esta tela será composta por um cabeçalho com o título da página, um menu com a opção de cadastrar usuários e uma área de conteúdo:

<figure>
    <a href="/images/2011-05-02-jsf-template-02.png"><img src="/images/2011-05-02-jsf-template-02.png" alt="Exemplo de aplicação web com template."></a>
</figure>

E também vamos criar uma página que possui o cadastro de usuário e um menu para voltar para a página inicial:

<figure>
    <a href="/images/2011-05-02-jsf-template-03.png"><img src="/images/2011-05-02-jsf-template-03.png" alt="Exemplo de aplicação web com template."></a>
</figure>

Crie uma aplicação web chamada `TemplateWeb` e utilize o framework JavaServer Faces 2.0.

Na página `index.xhtml` vamos criar o template (modelo) que será seguido depois pela página `usuario.xhtml`:

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets">
  <h:head>
    <title>Template Web</title>
  </h:head>
  <h:body>
    <h:form id="form">
      <table width="100%">
        <tr>
          <td colspan="2" align="center">
            <ui:insert name="cabecalho">
              <h1>Template Web</h1>
            </ui:insert>
          </td>
        </tr>
        <tr>
          <td width="200px" valign="top">
            <ui:insert name="menu">
              <h1>Menu</h1>
              <h:panelGrid columns="1">
                <h:commandLink value="Cadastro de Usuário" action="usuario.xhtml"/>
              </h:panelGrid>
            </ui:insert>
          </td>
          <td>
            <ui:insert name="conteudo">
              Conteúdo
            </ui:insert>
          </td>
        </tr>
      </table>
    </h:form>
  </h:body>
</html>
{% endhighlight %}

Note que importamos a biblioteca do facelets e definimos as áreas que podem ser alteradas.

Agora vamos criar a página que seguirá o template e terá o cadastro de usuário, para isto primeiro vamos criar uma classe para representar o `Usuário`:

{% highlight xml %}
package br.universidadejava.jsf.modelo;

import java.io.Serializable;

public class Usuario implements Serializable {
  private static final long serialVersionUID = 7371518231621030644L;
  private String nome;
  private String cpf;
  private String email;
  private String perfil;

  public String getCpf() {
    return cpf;
  }

  public void setCpf(String cpf) {
    this.cpf = cpf;
  }

  public String getEmail() {
    return email;
  }

  public void setEmail(String email) {
    this.email = email;
  }

  public String getNome() {
    return nome;
  }

  public void setNome(String nome) {
    this.nome = nome;
  }

  public String getPerfil() {
    return perfil;
  }

  public void setPerfil(String perfil) {
    this.perfil = perfil;
  }
}
{% endhighlight %}

E também vamos criar uma classe que será o `ManagedBean` que controlara as ações da página de cadastro do usuário.

{% highlight xml %}
package br.universidadejava.jsf.managedbean;

import java.io.Serializable;
import java.util.ArrayList;
import java.util.List;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import br.universidadejava.jsf.modelo.Usuario;

@ManagedBean
@SessionScoped
public class UsuarioMB implements Serializable {
  private static final long serialVersionUID = 356240640918386194L;

  // Objeto usuário utilizado no formulário.
  private Usuario usuario = new Usuario();
  /* Lista de objetos Usuario para simular o cadastro dos
     usuários. */
  private List<Usuario> usuarios = new ArrayList<Usuario>();

  /**
   * Ação para o botão salvar.
   */
  public void doAdicionarUsuario() {
    System.out.println("Adicionar usuario " + usuario.getNome());
    usuarios.add(usuario);
    usuario = new Usuario();
  }

  public Usuario getUsuario() {
    return usuario;
  }

  public void setUsuario(Usuario usuario) {
    this.usuario = usuario;
  }

  public List<Usuario> getUsuarios() {
    return usuarios;
  }

  public void setUsuarios(List<Usuario> usuarios) {
    this.usuarios = usuarios;
  }
}
{% endhighlight %}

Agora vamos criar a página de cadastro chamada `usuario.xhtml`:

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      xmlns:f="http://java.sun.com/jsf/core">
  <ui:composition template="/index.xhtml">
    <ui:define name="menu">
      <h:commandLink value="Inicio"  action="/index.xhtml"/>
    </ui:define>
    <ui:define name="conteudo">
      <h1>Cadastro de Usuário</h1>
      <h:panelGrid id="dadosUsuario" columns="2">
        <h:outputText id="nomeText" value="Nome:"/>
        <h:inputText id="nome" label="Nome" value="#{usuarioMB.usuario.nome}" maxlength="200" size="100"/>
        <h:outputText id="cpfText" value="CPF:"/>
        <h:inputText id="cpf" label="CPF" value="#{usuarioMB.usuario.cpf}" maxlength="14" size="20"/>
        <h:outputText id="emailText" value="Email:"/>
        <h:inputText id="email" label="Email" value="#{usuarioMB.usuario.email}" maxlength="50" size="50"/>
        <h:outputText id="perfilText" value="Perfil:"/>
        <h:selectOneMenu id="perfil" label="Perfil" value="#{usuarioMB.usuario.perfil}">
          <f:selectItem itemLabel="Administrador" itemValue="Administrador"/>
          <f:selectItem itemLabel="Supervisor" itemValue="Supervisor"/>
          <f:selectItem itemLabel="Vendedor" itemValue="Vendedor"/>
        </h:selectOneMenu>
      </h:panelGrid>
      <br/>
      <h:commandButton id="salvar" value="Salvar" actionListener="#{usuarioMB.doAdicionarUsuario}" >
        <f:ajax execute="@form" render="usuarios dadosUsuario" />
      </h:commandButton>
      <br/><br/>
      <h:dataTable id="usuarios" value="#{usuarioMB.usuarios}" var="usuario">
        <h:column>
          <f:facet name="header">
            <h:outputText id="columnNome" value="Nome"/>
          </f:facet>
          <h:outputText id="nomeUsuario" value="#{usuario.nome}"/>
        </h:column>
        <h:column>
          <f:facet name="header">
            <h:outputText id="columnCPF" value="CPF"/>
          </f:facet>
          <h:outputText id="cpfUsuario" value="#{usuario.cpf}"/>
        </h:column>
        <h:column>
          <f:facet name="header">
            <h:outputText id="columnEmail" value="Email"/>
          </f:facet>
          <h:outputText id="emailUsuario" value="#{usuario.email}"/>
        </h:column>
        <h:column>
          <f:facet name="header">
            <h:outputText id="columnPerfil" value="Perfil"/>
          </f:facet>
          <h:outputText id="perfilUsuario" value="#{usuario.perfil}"/>
        </h:column>
      </h:dataTable>
    </ui:define>
  </ui:composition>
</html>
{% endhighlight %}

Note que alteramos o conteúdo das áreas menu e conteudo através da tag `define`.

### Para se divertir um pouco:

* Se for necessário alterar o titulo da página que aparece no navegador, de acordo com a página atual como seria? Altere esta aplicação e teste.
* Qual a vantagem de ter usado o `h:form` no template?
* Será que se os campos do formulário na página de cadastrado fossem obrigatórios (`required=true`) o link para a página inicial iria funcionar?


### Conteúdos relacionados

- [Bibliotecas de tags básicas do JSF](http://www.universidadejava.com.br/jee/jsf-tags-html/)
- [Chamando um serviço web REST com JSF](http://www.universidadejava.com.br/jee/webservice-rest-jsf/)
- [Criando uma aplicação com EJB e JPA](http://www.universidadejava.com.br/jee/criando-aplicacao-ejb-jpa/)
- [Criando um Web Service SOAP com EJB](http://www.universidadejava.com.br/jee/criando-webservice-com-ejb/)