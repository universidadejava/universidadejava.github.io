---
layout: article
title: "Conhecendo e previnindo a vulnerabilidade SQL Injection"
categories: outros
author: sakurai
date: 2020-05-24 10:15:00
tags: [java, vulnerabilidade, sql, injection]
published: true
excerpt: 
comments: true
image:
  teaser: 2020-05-24-teaser-vulnerabilidade-sql-injection.png
ads: false
---

## SQL Injection

Vulnerabilidade que permite ao atacante manipular códigos SQL passados da aplicação para o banco de dados, com objetivo de extrair, alterar ou até mesmo causar danos.

Consiste em influenciar o que é encaminhado ao banco de dados, explorando sintaxe e funcionalidades da linguagem SQL ou vulnerabilidades do banco de dados.

Os principais bancos de dados relacionais, como: MySQL, Oracle, SQL Server e DB2, utilizam o linguagem SQL para manipular os dados e estruturas.

A maioria das aplicações precisa de algum modo guardar as informações e para isso fazem uso de sistemas de banco de dados. O atacante aproveita do mau uso do banco de dados feito pelas aplicações para explorar vulnerabilidades com a técnica SQL Injection.

### Exemplo 1

Queremos consultar livros por título e temos a seguinte consulta SQL:

{% highlight sql %}
SELECT * FROM Livro WHERE titulo like '%Harry Potter%';
{% endhighlight %}

Essa consulta obtém todos os livros que apresentam no título o texto `'%Harry Potter%'`. Os sinais de `%` indicam que pode haver mais texto antes ou depois do texto **Harry Potter**.

Podemos permitir que o usuário informe qual o texto que deve conter no título:

{% highlight sql %}
"SELECT * FROM Livro WHERE titulo like \'%" + texto + "%\';"
{% endhighlight %}

Então, temos uma variável `texto` que é informada pelo usuário e concatenada na consulta SQL.

O que ocorre se o usuário informar no texto `" '; DELETE FROM Livro; "` ?

Teremos a seguinte consulta SQL:

{% highlight sql %}
"SELECT * FROM Livro WHERE titulo like \'% '; DELETE FROM Livro; %\';"
{% endhighlight %}

Essa instrução SQL diz para consultar todos os livros que possuem um espaço em branco e depois apagar a tabela Livro inteira.

Existem outras técnicas para explorar a base de dados para encontrar todas as tabelas, mas quem quer causar problemas pode muito bem explorar essa vulnerabilidade até apagar todo o banco de dados.

### Exemplo 2

Realizando a autenticação de um usuário, se a consulta retornar o login é porque é um usuário válido:

{% highlight sql %}
"SELECT login FROM Usuario WHERE login = '" + var1 + "' AND senha = '" + var2 + "';"
{% endhighlight %}

Um usuário correto poderia informar dados corretos como: `var1 = "anderson"` e `var2 = "coordenador"`.

{% highlight sql %}
SELECT login FROM Usuario
WHERE login = 'anderson' AND senha = 'coordernador';
{% endhighlight %}

Mas um usuário mal intencionado, poderia passar informações que o fizessem ter acesso indevido, como `var1 = "%A%"` e `var2 = "sqlinjection' or '2'= '2"`.

{% highlight sql %}
SELECT login FROM Usuario
WHERE login = '%A%' AND senha = 'sqlinjection' or '2' = '2';
{% endhighlight %}

Essa consulta vai retornar todos os usuários com a letra `A` no login, ignorando a senha porque a condição `or '2' = '2'` sempre será verdadeira.

## Como encontrar SQL Injection na aplicação

Para verificar se a aplicação possui a vulnerabilidade SQL Injection, duas formas simples de verificar são:

1. Procure por concatenação de SQL com conteúdos informados pelos usuários.

2. Procure todas variáveis que seu conteúdo são informados pelo usuário, e verifique quais variáveis não são validadas.


## Previnindo a vulnerabilidade SQL Injection

Cinco pontos para você 

1. Definir tamanho mínimo e máximo para cada campo informado pelo usuário. 
2. Validar o conteúdo informado pelo usuário. Essa validação deve ocorrer tanto no front (nas telas) como no back-end (acesso ao banco de dados ou outros pontos de uso da informação) da aplicação.
3. Não concatenar diretamente o conteúdo informado com o SQL, a seguir mostro como fazer isso na aplicação Java.
4. O usuário de banco de dados que é utilizado pela aplicação precisa ter controle de nível de acesso. Verifique quais permissões são necessárias para executar a aplicação, na maioria das vezes não deveria ter permissão para deletar uma tabela por exemplo.
5. Auditoria do código-fonte regularmente. Revise o código regularmente, procure sempre pelos quatro pontos mencionados anteriormente e divulgue entre os desenvolvedores essa vulnerabilidade.

## Como evitar o uso incorreto do banco de dados no Java

Na aplicação Java quando usamos JDBC para acessar o banco de dados, encontramos a opção de fazer isso usando a classe `Statement`:

{% highlight java %}
String query = "SELECT * FROM Usuario WHERE login = '" + var1 + "' AND senha = '" + var2 + "'";
Statement stm = con.createStatement();
ResultSet rs = stm.executeQuery(query);
{% endhighlight %}

Mas nessa abordagem temos o problema de concatencar os dados informados pelo usuário com as instruções SQL. Uma forma simples de resolver isso é usando a classe `PreparedStatement` do JDBC:

{% highlight java %}
String query = "SELECT * FROM Usuario WHERE login = ? AND senha = ?";
PreparedStatement pstm = con.prepareStatement(query);
// O PreparedStatement irá marcar com escapes os caracteres que são sensíveis no SQL.
pstm.setString(1, var1);
pstm.setString(2, var2);
ResultSet rs = stm.executeQuery(query);
{% endhighlight %}

Note como usamos o `?` no lugar das variáveis e depois usamos o método apropriado como `pstm.setString(1, var1)` para definir os valores que serão informados. Essa abordagem é muito melhor que a anterior, porque o `PreparedStatemet` vai adicionar escape nos caracteres que são sensíveis nas instruções SQL.

Se a aplicação usa o JPA, evite criar query nativa, como essa:

{% highlight java %}
String query = "SELECT * FROM Usuario WHERE login = '" + var1 + "' AND senha = '" + var2 + "'";
Query q = em.createNativeQuery(query);
List<Usuario> usuarios = (List<Usuario>) q.getResultList();
{% endhighlight %}

Porque voltamos ao problema original de concatenar dados com instruções SQL. Prefira usar `NamedQuery` ou `Criteria`, nesse [post](http://www.universidadejava.com.br/javaee/jpa-query) explico como usar queries no JPA.

Na `NamedQuery` definimos uma consulta usando o JPAQL que é muito similar ao SQL, mas na passagem parâmetros não precisamos concatenar diretamente os valores informados pelo usuário:

{% highlight java %}
@NamedQuery(name="autenticar", query="SELECT * FROM Usuario WHERE login = :login AND senha = :senha";
{% endhighlight %}

E quando usamos essa `NamedQuery` passamos os parâmetros e o JPA vai adicionar escape nos caracteres que são sensíveis nas instruções SQL. 

{% highlight java %}
Query q = em.createNamedQuery("autenticar");
q.setParameter("login", var1);
q.setParameter("senha", var2);
List<Usuario> usuarios = (List<Usuario>) q.getResultList();
{% endhighlight %}


## Conteúdos relacionados

- [Introdução a criptografia](http://www.universidadejava.com.br/outros/introducao-criptografia/)
- [Criptografia simétrica utilizando chave privada](http://www.universidadejava.com.br/outros/criptografia-simetrica/)
- [Criptografia assimétrica utilizando um par de chaves pública/privada](http://www.universidadejava.com.br/outros/criptografia-assimetrica/)