---
layout: article
title: "Introdução ao JavaServer Faces 2.0"
categories: jee
author: sakurai
date: 2011-04-13 19:57:00
tags: [java]
published: true
excerpt: JavaServer Faces (JSF) é um framework de interface de usuário para desenvolvimento de aplicações web Java.
comments: true
image:
  teaser: 2011-04-13-teaser-jsf-introducao.png
ads: false
---

JavaServer Faces (JSF) é um framework de interface de usuário para desenvolvimento de aplicações web Java. Ele possui um design para facilitar significativamente o trabalho de escrever e manter aplicações wen que rodam em servidores de aplicações Java e renderizam suas interfaces de usuário (telas) de volta para o cliente (navegador) solicitante. (JSR 314: JavaServer Faces 2.0 Specification)

O JSF utiliza o padrão de projeto Model-View-Controller (MVC) para facilitar o desenvolvimento das telas, onde:

* No View teremos as páginas .xhtml que serão responsáveis apenas pela renderização das páginas;
* No Model teremos o ManagedBean que é uma classe que irá executar as ações da página e possuir os atributos que serão apresentados pela tela;
* E o Controller é gerenciado pelo JSF, mais especificamente pelo FacesServlet que irá redirecionar as chamadas de páginas, fazer as conversões das tas JSF para HTML e outros.

Uma das vantagens de utilizar o JSF é que a pessoa que escreve as páginas usando as tags do JSF não precisa saber programar em Java, portanto você pode deixar que o web designer desenhe o layout de todas as páginas enquanto outra pessoa pode ir criando as ações das telas e a parte de negócio da aplicação.


## Porque utilizar o JSF?

Facilidade de desenvolver aplicações web:

No desenvolvimento das telas os componentes do JSF são escritos como tags simples, onde uma pessoa que não programa nada em Java pode muito bem criar ótimas telas web.

Na tela todos os valores são apenas textos e nem sempre nos objetos temos apenas atributos String, com o JSF os valores dos componentes podem ser diretamente associados aos atributos objetos Java criados sendo feita automaticamente a busca (get) e alteração (set) de seus valores, sem tem quer fazer diversas conversões de String para os tipos de dados comuns (boolean, int, long, double e outros).

A conversão de tipos conhecidos é feita automaticamente, também é possível usar os conversores prontos para por exemplo Date ou BigDecimal e também criar um conversor para os seus próprios objetos.

Podemos validar o valor de entrada dos inputText (caixas de texto), para saber se um campo obrigatório foi preenchido, se a quantidade de caracteres mínimas foi digitado, se a quantidade de caracteres não ultrapassou o limite e outros. Podemos usar componentes para validação do próprio JSF ou criarmos os nossos próprios componentes de validação personalizados.

Com arquivos .properties podemos internacionalizar a aplicação para diversos idiomas. Os properties são arquivos de textos baseados em chave=valor. A partir de uma chave podemos obter o seu valor, então quando precisamos adicionar uma novo idioma ao projeto como o inglês, por exemplo: podemos adicionar no final do arquivo a sigla en (uj_en.properties) e traduzimos a parte do valor das propriedades para inglês, automaticamente quando o navegador (browser) estiver configurado com o idioma inglês o JSF irá ler o arquivo de propriedades com a final _en e pegar todos os textos traduzidos.

Caso algum componente não faça tudo o que você precisa é possível facilmente criar uma subclasse deste componente e melhorar ou adicionar novas funcionalidades, podemos também criar os nossos próprios componentes de tela.

É possível criar páginas de modelo (template) com a estrutura base para outras telas, aproveitando o código e diminuindo a alteração do código em muitas telas.

Os componentes podem funcionar de forma assíncrona com AJAX, fazendo requisições e alterando a menor quantidade possível de informações da tela.

Existem muitos componentes de terceiros como o RichFaces, MyFaces, PrimeFaces IceFaces e outros que podem ser adicionados ao projeto para aumentar ainda mais a quantidade de componentes de tela e funcionalidade.


## Criando um Hello World com JSF 2.0 no NetBeans

No Menu Arquivo -> Novo Projeto, selecione a Categoria: Java Web e escolha um Projeto: Aplicação Web, clique em Próximo.

Na tela Novo Aplicação Web defina o nome do projeto HelloWorld e escolha um local do projeto onde será gerado o código fonte da aplicação, clique em Próximo.

Informe o Servidor onde será publicado a aplicação, neste exemplo estou usando o Glassfish Server 3.1, informe a versão do Java EE: Java EE 6 Web e o caminho do contexto: `/HelloWorld`, clique em Próximo.

> O caminho do contexto é o nome da aplicação que será chamado pelo navegador (browser) neste caso para acessar a aplicação, utilize a URL `http://localhost:8000/HelloWorld`.

Selecione o framework JavaServer Faces, irá aparecer um painel de configuração do JavaServer Faces, selecione a biblioteca JSF 2.0 e clique em Finalizar.

Após criar o projeto web, clique com o botão direito do mouse sobre ele e escolha o item Executar no menu. Neste momento sua aplicação será compactada gerando um arquivo `HelloWorld.war` (Web Application Archive), publicada dentro do servidor web Glassfish e o NetBeans automaticamente inicia seu navegador padrão (browser) com a aplicação.

<figure>
    <a href="/images/2011-04-13-jsf-introducao-01.png"><img src="/images/2011-04-13-jsf-introducao-01.png" alt="Novo projeto JSF."></a>
</figure>

O NetBeans criou a aplicação web contendo uma página inicial chamada `index.xhtml` e o arquivo de configuração da aplicação `web.xml` dentro da pasta `META-INF`.

<figure>
    <a href="/images/2011-04-13-jsf-introducao-02.png"><img src="/images/2011-04-13-jsf-introducao-02.png" alt="Estrutura projeto JSF."></a>
</figure>

O arquivo index.xhtml possui uma declaração simples da página com as tags `h:head` para informar a área de cabeçalho da página (representando a tag <head> do HTML) e `h:body` para informar a área do corpo da página (representando a tag <body> do HTML).

{% highlight xml %}
<?xml version='1.0' encoding='UTF-8' ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://java.sun.com/jsf/html">
    <h:head>
        <title>Facelet Title</title>
    </h:head>
    <h:body>
      Hello from Facelets
    </h:body>
</html>
{% endhighlight %}

O arquivo `web.xml` possui as informações de configuração da aplicação web, note que nele temos:

* Declaração de um parâmetro de contexto da aplicação chamado `PROJECT_STAGE` usado para informar o estagio do projeto (novo do JSF 2.0);
* Declaração da `FacesServlet` que irá tratar todas as requisições da aplicação que possuem o caminho `faces/` após o nome da aplicação (ex: `http://localhost:8080/HelloWorld/faces/index.xhtml`);
* A configuração de timeout padrão de 30 minutos;
* Definição da página inicial do projeto `faces/index.xhtml`.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">
    <context-param>
        <param-name>javax.faces.PROJECT_STAGE</param-name>
        <param-value>Development</param-value>
    </context-param>
    <servlet>
        <servlet-name>Faces Servlet</servlet-name>
        <servlet-class>javax.faces.webapp.FacesServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>Faces Servlet</servlet-name>
        <url-pattern>/faces/*</url-pattern>
    </servlet-mapping>
    <session-config>
        <session-timeout>
            30
        </session-timeout>
    </session-config>
    <welcome-file-list>
        <welcome-file>faces/index.xhtml</welcome-file>
    </welcome-file-list>
</web-app>
{% endhighlight %}


### Conteúdos relacionados

- [Criando uma tela de login com JSF](http://www.universidadejava.com.br/javaee/jsf-tela-login/)
- [Criando um template de página com JSF](http://www.universidadejava.com.br/javaee/jsf-template/)
- [Bibliotecas de tags básicas do JSF](http://www.universidadejava.com.br/javaee/jsf-tags-html/)
- [Chamando um serviço web REST com JSF](http://www.universidadejava.com.br/javaee/webservice-rest-jsf/)