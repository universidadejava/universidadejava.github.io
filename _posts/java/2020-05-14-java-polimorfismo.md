---
layout: article
title: "Java - Polimorfismo"
categories: java
author: sakurai
date: 2020-05-13 06:42:00
tags: [java]
published: true
excerpt: 
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Polimorfismo

Quando trabalhamos com herança e dizemos que uma subclasse **PessoaFisica** e **PessoaJuridica** são filhas da super classe **Pessoa**, podemos então dizer que um **PessoaFisica** e **PessoaJuridica É UMA Pessoa**, justamente por ser uma extensão ou tipo mais especificado deste. Essa é a semântica da herança.

<figure>
    <a href="/images/2020-05-14-java-polimorfismo.png"><img src="/images/2020-05-14-java-polimorfismo.png" alt="Exemplo de polimorfismo."></a>
</figure>

Dizer que uma **Pessoa É UMA PessoaFisica** está errado, porque ela pode também ser uma **PessoaJuridica**.

Quando trabalhamos com uma variável do tipo Pessoa que é uma super classe, podemos fazer está variável receber um objeto do tipo PessoaFisica ou PessoaJuridica, por exemplo:
	
{% highlight java %}
Pessoa fisica = new PessoaFisica();
Pessoa juridica = new PessoaJuridica();
{% endhighlight %}

Com isso, podemos dizer que **polimorfismo** é a capacidade de um objeto ser referenciado de diversas formas diferentes e com isso realizar as mesmas tarefas (ou chamadas de métodos) de diferentes formas.

Um exemplo do uso do polimorfismo utilizando a classe Pessoa, seria todas as subclasses sobrescreverem o método getNome().

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para representar uma Pessoa.
 */
public class Pessoa {
  private String nome;

  public String getNome() {
    return nome;
  }

  public void setNome(final String nome) {
    this.nome = nome;
  }
}
{% endhighlight %}

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para representar uma Pessoa Física
 * que É UMA subclasse de Pessoa.
 */
public class PessoaFisica extends Pessoa {
  private long cpf;

  public PessoaFisica() {
  }

  public long getCpf() {
    return cpf;
  }

  public void setCpf(long cpf) {
    this.cpf = cpf;
  }

  public String getNome() {
    return "Pessoa Fisica: " + super.getNome() + " - CPF: " + this.getCpf();
  }
}
{% endhighlight %}

A subclasse PessoFisica sobrescreve o método **getNome()** e retorna a seguinte frase: **“Pessoa Fisica: nomePessoa – CPF: cpfPessoa”**.

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para representar uma Pessoa Fisica
 * que É UMA subclasse de Pessoa.
 */
public class PessoaJuridica extends Pessoa {
  private long cnpj;

  public PessoaJuridica() {
  }

  public long getCnpj() {
    return cnpj;
  }

  public void setCnpj(long cnpj) {
    this.cnpj = cnpj;
  }

  public String getNome() {
    return "Pessoa Juridica: " + super.getNome() + " - CNPJ: " + this.getCnpj();
  }
}
{% endhighlight %}

A subclasse PessoaJuridica sobrescreve o método getNome() e retorna a seguinte frase: “Pessoa Juridica: nomePessoa – CNPJ: cnpjPessoa”.

Desta maneira, independentemente do nosso objeto PessoaFisica e PessoaJuridica ter sido atribuído a uma referencia para Pessoa, quando chamamos o método getNome() de ambas variáveis, temos a seguinte saída:

{% highlight java %}
Pessoa Fisica: Cristiano - CPF: 0
Pessoa Juridica: Rafael - CNPJ: 0
{% endhighlight %}

Exemplo:

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para demonstrar o uso do polimorfismo,
 * vamos criar um vetor de Pessoa e adicionar nele objetos
 * do tipo PessoaFisica e PessoaJuridica.
 */
public class TestePessoa {
  public static void main(String[] args) {
    Pessoa fisica = new PessoaFisica();
    fisica.setNome("Cristiano");
    fisica.setCpf(12345678901L);

    Pessoa juridica = new PessoaJuridica();
    juridica.setNome("Rafael");

    Pessoa[] pessoas = new Pessoa[2];
    pessoas[0] = fisica;
    pessoas[1] = juridica;

    for(Pessoa pessoa : pessoas) {
      System.out.println(pessoa.getNome());
    }
  }
}
{% endhighlight %}

Saída:

{% highlight java %}
C:\>javac material\polimorfismo\TestePessoa.java
C:\>java material.polimorfismo.TestePessoa
Pessoa Fisica: Cristiano - CPF: 0
Pessoa Juridica: Rafael - CNPJ: 0
{% endhighlight %}

Mesmo as variáveis sendo do tipo Pessoa, o método getNome() foi chamado da classe PessoaFisica e PessoaJuridica, porque durante a execução do programa, a JVM percebe que a variável fisica está guardando um objeto do tipo PessoaFisica, e a variável juridica está guardando um objeto do tipo PessoaJuridica.

Note que neste exemplo apenas atribuímos o valor do nome da Pessoa, não informamos qual o CPF ou CNPJ da pessoa, se tentarmos utilizar a variável do tipo Pessoa para atribuir o CPF através do método setCpf() teremos um erro de  compilação, pois somente a classe PessoaFisica possui este método:

{% highlight java %}
public static void main(String[] args) {
  Pessoa fisica = new PessoaFisica();
  fisica.setNome("Cristiano");
  fisica.setCpf(12345678901L);
{% endhighlight %}

Durante a compilação teremos o seguinte erro informando que a classe material.polimorfismo.Pessoa não possui o método setCpf(long):

{% highlight java %}
C:\>javac material\polimorfismo\TestePessoa.java
material\polimorfismo\TestePessoa.java:12: cannot find symbol
symbol  : method setCpf(long)
location: class material.polimorfismo.Pessoa
	fisica.setCpf(12345678901L);
           ^
1 error
{% endhighlight %}

Atenção: mesmo o objeto sendo do tipo PessoaFisica, quando chamamos um método através da classe Pessoa, só podemos chamar os métodos que existem na classe Pessoa.

Durante a execução do programa, a JVM verifica qual a classe de origem do objeto e chama o método desta classe.

Para podermos atribuir o valor para CPF ou CNPJ, é preciso ter variáveis do tipo PessoaFisica e PessoaJuridica.

No exemplo a seguir as variáveis são do tipo PessoaFisica e PessoaJuridica, elas serão guardadas dentro de um vetor de Pessoa:

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para demonstrar o uso do polimorfismo,
 * vamos criar um vetor de Pessoa e adicionar nele objetos
 * do tipo PessoaFisica e PessoaJuridica.
 */
public class TestePessoa2 {
  public static void main(String[] args) {
    PessoaFisica fisica = new PessoaFisica();
    fisica.setNome("Cristiano");
    fisica.setCpf(12345678901L);

    PessoaJuridica juridica = new PessoaJuridica();
    juridica.setNome("Rafael");
    juridica.setCnpj(1000100012345678L);

    Pessoa[] pessoas = new Pessoa[2];
    pessoas[0] = fisica;
    pessoas[1] = juridica;

    for(Pessoa pessoa : pessoas) {
      System.out.println(pessoa.getNome());
    }
  }
}
{% endhighlight %}

Agora quando executarmos este programa tem a seguinte saída:

{% highlight java %}
C:\>javac material\polimorfismo\TestePessoa2.java
C:\>java material.polimorfismo.TestePessoa2
Pessoa Fisica: Cristiano - CPF: 12345678901
Pessoa Juridica: Rafael - CNPJ: 1000100012345678
{% endhighlight %}

Se criarmos variáveis do tipo PessoaFisica ou PessoaJuridica, atribuirmos os valores para nome e cpf ou cnpj, depois disso podemos fazer variáveis do tipo Pessoa terem referencia para o mesmo objeto que as variáveis do tipo PessoaFisica e PessoaJuridica, por exemplo:

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para demonstrar o uso do polimorfismo,
 * vamos criar duas variaveis do tipo Pessoa e adicionar nele objetos
 * do tipo PessoaFisica e PessoaJuridica.
 */
public class TestePessoa3 {
  public static void main(String[] args) {
    PessoaFisica fisica = new PessoaFisica();
    fisica.setNome("Cristiano");
    fisica.setCpf(12345678901L);

    PessoaJuridica juridica = new PessoaJuridica();
    juridica.setNome("Rafael");
    juridica.setCnpj(1000100012345678L);

    Pessoa pessoa1 = fisica;
    Pessoa pessoa2 = juridica;

    System.out.println("Pessoa 1");
    System.out.println(pessoa1.getNome());

    System.out.println("Pessoa 2");
    System.out.println(pessoa2.getNome());
  }
}
{% endhighlight %}

Na linha 18 foi criado uma variável pessoa1 do tipo Pessoa que recebe um objeto do tipo PessoaFisica.

Na linha 19 foi criado uma variável pessoa2 do tipo Pessoa que recebe um objeto do tipo PessoaJuridica.

Quando executarmos este programa tem a seguinte saída:

{% highlight java %}
C:\>javac material\polimorfismo\TestePessoa3.java
C:\>java material.polimorfismo.TestePessoa3
Pessoa 1
Pessoa Fisica: Cristiano - CPF: 12345678901
Pessoa 2
Pessoa Juridica: Rafael - CNPJ: 1000100012345678
{% endhighlight %}

Dessa vez o valor do cpf e cnpj são impressos, pois foram previamente preenchidos.

### Casting (conversão) de objetos

Vimos anteriormente que uma Pessoa (super classe) nem sempre É UMA PessoaFisica (subclasse), mas quando estamos trabalhando com uma super classe e temos a certeza de qual o tipo de subclasse ele está representando podemos fazer o casting de objetos, para guardar o objeto em sua classe, funciona de forma similar ao casting de atributos primitivos.

No exemplo a seguir, vamos criar duas variáveis do tipo Pessoa com objetos do tipo PessoaFisica e PessoaJuridica, depois vamos também criar uma variável do tipo Object (que é a super classe de todas as classes) e guardar nela um objeto do tipo String.

{% highlight java %}
package material.polimorfismo;

/**
 * Classe utilizada para demonstrar o uso do polimorfismo,
 * vamos criar duas variaveis do tipo Pessoa e adicionar nele objetos
 * do tipo PessoaFisica e PessoaJuridica.
 */
public class TestePessoa4 {
  public static void main(String[] args) {
    Pessoa pessoa1 = new PessoaFisica();
    pessoa1.setNome("Cristiano");

    Pessoa pessoa2 = new PessoaJuridica();
    pessoa2.setNome("Rafael");

    Object objeto = "Programacao Orientada a Objetos";

    PessoaFisica fisica = (PessoaFisica) pessoa1;
    fisica.setCpf(12345678901L);

    PessoaJuridica juridica = (PessoaJuridica) pessoa2;
    juridica.setCnpj(1000100012345678L);

    String texto = (String) objeto;

    System.out.println(fisica.getNome());
    System.out.println(juridica.getNome());
    System.out.println(texto);
  }
}
{% endhighlight %}

Na linha 18 estamos fazendo um casting de Pessoa para PessoaFisica. Na linha 21 estamos fazendo um casting de Pessoa para PessoaJuridica. Na linha 24 estamos fazendo um casting de Object para String.

Temos a seguinte saída no console:

{% highlight java %}
C:\>javac material\polimorfismo\TestePessoa4.java
C:\>java material.polimorfismo.TestePessoa4
Pessoa Fisica: Cristiano - CPF: 12345678901
Pessoa Juridica: Rafael - CNPJ: 1000100012345678
Programacao Orientada a Objetos
{% endhighlight %}