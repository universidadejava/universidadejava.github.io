---
layout: article
title: "Assinatura de métodos"
categories: java
author: sakurai
date: 2020-05-09 23:05:00
tags: [java, método, assinatura]
published: true
excerpt: Assinatura de métodos.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Assinatura de Métodos

Formalizando os conceitos de nome de método e de métodos com parâmetros, vamos agora então tratar de assinatura de método.

A assinatura de um método é composta por:
- **visibilidade** – tipo de permissão ao método.
- **retorno** – tipo de retorno do método ou **void** caso não tenha retorno.
- **parâmetros** – atributos utilizados dentro do processamento do método.
- **exceções** – informação de algum erro que pode ocorrer na execução do método.

No exemplo a seguir, temos alguns exemplos de métodos que podemos declarar:

{% highlight java %}
/**
 * Classe utilizada para demonstrar a utilização de diferentes assinaturas
 * de métodos.
 */
public class AssinaturaMetodo {
    public void imprimir() {
        System.out.println("Ola!!!");
    }
    
    public long calcular(int x, int y) {
        return x * y;
    }
    
    public String buscarDados(int matricula) {
        return "abcdef";
    }
    
    public Pessoa consultarPessoa(String nome) {
        return new Pessoa();
    }
    
    public void enviarEmail(Email e) {
    }
}
{% endhighlight %}

Para o compilador os métodos de mesmo nome, porém com parâmetros diferentes são identificados como métodos distintos entre si.

Exemplo:

Criamos a classe **Televisao**, contendo os seguintes métodos:

{% highlight java %}
/**
 * Classe utilizada para demonstrar vários métodos
 * com o mesmo nome, mas com parâmetros diferentes.
 */
public class Televisao {
    public void alterarCanal() {}
    public void alterarCanal(int numeroCanal) {}
    public void alterarCanal(int numeroCanal, int modeloAparelhoTv) {}
}
{% endhighlight %}

O compilador interpretaria estes três métodos de forma distinta, e em uma determinada chamada, saberia exatamente qual deles utilizar.

{% highlight java %}
/**
 * Classe utilizada para demonstrar a chamada de um método,
 * em uma classe com diversos método com mesmo nome.
 */
public class TelevisaoTeste {
    public static void main(String[] args) {
        Televisao tv = new Televisao();
        tv.alterarCanal(10);
    }
}
{% endhighlight %}

Note que na chamada ao método **alterarCanal()**, esta sendo invocado com passagem de um parâmetro inteiro, logo o método da classe **Televisao** a ser executado seria:

{% highlight java %}
public void alterarCanal(int numeroCanal) { }
{% endhighlight %}

O que identifica a assinatura do método é a união das informações de nome do método e parâmetros, logo os seguintes métodos listados abaixo seriam considerados erros de compilação:

{% highlight java %}
/**
 * Classe utilizada para demonstrar vários métodos com o mesmo nome e 
 * mesmos parâmetros, mas com retorno diferente.
 */
public class Contas {
    public int fazerConta(int num1, int num2) { }
    public float fazerConta(int num1, int num2) { } //Erro de compilação.
}
{% endhighlight %}

Visto que seus nomes e argumentos são idênticos, mudando apenas o tipo de retorno.

Ao conceito de se criar vários métodos de mesmo nome, porém com parâmetros diferentes entre si, damos o nome de **Sobrecarga de Método**.


### Troca de Mensagens

Na linguagem Java, é possível que objetos distintos se relacionem entre si por meio de seus métodos. A este tipo de operação damos o nome de **Troca de Mensagens**. Este tipo de relacionamento pode ser representado pela troca de informações, chamada de execução de métodos, etc.

Para que seja possível aos objetos se conversarem, necessariamente não é preciso que um objeto esteja contido dentro de outro, formando uma cadeia, mas basta simplesmente que um referencie os métodos do outro.

A fim de exemplificar melhor o que estamos falando, vamos imaginar uma mesa de bilhar. Em um ambiente deste tipo, para que a bola possa se mover é necessário que o taco a atinja diretamente. Abstraindo esta idéia, para nós usuários, o que nos compete é apenas o movimento do taco, sendo que a movimentação da bola ao longo da mesa deve ser realizada pela imposição do taco.

<figure>
    <a href="/images/2020-05-09-java-assinatura-metodo.png"><img src="/images/2020-05-09-java-assinatura-metodo.png" alt="Assinatura de métodos."></a>
</figure>

Agora imagine que o Usuário será representado pela classe **TacoTeste**, logo:

{% highlight java %}
/**
 * Classe utilizada para demonstrar a troca
 * de mensagens entre as classes.
 */
public class TacoTeste {
    public static void main(String[] args) {
        Taco tacoDeBilhar = new Taco();
        Bola bola8 = new Bola();
        
        tacoDeBilhar.bater(bola8);
    }
}
{% endhighlight %}

Imaginando a situação acima, teríamos a classe **TacoTeste** instanciando objetos de cada um dos dois tipos. Feito isso, uma chamada ao método **bater(Bola bola)** da classe **Taco**, indicando qual a bola alvo do taco, este método indica apenas que estamos movendo o taco para acertar uma bola.

Já na classe Taco, o método **bater(Bola bola)** é encarregado de executar a ação de bater na bola. Logo, teríamos algo deste gênero:

{% highlight java %}
/**
 * Classe utilizada para representar um taco de bilhar.
 */
class Taco {
    /**
     * Método utilizado para representar a ação do taco
     * bater na bola de bilhar.
     * 
     * @param bola - Bola que será alvo do taco.
     */
    public void bater(Bola bola) {
        bola.mover(5, 25.5F);
    }
}
{% endhighlight %}

Note então, que diferentemente das classes que desenvolvemos até então, a chamada do método **mover(int forca, float angulo)** da classe **Bola** está sendo referenciado de dentro da classe Taco, sem qualquer interferência do usuário através da classe **TacoTeste**.

{% highlight java %}
/**
 * Classe utilizada representar uma bola de bilhar.
 */
class Bola {
    /**
     * Método que será utilizado para fazer o deslocamento
     * da bola de bilhar.
     * 
     * @param forca - Intensidade da batida.
     * @param angulo - Angulo que a bola foi acertada.
     */
    public void mover(int forca, float angulo) {
        //Código para calcular a tragetória da bola.
    }
}
{% endhighlight %}

Este relacionamento entre as classes sem a intervenção externa, assemelhando-se a uma conversa entre elas, damos o nome de **troca de mensagens**.