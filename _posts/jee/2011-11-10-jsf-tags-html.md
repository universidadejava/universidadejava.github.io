---
layout: article
title: "JSF - Biblioteca de tags HTML"
categories: jee
author: sakurai
date: 2011-11-10 10:30:00
tags: [java]
published: true
excerpt: A biblioteca http://java.sun.com/jsf/html possui os componentes básicos para renderização de telas em HTML.
comments: true
image:
  teaser: 2011-11-10-teaser-jsf-tags-html.png
ads: false
---

A biblioteca http://java.sun.com/jsf/html possui os componentes básicos para renderização de telas em HTML.

Para utilizar esta biblioteca dentro da página xhtml, precisamos adicionar ela na propriedade da tag `html` e darmos um apelido (alias) para ela, por padrão é declarado da seguinte forma:

{% highlight xml %}
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">

</html>
{% endhighlight %}

Usamos o alias `h` para referenciar a biblioteca `html` e para utilizar algum componente desta biblioteca utilizamos a sintaxe `h: + nome da tag`, exemplo: `\<h:commandButton ...\>`.

A biblioteca `html` possui os seguintes componentes: `body`, `head`, `form`, `outputFormat`, `outpuLabel`, `outputLink`, `outputScript`, `outputStylesheet`, `outputText`, `button`, `commandButton`, `commandLink`, `link`, `graphicImage`, `inputHidden`, `inputSecret`, `inputText`, `inputTextarea`, `message`, `messages`, `selectBooleanCheckbox`, `selectManyCheckbox`, `selectManyListbox`, `selectManyMenu`, `selectOneListbox`, `selectOneMenu`, `selectOneRadio`, `dataTable`, `column`, `panelGrid` e `panelGroup`.

## Formulário

Quando montamos uma tela onde o usuário precisa entrar de alguma forma com uma informação, seja através de campos de digitação, itens de seleção, botões e outros, há a necessidade de criarmos um formulário.

Para enviar informações para o servidor, mais precisamente para uma `ManagedBean` precisamos criar um formulário e associar os campos da tela com os atributos do `ManagedBean`, exemplo:

Vamos criar um pequeno formulário para preencher informações e enviar uma mensagem para a tela com os dados recebidos.

Para isto vamos criar uma classe para representar um `Contato`:

{% highlight java %}
package br.universidadejava.jsf.modelo;

import java.util.Date;

public class Contato {
  private String nome;
  private String telefone;
  private Date dataNascimento;

  public Date getDataNascimento() {
    return dataNascimento;
  }

  public void setDataNascimento(Date dataNascimento) {
    this.dataNascimento = dataNascimento;
  }

  public String getNome() {
    return nome;
  }

  public void setNome(String nome) {
    this.nome = nome;
  }

  public String getTelefone() {
    return telefone;
  }

  public void setTelefone(String telefone) {
    this.telefone = telefone;
  }
}
{% endhighlight %}

Agora vamos criar um `ManagedBean` para armazenar os atributos e ações da página de cadastro dos contatos.

{% highlight java %}
package br.universidadejava.jsf.managedbean;

import br.universidadejava.jsf.modelo.Contato;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import javax.faces.application.FacesMessage;
import javax.faces.bean.ManagedBean;
import javax.faces.context.FacesContext;

@ManagedBean
public class ContatoMB {
  private Contato contato = new Contato();

  /**
   * Método que irá simular o cadastro do contato.
   * @return página de entrada (index.xhtml)
   */
  public String adicionarContato() {
    //Cria um formatador de datas para o padrão dd/MM/yyyy.
    DateFormat df = new SimpleDateFormat("dd/MM/yyyy");
    //Envia uma mensagem para a tela informando que foi cadastrado o contato.
    String msg = "Contato adicionado: " + contato.getNome() + " - " + contato.getTelefone() + " - " + df.format(contato.getDataNascimento());
    FacesMessage fm = new FacesMessage(msg);
    FacesContext.getCurrentInstance().addMessage("msg", fm);

    //Retorna para a página de entrada (index.xhtml).
    return "index";
  }

  public Contato getContato() {
    return contato;
  }

  public void setContato(Contato contato) {
    this.contato = contato;
  }
}
{% endhighlight %}

Criamos um simples `ManagedBean` com um atributo do tipo `Contato` e seus métodos get e set.

Vamos agora criar uma tela de cadastro de contatos que terá os campos de digitação do nome, telefone e data de nascimento de cada contato.

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core">
    <h:head>
        <title>Cadastro de Contato</title>
    </h:head>
    <h:body>
      <h:form id="formulario">
        <h:outputText id="titulo" value="Cadastro de Contatos" style="font-weight: bold;"/>
        <h:panelGrid id="dados" columns="2">
          <h:outputText id="nomeLabel" value="Nome:"/>
          <h:inputText id="nome" label="Nome" value="#{contatoMB.contato.nome}" maxlength="50"/>
          <h:outputText id="telefoneLabel" value="Telefone:"/>
          <h:inputText id="telefone" label="Telefone" value="#{contatoMB.contato.telefone}" maxlength="15"/>
          <h:outputText id="nascimentoLabel" value="Data de Nascimento:"/>
          <h:inputText id="nascimento" label="Data de Nascimento" value="#{contatoMB.contato.dataNascimento}" maxlength="10">
            <f:convertDateTime id="padraoData" pattern="dd/MM/yyyy" timeZone="America/Sao_Paulo"/>
          </h:inputText>
        </h:panelGrid>
        <h:commandButton id="cadastrar" value="Cadastrar" action="#{contatoMB.adicionarContato}"/>
        <h:messages id="msg"/>
      </h:form>
    </h:body>
</html>
{% endhighlight %}

Nesta tela usamos as tags `<h:form\>` conteúdo do formulário `</h:form>` para informar a área que terá o formulário.

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-01.png"><img src="/images/2011-11-10-jsf-tags-html-01.png" alt="Formulário com JSF."></a>
</figure>

## Seleção

Alguns componentes podem permitir que o usuário escolha uma opção, sem a necessidade de digitar alguma informação.

Com o componente `h:selectOneRadio` é possível mostrar diversas opções para que apenas uma possa ser selecionada.

Exemplo:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-02.png"><img src="/images/2011-11-10-jsf-tags-html-02.png" alt="Exemplo de uso do selectOneRadio."></a>
</figure>

O código para montar esse `h:selectOneRadio` é:

{% highlight xml %}
<h:selectOneRadio id="sexo" value="#{contatoMB.contato.sexo}">
  <f:selectItem itemLabel="Masculino" itemValue="Masculino"/>
  <f:selectItem itemLabel="Feminino" itemValue="Feminino"/>
</h:selectOneRadio>
{% endhighlight %}

As opções disponíveis nos componentes de seleção são criados com o componente `f:selectItem`, exemplo:

{% highlight xml %}
<f:selectItem itemLabel="Masculino" itemValue="Masculino"/>
{% endhighlight %}

Nele podemos especificar um texto que será apresentado na tela com a propriedade `itemLabel` e utilizamos a propriedade `itemValue` para definir o seu valor.

Uma outra forma se apresentar diversas opções onde o usuário tenha que escolher apenas uma, é utilizando o componente `h:selectOneListbox` exemplo:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-03.png"><img src="/images/2011-11-10-jsf-tags-html-03.png" alt="Exemplo de uso do selectOneListbox."></a>
</figure>

O código para montar esse `h:selectOneListbox` é:

{% highlight xml %}
<h:selectOneListbox id="categ" value="#{contatoMB.contato.categoria}" size="2">
  <f:selectItem itemLabel="Amigo" itemValue="Amigo"/>
  <f:selectItem itemLabel="Familia" itemValue="Familia"/>
  <f:selectItem itemLabel="Trabalho" itemValue="Trabalho"/>
</h:selectOneListbox>
{% endhighlight %}

O `selectOneListbox` possui diversas propriedades como `size` que informa o tamanho de elementos aparecerão na lista.

Com o componente `h:selectOneMenu` podemos montar um `ComboBox` onde é possível selecionar apenas uma opção também, exemplo:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-04.png"><img src="/images/2011-11-10-jsf-tags-html-04.png" alt="Exemplo de uso do selectOneMenu."></a>
</figure>

O código para montar esse `h:selectOneMenu` é:

{% highlight xml %}
<h:selectOneMenu id="tipo" value="#{contatoMB.contato.tipoTelefone}">
  <f:selectItem itemLabel="Cel" itemValue="Cel"/>
  <f:selectItem itemLabel="Com" itemValue="Com"/>
  <f:selectItem itemLabel="Res" itemValue="Res"/>
</h:selectOneMenu>
{% endhighlight %}

Se tivermos diversas opções onde o usuário pode selecionar mais de uma opção podemos utilizar uma lista com `h:selectManyListbox`, por exemplo:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-05.png"><img src="/images/2011-11-10-jsf-tags-html-05.png" alt="Exemplo de uso do selectManyListbox."></a>
</figure>

O código para montar esse `h:selectManyListbox` é:

{% highlight xml %}
<h:selectManyListbox id="muitasCategorias" size="4">
  <f:selectItem itemLabel="Amigo" itemValue="Amigo"/>
  <f:selectItem itemLabel="Familia" itemValue="Familia"/>
  <f:selectItem itemLabel="Trabalho" itemValue="Trabalho"/>
</h:selectManyListbox>
{% endhighlight %}

Também podemos usar o `h:selectManyCheckbox` para permitir que o usuário selecione mais de uma opção:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-06.png"><img src="/images/2011-11-10-jsf-tags-html-06.png" alt="Exemplo de uso do selectManyCheckbox."></a>
</figure>

O código para montar esse `h:selectManyCheckbox` é:

{% highlight xml %}
<h:selectManyCheckbox id="redesSociais" value="#{contatoMB.contato.redesSociais}">
  <f:selectItem itemLabel="Google+" itemValue="Google+"/>
  <f:selectItem itemLabel="Twitter" itemValue="Twitter"/>
  <f:selectItem itemLabel="Facebook" itemValue="Facebook"/>
  <f:selectItem itemLabel="LinkedIn" itemValue="LinkedIn"/>
</h:selectManyCheckbox>
{% endhighlight %}

Para armazenar quais as opções selecionadas podemos utilizar um vetor de `private String[] redesSociais;` , exemplo:

{% highlight java %}
private String[] redesSociais;

public String[] getRedesSociais() { return redesSociais; }

public void setRedesSociais(String[] redesSociais){
  this.redesSociais = redesSociais;
}
{% endhighlight %}

Um checkbox também pode ser utilizado para obter valores do tipo `true` (verdadeiro) ou `false` (falso), para isto utilizamos o `h:selectBooleanCheckbox`.

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-07.png"><img src="/images/2011-11-10-jsf-tags-html-07.png" alt="Exemplo de uso do selectBooleanCheckbox."></a>
</figure>

O código para montar esse `h:selectManyCheckbox` é:

{% highlight xml %}
<h:selectBooleanCheckbox id="statusAtivo" value="#{contatoMB.contato. ativo}"/>
{% endhighlight %}

Quando ele estiver selecionado significa `true` (verdadeiro), caso contrário significa `false` (falso), para armazenar este valor podemos utilizar um atributo `private boolean ativo;`, exemplo:

{% highlight java %}
private boolean ativo;

public boolean isAtivo() { return ativo; }

public void setAtivo(boolean ativo) { this.ativo = ativo; }
{% endhighlight %}

## Tabela

Podemos definir uma tabela utilizando o componente `h:dataTable`, exemplo:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-08.png"><img src="/images/2011-11-10-jsf-tags-html-08.png" alt="Exemplo de uso do dataTable."></a>
</figure>

Para criar uma tabela podemos passar para ela um vetor ou lista através da propriedade `value` e com a propriedade `var` é criado uma variável para representar cada elemento do vetor ou lista, exemplo:

{% highlight xml %}
<h:dataTable id="contatos" value="#{contatoMB.contatos}" var="c" style="width: 100%">
{% endhighlight %}

Foi passado para o `dataTable` uma lista de objetos `Contato` através da propriedade `value="#{contatoMB.contatos}"` e para cada objeto `Contato` da lista será armazenado em uma variável chamada `c` através da propriedade `var="c"` para ser utilizada dentro da tabela.

Para definir os valores de cada coluna podemos utilizar o componente `h:column`, exemplo:

{% highlight xml %}
 <h:column id="columnNome">
    <f:facet name="header">
      <h:outputText id="headerNome" value="Nome"/>
    </f:facet>
    <h:outputText id="valorNome" value="#{c.nome}"/>
  </h:column>
{% endhighlight %}

Note que dentro da coluna estamos usando um `f:facet` que é usado para representar um cabecalho (header) ou rodapé (footer) da coluna.

O exemplo completo da cadastro de contato vai ficar da seguinte forma:

### Contato

Na classe `Contato` vamos adicionar mais alguns atributos para representar todos os valores que podem ser informados pelo usuário.

{% highlight java %}
package br.universidadejava.jsf.modelo;

import java.util.Date;

public class Contato {
  private String nome;
  private String tipoTelefone;
  private String telefone;
  private Date dataNascimento;
  private String sexo;
  private String categoria;
  private String[] redesSociais;
  private boolean ativo;

  public Date getDataNascimento() { return dataNascimento; }
  public void setDataNascimento(Date dataNascimento) {
    this.dataNascimento = dataNascimento;
  }

  public String getNome() { return nome; }
  public void setNome(String nome) { this.nome = nome; }

  public String getTelefone() { return telefone; }

  public void setTelefone(String telefone) {
    this.telefone = telefone;
  }

  public boolean isAtivo() { return ativo; }
  public void setAtivo(boolean ativo) { this.ativo = ativo; }

  public String getTipoTelefone() { return tipoTelefone; }

  public void setTipoTelefone(String tipoTelefone) {
    this.tipoTelefone = tipoTelefone;
  }

  public String getSexo() { return sexo; }
  public void setSexo(String sexo) { this.sexo = sexo; }

  public String getCategoria() { return categoria; }
  public void setCategoria(String categoria) {
    this.categoria = categoria;
  }

  public String[] getRedesSociais() { return redesSociais; }

  public void setRedesSociais(String[] redesSociais) {
    this.redesSociais = redesSociais;
  }

  public String getRedesSociaisFormatadas() {
    String redes = "";

    if(getRedesSociais() != null) {
      for(int cont = 0; cont < getRedesSociais().length; cont++) {
        redes += getRedesSociais()[cont];

        if(cont < getRedesSociais().length - 1) {
          redes += "; ";
        }

      }
    }

    return redes;
  }
}
{% endhighlight %}

### ContatoMB

No `ManagedBean` vamos adicionar agora a lista de contatos que será apresentado na tela no formato de tabela.

{% highlight java %}
package br.universidadejava.jsf.managedbean;

import br.universidadejava.jsf.modelo.Contato;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.List;
import javax.faces.application.FacesMessage;
import javax.faces.bean.ManagedBean;
import javax.faces.bean.SessionScoped;
import javax.faces.context.FacesContext;

@ManagedBean
@SessionScoped
public class ContatoMB {
  private Contato contato = new Contato();
  private List<Contato> contatos = new ArrayList<Contato>();

  /**
   * Método que irá simular o cadastro do contato.
   * @return página de entrada (index.xhtml)
   */
  public String adicionarContato() {
    contatos.add(contato);

    //Cria um formatador de datas para o padrão dd/MM/yyyy.
    DateFormat df = new SimpleDateFormat("dd/MM/yyyy");
    /* Envia uma mensagem para a tela informando que foi cadastrado o contato. */
    String msg = "Contato adicionado: " + contato.getNome() + " - " + contato.getTelefone() + " - " + df.format(contato.getDataNascimento());
    FacesMessage fm = new FacesMessage(msg);
    FacesContext.getCurrentInstance().addMessage("msg", fm);

    contato = new Contato();

    //Retorna para a página de entrada (index.xhtml).
    return "index";
  }

  public Contato getContato() {
    return contato;
  }

  public void setContato(Contato contato) {
    this.contato = contato;
  }

  public List<Contato> getContatos() {
    return contatos;
  }

  public void setContatos(List<Contato> contatos) {
    this.contatos = contatos;
  }
}
{% endhighlight %}

### index.xhtml

E na tela vamos adicionar mais alguns componentes de seleção do usuário e a tabela para apresentar todos os contratos cadastrados.

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core">
  <h:head>
    <title>Cadastro de Contato</title>
  </h:head>
  <h:body>
    <h:form id="formulario">
      <h:outputText id="titulo" value="Cadastro de Contatos" style="font-weight: bold;"/>
      <h:panelGrid id="dados" columns="2">
        <h:outputText id="nomeLabel" value="Nome:"/>
        <h:inputText id="nome" label="Nome" value="#{contatoMB.contato.nome}" maxlength="50"/>
        <h:outputText id="telefoneLabel" value="Telefone:"/>
        <h:panelGroup id="groupTelefone">
          <h:selectOneMenu id="tipoTelefone" value="#{contatoMB.contato.tipoTelefone}">
            <f:selectItem itemLabel="Cel" itemValue="Cel"/>
            <f:selectItem itemLabel="Com" itemValue="Com"/>
            <f:selectItem itemLabel="Res" itemValue="Res"/>
          </h:selectOneMenu>
          <h:inputText id="telefone" label="Telefone" value="#{contatoMB.contato.telefone}" maxlength="15"/>
        </h:panelGroup>
        <h:outputText id="nascimentoLabel" value="Data de Nascimento:"/>
        <h:inputText id="nascimento" label="Data de Nascimento" value="#{contatoMB.contato.dataNascimento}" maxlength="10">
          <f:convertDateTime id="padraoData" pattern="dd/MM/yyyy" timeZone="America/Sao_Paulo"/>
        </h:inputText>
        <h:outputText id="sexoLabel" value="Sexo:"/>
        <h:selectOneRadio id="sexo" value="#{contatoMB.contato.sexo}">
          <f:selectItem itemLabel="Masculino" itemValue="Masculino"/>
          <f:selectItem itemLabel="Feminino" itemValue="Feminino"/>
        </h:selectOneRadio>
        <h:outputText id="categoriaLabel" value="Categoria:"/>
        <h:selectOneListbox id="categoria" value="#{contatoMB.contato.categoria}" size="2">
          <f:selectItem itemLabel="Amigo" itemValue="Amigo"/>
          <f:selectItem itemLabel="Familia" itemValue="Familia"/>
          <f:selectItem itemLabel="Trabalho" itemValue="Trabalho"/>
        </h:selectOneListbox>
        <h:outputText id="redesSociaisLabel" value="Redes Sociais:"/>
        <h:selectManyCheckbox id="redesSociais" value="#{contatoMB.contato.redesSociais}">
          <f:selectItem itemLabel="Google+" itemValue="Google+"/>
          <f:selectItem itemLabel="Twitter" itemValue="Twitter"/>
          <f:selectItem itemLabel="Facebook" itemValue="Facebook"/>
          <f:selectItem itemLabel="LinkedIn" itemValue="LinkedIn"/>
        </h:selectManyCheckbox>
        <h:outputText id="statusLabel" value="Status:"/>
        <h:selectBooleanCheckbox id="ativostatus" value="#{contatoMB.contato.ativo}"/>
      </h:panelGrid>
      <h:commandButton id="cadastrar" value="Cadastrar" action="#{contatoMB.adicionarContato}"/>
      <h:messages id="msg"/>
      <h:dataTable id="contatos" value="#{contatoMB.contatos}" var="c" style="width: 100%">
        <h:column id="columnNome">
          <f:facet name="header">
            <h:outputText id="headerNome" value="Nome"/>
          </f:facet>
          <h:outputText id="valorNome" value="#{c.nome}"/>
        </h:column>
        <h:column id="columnTipoTelefone">
          <f:facet name="header">
            <h:outputText id="headerTipoTelefone" value="Tipo"/>
          </f:facet>
          <h:outputText id="valorTipoTelefone" value="#{c.tipoTelefone}"/>
        </h:column>
        <h:column id="columnTelefone">
          <f:facet name="header">
            <h:outputText id="headerTelefone" value="Telefone"/>
          </f:facet>
          <h:outputText id="valorTelefone" value="#{c.telefone}"/>
        </h:column>
        <h:column id="columnDataNascimento">
          <f:facet name="header">
            <h:outputText id="headerDataNascimento" value="Data de Nascimento"/>
          </f:facet>
          <h:outputText id="valorDataNascimento" value="#{c.dataNascimento}">
            <f:convertDateTime id="padraoDataNascimento" pattern="dd/MM/yyyy" timeZone="America/Sao_Paulo"/>
          </h:outputText>
        </h:column>
        <h:column id="columnSexo">
          <f:facet name="header">
            <h:outputText id="headerSexo" value="Sexo"/>
          </f:facet>
          <h:outputText id="valorSexo" value="#{c.sexo}"/>
        </h:column>
        <h:column id="columnCategoria">
          <f:facet name="header">
            <h:outputText id="headerCategoria" value="Categoria"/>
          </f:facet>
          <h:outputText id="valorCategoria" value="#{c.categoria}"/>
        </h:column>
        <h:column id="columnRedes ">
          <f:facet name="header">
            <h:outputText id="headerRedes" value="Redes Sociais"/>
          </f:facet>
          <h:outputText id="valorRedesSociais" value="#{c.redesSociaisFormatadas}"/>
        </h:column>
        <h:column id="columnStatus">
          <f:facet name="header">
            <h:outputText id="headerStatus" value="Status"/>
          </f:facet>
          <h:outputText id="valorStatus" value="#{c.ativo ? 'Ativo' : 'Inativo'}"/>
        </h:column>
      </h:dataTable>
    </h:form>
  </h:body>
</html>
{% endhighlight %}

A tela da aplicação ficará assim depois de alguns cadastros:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-09.png"><img src="/images/2011-11-10-jsf-tags-html-09.png" alt="Exemplo de uso das tags da biblioteca HTML."></a>
</figure>

## Exercícios

### Exercício 1

Crie um formulário para agendar a data/hora para fazer o café, dado o seguinte formulário:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-10.png"><img src="/images/2011-11-10-jsf-tags-html-10.png" alt="Exercício agendar café."></a>
</figure>

Ao clicar no botão `Agendar`, apresentar uma tela com a seguinte informação:

<figure>
    <a href="/images/2011-11-10-jsf-tags-html-11.png"><img src="/images/2011-11-10-jsf-tags-html-11.png" alt="Exercício agendar café."></a>
</figure>


### Conteúdos relacionados

- [Criando uma tela de login com JSF](http://www.universidadejava.com.br/javaee/jsf-tela-login/)
- [Criando um template de página com JSF](http://www.universidadejava.com.br/javaee/jsf-template/)
- [Chamando um serviço web REST com JSF](http://www.universidadejava.com.br/javaee/webservice-rest-jsf/)
- [Criando uma aplicação com EJB e JPA](http://www.universidadejava.com.br/javaee/criando-aplicacao-ejb-jpa/)