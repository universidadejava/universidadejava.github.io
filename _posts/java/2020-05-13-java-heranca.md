---
layout: article
title: "Herança (extends)"
categories: java
author: sakurai
date: 2020-05-13 00:30:00
tags: [java, extends, herança]
published: true
excerpt: Em Java, podemos criar classes que herdem atributos e métodos de outras classes, evitando rescrita de código.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Herança

Em Java, podemos criar classes que herdem atributos e métodos de outras classes, evitando rescrita de código. Este tipo de relacionamento é chamado de **Herança**.

Para representarmos este tipo de relacionamento na linguagem, devemos utilizar a palavra reservada **extends**, de forma a apontar para qual classe a nossa nova classe deve herdar seus atributos e métodos.

No vídeo a seguir mostramos passo a passo como funciona a herança:

<iframe width="420" height="315" src="https://www.youtube.com/embed/qcR5bnUSJS4" frameborder="0" allowfullscreen></iframe>
	
Vamos praticar mais um pouco, neste outro exemplo demonstraremos a vantagem do reaproveitamento de código utilizando a Herança. Temos as classes Funcionario e Coordenador que possuem o atributo nome e matricula em comum.

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para representar o Funcionario.
 */
public class Funcionario {
    private String nome;
    private int matricula;
    private String departamento;
    
    public Funcionario(String nome, int matricula, String departamento) {
        this.nome = nome;
        this.matricula = matricula;
        this.departamento = departamento;
    }

    public int getMatricula() { return matricula; }
    public void setMatricula(int matricula) { this.matricula = matricula; }

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }

    public String getDepartamento() { return departamento; }
    public void setDepartamento(String departamento) {
        this.departamento = departamento;
    }
}
{% endhighlight %}

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para representar o Coordenador.
 */
public class Coordenador {
    private String nome;
    private int matricula;
    private String cursoCoordenado;
    
    public Coordenador(String nome, int matricula, String cursoCoordenado) {
        this.nome = nome;
        this.matricula = matricula;
        this.cursoCoordenado = cursoCoordenado;
    }

    public int getMatricula() { return matricula; }
    public void setMatricula(int matricula) { this.matricula = matricula; }

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }

    public String getCursoCoordenado() { return cursoCoordenado; }
    public void setCursoCoordenado(String cursoCoordenado) {
        this.cursoCoordenado = cursoCoordenado;
    }
}
{% endhighlight %}

Como os atributos **nome** e **matricula**, são comuns para ambas as classes, e elas possuem algo a mais em comum que é seu propósito, ambas são utilizadas para representar Pessoas.

Podemos criar uma classe **Pessoa** que terá os atributos **nome** e **matricula**, e por meio da herança reaproveitaremos esses atributos nas classes Funcionario e Coordenador.

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para representar a Pessoa.
 */
public class Pessoa {
    private String nome;
    private int matricula;
    
    /**
     * Construtor que recebe o nome da pessoa.
     *
     * @param nome
     */
    public Pessoa(String nome, int matricula) {
        this.nome = nome;
        this.matricula = matricula;
    }

    public int getMatricula() { return matricula; }
    public void setMatricula(int matricula) { this.matricula = matricula; }

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }
}
{% endhighlight %}

Agora vamos alterar a classe Funcionario e Coordenador:

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para representar um Funcionario que é uma Pessoa.
 */
public class Funcionario extends Pessoa {
    private String departamento;
    
    public Funcionario(String nome, int matricula, String departamento) {
        super(nome, matricula);
        this.departamento = departamento;
    }

    public String getDepartamento() { return departamento; }
    public void setDepartamento(String departamento) {
        this.departamento = departamento;
    }
}
{% endhighlight %}

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para representar um Coordenador que é uma Pessoa.
 */
public class Coordenador extends Pessoa {
    private String cursoCoordenado;

    public Coordenador(String nome, int matricula, String cursoCoordenado) {
        super(nome, matricula);
        this.cursoCoordenado = cursoCoordenado;
    }

    public String getCursoCoordenado() { return cursoCoordenado; }
    public void setCursoCoordenado(String cursoCoordenado) {
        this.cursoCoordenado = cursoCoordenado;
    }
}
{% endhighlight %}

Com a declaração acima, temos as classes Funcionario e Coordenador como classes filha ou subclasses da classe pai Pessoa. Com isso podemos dizer que **as subclasses Funcionario e Coordenador herdam todos os atributos e métodos da sua super classe Pessoa**.

Por isso, lembre-se, o **Funcionario É UMA Pessoa**, pois é uma subclasse, logo, apenas possui algumas características a mais do que Pessoa, porém podemos sempre manuseá-lo como uma Pessoa, logo, também é possível se fazer o seguinte tipo de declaração:

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para testar a Herança da classe
 * Funcionario.
 */
public class TesteFuncionario {
    public static void main(String[] args) {
        /* Declarações comuns. */
        Pessoa camilo = new Pessoa("Camilo", 123);
        Funcionario rafael = new Funcionario("Rafael", 111, "informatica");
        
        /* Todo Funcionario é uma Pessoa. */
        Pessoa sakurai = new Funcionario("Sakurai", 222, "telecomunicação");
        
        /* Erro de compilação, porque nem toda
           Pessoa é um Funcionario. */
        Funcionario cristiano = new Pessoa("Cristiano", 456);
    }
}
{% endhighlight %}

Porém note que na linha 18, temos um erro de compilação, pois uma Pessoa nem sempre é um Funcionario, afinal de contas, poderíamos ter a seguinte situação:

<figure>
    <a href="/images/2020-05-13-java-heranca-01.png"><img src="/images/2020-05-13-java-heranca-01.png" alt="Estrutura de classes com herança modeladas com UML."></a>
</figure>

Neste exemplo, temos a super classe Pessoa, e três subclasses Funcionario, Aluno e Professor.

Uma classe pode herdar apenas de uma classe (super classe). Quando uma classe não define explicitamente que está herdando outra classe então, esta classe é filha de **java.lang.Object**, ou seja, a classe **Object** é a classe pai de todas as classes.

Por Object ser pai de todas as classes, todas as classes herdam os seguintes métodos dela:

<figure>
    <a href="/images/2020-05-13-java-heranca-02.png"><img src="/images/2020-05-13-java-heranca-02.png" alt="Métodos herdados da classe Object."></a>
</figure>

Quando lidamos com classes que possuem a relação de herança, podemos fazer uso de duas palavras-chave que servem para identificar se estamos utilizando um método e ou atributo da classe atual ou de sua super classe. Estes comandos são:
	
> this = Define que o recurso pertence à classe atual.

> super = Define que o recurso pertence à super classe.

Podemos visualizar que a classe Coordenador utiliza ambas as palavras-chaves:

{% highlight java %}
package material.heranca;

/**
 * Classe utilizada para representar um Coordenador que é uma Pessoa.
 */
public class Coordenador extends Pessoa {
    private String cursoCoordenado;

    public Coordenador(String nome, int matricula, String cursoCoordenado) {
        super(nome, matricula);
        this.cursoCoordenado = cursoCoordenado;
    }

    public String getCursoCoordenado() { return cursoCoordenado; }
    public void setCursoCoordenado(String cursoCoordenado) {
        this.cursoCoordenado = cursoCoordenado;
    }
}
{% endhighlight %}

O construtor que recebe o nome, matrícula e curso do coordenador. Note que neste construtor temos a chamada **super(nome, matricula)**, que irá chamar o construtor da classe Pessoa que recebe um String e um inteiro como parâmetro.

Dentro deste mesmo construtor temos a seguinte chamada **this.cursoCoordenado = cursoCoordenado**. Utilizando a palavra-chave **this**, referenciaremos o atributo cursoCoordenador da própria classe Coordenador.