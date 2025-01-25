---
layout: article
title: "JPA - Unidade de Persistência"
categories: jee
author: sakurai
date: 2011-02-20 19:25:00
tags: [java, javaee, jpa, persistencia]
published: true
excerpt: Montando uma únidade de persistência do JPA.
comments: true
image:
  teaser: teaser-jpa.png
ads: false
---

## Unidade de Persistência

A unidade de persistência é utilizada para configurar as informações referentes ao provedor do JPA (implementação da especificação JPA) e ao banco de dados, também podemos identificar as classes que serão mapeadas como entidades do banco de dados.

Para definir a unidade de persistência utilizamos um arquivo XML chamado **persistence.xml** que deve ser criado dentro da pasta **META-INF** do projeto, através deste arquivo podemos definir quantas unidades de persistência for necessárias para o projeto.

Exemplo de unidade de persistência mapeado para um banco de dados MySQL:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

  <persistence-unit name="ExemplosJPAPU" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <class>br.universidadejava.jpa.exemplo.Usuario</class>
    <properties>
      <property name="hibernate.connection.username" value="usuario"/>
      <property name="hibernate.connection.password" value="senha"/>
      <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver"/>
      <property name="hibernate.connection.url" value="jdbc:mysql://localhost:3306/ExemplosJPA"/>
      <property name="hibernate.cache.provider_class" value="org.hibernate.cache.NoCacheProvider"/>
      <property name="hibernate.show_sql" value="true"/>
    </properties>
  </persistence-unit>

</persistence>
{% endhighlight %}

Neste arquivo **persistence.xml** utilizamos a tag **\<persistence-unit\>** para definir uma unidade de persistência, onde podemos informar através da propriedade name qual seu nome (utilizado quando criamos um [EntityManager](http://www.universidadejava.com.br/jee/jpa-entitymanager/) através de um EntityManagerFactory ou quando utilizamos injeção de dependência através da anotação **javax.persistence.PersistenceContext** quando estamos no contexto Java EE) e através da propriedade **transaction-type** qual seu tipo de transação (RESOURCE_LOCAL ou JTA). Se a aplicação é Java SE utilizamos o tipo de transação **RESOURCE_LOCAL** onde programaticamente criamos as transações com o banco de dados ou utilizamos **JTA** quando acessamos um pool de conexões em um servidor web.

Dentro de uma unidade de persistência podemos utilizar a tag **\<provider\>** para informar qual a API irá fornecer uma implementação do JPA.

Quando estamos utilizando uma aplicação Java SE precisamos informar quais as classes são entidades do banco de dados através da tag **\<class\>** e também precisamos informar quais as propriedades necessárias para encontrar o banco de dados através da tag **\<properties\>**.

Quando trabalhamos com aplicações Java EE podemos criar um pool de conexões com o banco de dados no servidor web, neste caso é aconselhado utilizar o tipo de transação **JTA (Java Transaction API)** que é fornecida pelo container EJB. Também utilizamos a tag **\<jta-data-source\>** para informar a fonte do pool de conexões (nome JNDI), exemplo de **persistence.xml** para aplicações Java EE:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

  <persistence-unit name="ExemplosJPAPU" transaction-type="JTA">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <jta-data-source>jdbc/ExemplosJPA</jta-data-source>
    <properties>
      <property name="hibernate.show_sql" value="true"/>
    </properties>
  </persistence-unit>
</persistence>
{% endhighlight %}

##Criando uma unidade de persistência no NetBeans

O NetBeans fornece uma forma mais simples (next, next, finish) de criação da unidade de persistência.

Clique com o botão direito no projeto e selecione a opção **Novo → Outro...**, na tela de **Novo arquivo** selecione a **categoria Persistence** e o **tipo de arquivo Unidade de Persistência**.

Clique em **Próximo >** para definirmos as propriedades do banco de dados. Na tela de **Provedor e banco de dados** digite um nome para a unidade de persistência, escolha qual a biblioteca de persistência, defina a conexão com o banco de dados e qual a estratégia para geração de tabelas conforme abaixo:

* **O nome da unidade de persistência** é utilizado quando criamos um EntityManager através deste nome o EntityManager encontra as configurações do banco de dados.
* **A biblioteca de persistência** é a implementação do JPA utilizada para acessar o banco de dados (existem diversas implementações), neste exemplo estamos utilizando o framework Hibernate.

Para utilizarmos o Hibernate precisamos adicionar suas bibliotecas dentro do projeto, para fazer isso clique com o botão direito no projeto e selecione a opção **Propriedades**, nesta tela selecione a categoria **Bibliotecas**, após isso clique no botão **Adicionar biblioteca...**, adicione as bibliotecas **Hibernate JPA e MySQL JDBC Driver** (estamos adicionando o driver do MySQL, pois estamos utilizando ele como base de dados, caso você venha a utilizar outro banco de dados então adicione as bibliotecas especificas do banco de dados escolhido).

* **Conexão com o banco de dados** é uma URL que podemos utilizar para encontrar qual o banco de dados iremos utilizar, caso não aparece o banco de dados que vamos utilizar nesta caixa de seleção então podemos utilizar o item **Nova conexão com banco de dados...** (que esta dentro dessa caixa de seleção) para criar uma nova conexão, nesta conexão podemos escolher o driver do banco de dados, definir o host (IP), porta, nome da base de dados, usuário e senha da conexão.

Quando desenvolvemos uma aplicação web onde utilizamos um pool de conexões que pode ser criado pelo próprio servidor web, ao invés de definirmos a **Conexão com o banco de dados** precisamos definir a **Fonte de dados** (que é nosso pool de conexão criado no servidor web, veja material sobre como criar um pool de conexão no servidor web Glassfish).

**Estratégia de geração de tabelas** permite:

* **Criar** - o JPA cria a estrutura de tabelas no banco de dados.
* **Apagar e criar** - o JPA apaga a estrutura existente e cria uma estrutura nova das tabelas do banco de dados.
* **Nenhum** - Criamos manualmente as tabelas do banco de dados.

Depois de criado a unidade de persistência teremos a seguinte tela mostrando as configurações que realizamos, note que está é uma versão visual do arquivo persistence.xml.

Este é o arquivo **persistence.xml** que foi criado dentro da pasta **META-INF** do projeto:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

  <persistence-unit name="ExemplosJPAPU" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <properties>
      <property name="hibernate.connection.username" value="usuario"/>
      <property name="hibernate.connection.password" value="senha"/>
      <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver"/>
      <property name="hibernate.connection.url" value="jdbc:mysql://localhost:3306/gerenciarlivro"/>
      <property name="hibernate.cache.provider_class" value="org.hibernate.cache.NoCacheProvider"/>
    </properties>
  </persistence-unit>
</persistence>
{% endhighlight %}

Depois de criado o arquivo persistence.xml podemos definir quais as classes serão entidades do banco de dados através da tag **\<class\>** dentro da tag **\<persistence-unit\>** (obrigatório no ambiente Java SE), exemplo:

{% highlight xml %}
<persistence-unit name="ExemplosJPAPU" transaction-type="RESOURCE_LOCAL">
  <provider>org.hibernate.ejb.HibernatePersistence</provider>
  <class>br.universidadejava.jpa.modelo.Livro</class>
  <class>br.universidadejava.jpa.modelo.Pessoa</class>
  <class>br.universidadejava.jpa.modelo.Emprestimo</class>
  <properties>
    …
  </properties>
</persistence-unit>
{% endhighlight %}

## Unidade de persistência com Oracle

Quando queremos trocar o banco de dados utilizado pela aplicação, podemos fazé-lo de forma simples quando utilizamos o JPA, no exemplo anterior criamos a conexão com o MySQL se precisamos alterá-lo para Oracle, então devemos mudar o driver e as propriedades do persistence.xml:

Adicione o driver do Oracle **ojdbc7.jar** dentro do projeto (Propriedades → Bibliotecas → Adicionar JAR/Pasta → Encontre o arquivo ojdbc7.jar).

Altere as propriedades referentes ao banco de dados, como usuário, senha, nome do driver do Oracle e URL de conexão:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">
  <persistence-unit name="ExemplosJPAPU" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <properties>
      <property name="hibernate.connection.username" value="usuario"/>
      <property name="hibernate.connection.password" value="senha"/>
      <property name="hibernate.connection.driver_class" value="oracle.jdbc.driver.OracleDriver"/>
      <property name="hibernate.connection.url" value="jdbc:oracle:thin:@ipDoBanco:1521:nomeDaBase"/>
      <property name="hibernate.cache.provider_class" value="org.hibernate.cache.NoCacheProvider"/>
      <property name="hibernate.dialect" value="org.hibernate.dialect.Oracle9Dialect"/>
      <property name="hibernate.show_sql" value="true"/>
    </properties>
  </persistence-unit>
</persistence>
{% endhighlight %}


### Conteúdos relacionados

- [Alguns exercícios para você praticar com JPA](http://www.universidadejava.com.br/jee/jpa-exercicios-01/)
- [Exemplo de CRUD com JPA](http://www.universidadejava.com.br/jee/jpa-exemplo-crud/)
- [Criando entidades com JPA](http://www.universidadejava.com.br/jee/jpa-entity/)
- [Criando consultas com JPA](http://www.universidadejava.com.br/jee/jpa-query/)