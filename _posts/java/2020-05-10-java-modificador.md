---
layout: article
title: "Java - Modificadores de visibilidade"
categories: java
author: sakurai
date: 2020-05-10 00:14:00
tags: [java]
published: true
excerpt: Visibilidade.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Visibilidade

Em Java, utilizamos alguns modificadores que nos permitem “proteger” o acesso a um atributo, método ou até mesmo uma classe, de outras classes. Basicamente Java possui quatro modificadores básicos de acesso, sendo eles:

### Modificador de acesso: private

**private** é um modificador de acesso que restringe totalmente o acesso aquele recurso da classe de todas as demais classes, sejam elas do mesmo pacote, de outros pacotes ou até subclasses. Este modificador pode ser aplicado a atributos e métodos.

Exemplo:

{% highlight java %}
package br.visibilidade;

/**
 * Classe utilizada para representar um email que será enviado.
 */
public class Email {
  private String remetente;
  private String destinatario;
  private String assunto;
  private String mensagem;

  public void enviarEmail(String remetente, String destinatario,
      String assunto, String mensagem) {
    this.remetente = remetente;
    this.destinatario = destinatario;
    this.assunto = assunto;
    this.mensagem = mensagem;

    if(this.validarEmail()) {
      this.enviar();
    }
  }

  private boolean validarEmail() {
    boolean ok = false;
    // Código para validar se as informações do email estão corretas.
    return ok;
  }

  private void enviar() {
    // Envia o email.
  }
}
{% endhighlight %}

Neste exemplo, temos a classe **Email**, a partir de outras classes podemos chamar apenas o método **enviarEmail(...)**, os outros métodos e atributos da classe não podem ser usados por outras classes pois tem o modificador **private**.

{% highlight java %}
package br.visibilidade;

/**
 * Classe utilizada para testar o envio de email.
 */
public class TesteEmail {
  public static void main(String[] args) {
    Email email = new Email();
    email.enviarEmail("rafael@sakurai.com.br",
        "cristiano@almeida.com.br",
        "Aula de POO - Visibilidade",
        "Material da aula de visibilidade já está disponivel.");

    email.remetente = "teste@almeida.com.br"; //ERRO de Compilação.
    email.enviar(); //ERRO de Compilação.
  }
}
{% endhighlight %}

Neste exemplo conseguimos criar um objeto a partir da classe **Email** e enviar o email utilizando o método **enviarEmail(...)**, mas não conseguimos acessar o atributo **remetente** e o método **enviar()**, pois são privados e o compilador gera dois erros de compilação:

{% highlight java %}
C:\>javac br/visibilidade/TesteEmail.java
br\visibilidade\TesteEmail.java:14: remetente has private access in br.visibilidade.Email
    email.remetente = "teste@almeida.com.br"; // ERRO de Compilação.
         ˆ
br\visibilidade\TesteEmail.java:15: enviar() has private access in br.visibilidade.Email
    email.enviar(); //ERRO de Compilação.
         ^
2 errors
{% endhighlight %}

### Modificador de acesso: default (ou package)

**default** é um modificador um pouco menos restritivo que o private. Este tipo de modificador é aplicado a todas as classes, atributos ou métodos, os quais, não tiveram o seu modificador explicitamente declarado.

Este modificador permite que apenas classes do mesmo pacote tenham acesso as propriedades que possuem este modificador.

Neste exemplo, temos uma classe **Pessoa** que não declarou o modificador, portanto o modificador é o **default**.

{% highlight java %}
package br.visibilidade;

/**
 * Classe utilizada para representar uma Pessoa.
 */
class Pessoa {
  String nome;
}
{% endhighlight %}

Se criarmos outra classe dentro do mesmo pacote **br.visibilidade** da classe **Pessoa**, então está classe tem acesso à classe Pessoa também.

{% highlight java %}
package br.visibilidade;

/**
 * Classe utilizada para testar a classe Pessoa.
 */
public class TestePessoaPacote {
  public static void main(String[] args) {
    Pessoa pessoa = new Pessoa();
    pessoa.nome = "Rafael Guimarães Sakurai"; //OK
  }
}
{% endhighlight %}

Agora se tentarmos utilizar a classe **Pessoa**, a partir de outro pacote, terá erro de compilação, pois não tem acesso.

{% highlight java %}
package br.visibilidade.outropacote;

/**
 * Classe utilizada para testar a classe Pessoa.
 */
public class TestePessoaPacote2 {
  public static void main(String[] args) {
    Pessoa pessoa = new Pessoa(); //Não encontra a classe Pessoa.
    pessoa.nome = "Cristiano Camilo dos Santos de Almeida";
  }
}
{% endhighlight %}

Quando tentamos compilar essa classe, temos a seguinte mensagem do compilador:

{% highlight java %}
C:\>javac br/visibilidade/outropacote/TestePessoaPacote2.java
br\visibilidade\outropacote\TestePessoaPacote2.java:08: cannot find symbol
symbol: class Pessoa
location: class br.visibilidade.outropacote.TestePessoaPacote2
    Pessoa pessoa = new Pessoa();
    ˆ
br\visibilidade\outropacote\TestePessoaPacote2.java:08: cannot find symbol
symbol: class Pessoa
location: class br.visibilidade.outropacote.TestePessoaPacote2
    Pessoa pessoa = new Pessoa();
                        ˆ
{% endhighlight %}

### Modificador de acesso: protected

**protected** é um modificador um pouco menos restritivo que o **default**. Com este tipo modificador, podemos declarar que um atributo ou método é visível apenas para as classes do mesmo pacote ou para as subclasses daquela classe.

Neste exemplo, temos a classe **Produto**, e está classe tem o atributo **nomeProduto** e os métodos **getNomeProduto()** e **setNomeProduto()** que possuem o modificador **protected**.

{% highlight java %}
package br.visibilidade;

/**
 * Classe utilizada para representar um Produto.
 */
public class Produto {
  protected String nomeProduto;

  protected void setNomeProduto(String nomeProduto) {
    this.nomeProduto = nomeProduto;
  }

  protected String getNomeProduto() {
    return nomeProduto;
  }
}}
{% endhighlight %}

Se criarmos outra classe no pacote **br.visibilidade**, podemos utilizar os atributos e métodos **protected** declarados na classe **Produto**.

{% highlight java %}
package br.visibilidade;

/**
 * Classe utilizada para testar a visibilidade protected
 * do atributo e métodos da classe Produto.
 */
public class TesteProdutoPacote {
  public static void main(String[] args) {
    Produto prodLapis = new Produto();
    prodLapis.nomeProduto = "Lapis";
    System.out.println(prodLapis.getNomeProduto());

    Produto prodCaneta = new Produto();
    prodCaneta.setNomeProduto("Caneta");
    System.out.println(prodCaneta.getNomeProduto());
  }
}
{% endhighlight %}

Se criarmos uma classe em um pacote diferente, não conseguiremos utilizar os atributos e métodos da classe **Produto**, pois não temos acesso ao método. 

{% highlight java %}
package br.visibilidade.outropacote;

import br.visibilidade.Produto;

/**
 * Classe utilizada para testar a visibilidade protected
 * do atributo e métodos da classe Produto a partir de outro
 * pacote.
 */
public class TesteProdutoOutroPacote {
  public static void main(String[] args) {
    Produto prodCaneta = new Produto();
    prodCaneta.setNomeProduto("Caneta");
    System.out.println(prodCaneta.getNomeProduto());
  }
}
{% endhighlight %}

Se a classe que está em outro pacote for uma subclasse da classe Produto, então está classe recebe através da herança os atributos e métodos **protected**. (Veremos o conceito de herança mais adiante.)

{% highlight java %}
package br.visibilidade.outropacote;

import br.visibilidade.Produto;

/**
 * Classe utilizada para representar um Produto.
 */
public class SubProdutoOutroPacote extends Produto {
  public String getProduto() {
    return "Produto: " + getNomeProduto();
  }
}
{% endhighlight %}

### Modificador de acesso: public

**public** é o modificador menos restritivo de todos os demais. Declarando classes, atributos ou métodos com este modificador, automaticamente dizemos que aquela propriedade é acessível a partir de qualquer outra classe.

Obs: Dentro de um arquivo **.java**, só pode existir uma classe do tipo **public**, e está classe precisa obrigatoriamente ter o mesmo nome que o nome do arquivo **.java**.

Além dos modificadores de acesso, temos vários outros modificadores que serão abordados futuramente. A seguir, temos uma relação de todos os modificadores e os seus respectivos impactos quando aplicados a Classes, Métodos e Atributos.

| Modificador | Em uma classe  | Em um método                    | Em um atributo                  |
| ----------- | -------------- | ------------------------------- | ------------------------------- |
| private     | Não aplicável  | Acesso pela classe              | Acesso pela classe              |
| protected   | Não aplicável  | Acesso pelo pacote e subclasses | Acesso pelo pacote e subclasses |
| default     | Somente pacote | Acesso pelo pacote              | Acesso pelo pacote              |
| public      | Acesso total   | Acesso total                    | Acesso total                    |
| abstract    | Não instância  | Deve ser sobrescrito            | Não aplicável                   |
| final       | Sem herança    | Não pode ser sobrescrito        | Constante                       |
| static      | Não aplicável  | Acesso pela classe              | Acesso pela classe              |
| native      | Não aplicável  | Indica código nativo            | Não aplicável                   |
| transient   | Não aplicável  | Não aplicável                   | Não serializavel                |
| synchonized | Não aplicável  | Sem acesso simultâneo           | Não aplicável                   |