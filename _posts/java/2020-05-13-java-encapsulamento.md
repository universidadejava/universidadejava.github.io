---
layout: article
title: "Encapsulamento"
categories: java
author: sakurai
date: 2020-05-12 23:38:00
tags: [java, encapsulamento]
published: true
excerpt: Encapsular, nada mais é do que proteger membros de outra classe de acesso externo, permitindo somente sua manipulação de forma indireta.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Encapsulamento

Encapsular, nada mais é do que proteger membros de outra classe de acesso externo, permitindo somente sua manipulação de forma indireta. Isso é possível da seguinte maneira:

Consideremos o seguinte exemplo de uma classe Livro:

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar o Livro.
 */
public class Livro {
    private String titulo;
    private String autor;

    public String getAutor() {
        return autor;
    }

    public void setAutor(String autor) {
        this.autor = autor;
    }

    public String getTitulo() {
        return titulo;
    }

    public void setTitulo(String titulo) {
        this.titulo = titulo;
    }
}
{% endhighlight %}

Note que os dois atributos da classe (titulo e autor) são do tipo **private**, que permite apenas o acesso destas informações a partir da própria classe, logo para que seja possível ler ou alterar essas informações criamos métodos ditos **métodos assessores** ou então **getters** e **setters**.

A princípio parece ser algo sem muita utilidade, mas desta forma podemos criar atributos os quais podemos apenas ler informações ou ainda gerar tratamentos específicos sempre que outra classe solicita a alteração de um atributo de nossa classe.

Encapsule aquilo que pode ser alterado com frequência na sua aplicação, por exemplo:

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar um Professor
 */
public class Professor {
    private int registro;
    private String nome;

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public int getRegistro() {
        return registro;
    }

    public void setRegistro(int registro) {
        this.registro = registro;
    }
}
{% endhighlight %}

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar um Aluno.
 */
public class Aluno {
    private int matricula;
    private String nome;

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public int getMatricula() {
        return matricula;
    }

    public void setMatricula(int matricula) {
        this.matricula = matricula;
    }
}
{% endhighlight %}

Note que os atributos das classes **Professor** e **Aluno** possuem a visibilidade **private**, dessa forma eles só podem ser acessados pelas suas classes, também criamos métodos **set** (métodos utilizados para alterar o valor de uma propriedade) e **get** (método utilizado para obter o valor de uma propriedade) para cada atributo.

Dado a classe Professor e Aluno, queremos consultar os alunos e professores pelo nome:

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para consultar os alunos e professores
 * através de seu nome.
 */
public class Consultar {
    public Aluno[] consultarAlunoPorNome(Aluno[] alunos, String nome) {
        Aluno[] alunosEncontrados = new Aluno[alunos.length];
        int contAlunos = 0;

        for(int i = 0; i < alunos.length; i++) {
            if(alunos[i].getNome().equals(nome)) {
                alunosEncontrados[contAlunos++] = alunos[i];
            }
        }

        Aluno[] retorno = new Aluno[contAlunos];
        for(int i = 0; i < contAlunos; i++) {
            retorno[i] = alunosEncontrados[i];
        }

        return retorno;
    }

    public Professor[] consultarProfessorPorNome(Professor[] professores,
String nome) {
        Professor[] profEncontrados = new Professor[professores.length];
        int contProfessores = 0;

        for(int i = 0; i < professores.length; i++) {
            if(professores[i].getNome().equals(nome)) {
                profEncontrados[contProfessores++] = professores[i];
            }
        }

        Professor[] retorno = new Professor[contProfessores];
        for(int i = 0; i < contProfessores; i++) {
            retorno[i] = profEncontrados[i];
        }

        return retorno;
    }
}
{% endhighlight %}

Agora digamos que nosso programa que busca os alunos e professores esta funcionando corretamente, e precisamos adicionar outra busca que procura coordenadores pelo seu nome.

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar um Coordenador.
 */
public class Coordenador {
    private String cursoCoordenado;
    private String nome;

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }

    public String getCursoCoordenado() {
        return cursoCoordenado;
    }

    public void setCursoCoordenado(String cursoCoordenado) {
        this.cursoCoordenado = cursoCoordenado;
    }
}
{% endhighlight %}

Para consultar o coordenador pelo nome, precisamos usar um método bem parecido com o que consulta o aluno e professor, mas se notarmos as classes **Aluno**, **Professor** e **Coordenador** tem o atributo **nome** em comum e é exatamente por este atributo que os consultamos.

Se criarmos uma classe **Pessoa** para ser uma classe Pai dessas classes e se a classe Pai tiver o atributo **nome**, então todas as subclasses recebem este atributo por herança:

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar um Pessoa.
 */
public class Pessoa {
    private String nome;

    public String getNome() {
        return nome;
    }

    public void setNome(String nome) {
        this.nome = nome;
    }
}
{% endhighlight %}

As subclasses Professor, Aluno e Coordenador ficaram da seguinte forma:

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar um Professor.
 */
public class Professor extends Pessoa {
    private int registro;

    public int getRegistro() {
        return registro;
    }

    public void setRegistro(int registro) {
        this.registro = registro;
    }
}
{% endhighlight %}

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar um Aluno.
 */
public class Aluno extends Pessoa {
    private int matricula;

    public int getMatricula() {
        return matricula;
    }

    public void setMatricula(int matricula) {
        this.matricula = matricula;
    }
}
{% endhighlight %}

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para representar o Coordenador.
 */
public class Coordenador extends Pessoa {
    private String cursoCoordenado;

    public String getCursoCoordenado() {
        return cursoCoordenado;
    }

    public void setCursoCoordenado(String cursoCoordenado) {
        this.cursoCoordenado = cursoCoordenado;
    }
}
{% endhighlight %}

Para consultar o Aluno, Professor e Coordenador vamos criar apenas um método que recebe um vetor de Pessoa e o nome que será procurado e retorna um vetor com as Pessoas encontradas:

{% highlight java %}
package br.encapsulamento;

/**
 * Classe utilizada para consultar de uma forma genérica qualquer
 * subclasse de Pessoa através de seu nome.
 */
public class ConsultarPessoa {
    public Pessoa[] consultarAlunoPorNome(Pessoa[] pessoas, String nome) {
        Pessoa[] pessoasEncontradas = new Pessoa[pessoas.length];
        int contPessoas = 0;

        for(int i = 0; i < pessoas.length; i++) {
            if(pessoas[i].getNome().equals(nome)) {
                pessoasEncontradas[contPessoas++] = pessoas[i];
            }
        }

        Pessoa[] retorno = new Pessoa[contPessoas];
        for(int i = 0; i < contPessoas; i++) {
            retorno[i] = pessoasEncontradas[i];
        }

        return retorno;
    }
}
{% endhighlight %}

Dessa forma usamos herança e encapsulamos a forma de encontrar um Aluno, Pessoa e Coordenador, agora se tivermos que adicionar um Diretor, por exemplo, nosso método de consulta não precisa ser alterado, precisamos apenas criar a classe Diretor e fazer ela filha da classe Pessoa.