---
layout: article
title: "Java - varargs"
categories: java
author: sakurai
date: 2020-05-09 23:05:00
tags: [java]
published: true
excerpt: varargs.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## varargs

Quando temos um método que não sabemos ao certo quantos parâmetros serão passados, podemos passar um vetor ou lista de atributos primitivos ou objetos, mas em Java também podemos definir na assinatura do método, que ele recebe um parâmetro do tipo **varargs**.

O método pode receber quantos parâmetros forem necessários, porém só pode ter apenas um **varargs** na assinatura do método, sua sintaxe é assim:

{% highlight java %}
<< Tipo ... Identificador >>
{% endhighlight %}

Exemplo:

{% highlight java %}
/**
 * Classe utilizada demostrar a chamada de um método
 * que recebe parâmetros variados.
 */
public class VarargsTeste {
    public static void main(String[] args) {
        //Você passa quantos parâmetros quiser.
        int soma = somarValores(10, 5, 15, 20);
        System.out.println(soma);
        
        //Você pode chamar o método sem passar parâmetro.
        int soma2 = somarValores();
        System.out.println(soma2);
    }
    
    /**
     * Método que soma valores inteiros não importando
     * a quantidade de parâmetros.
     * 
     * @param valores - Parâmetros variados.
     * @return Soma total dos parâmetros.
     */
    public static int somarValores(int... valores) {
        int soma = 0;
        
        for(int valor : valores) {
            soma += valor;
        }
        
        return soma;
    }
}
{% endhighlight %}

Neste exemplo, temos o seguinte método **somarValores** que recebe um **varargs** de inteiros, quando chamamos este método podemos passar quantos atributos do tipo inteiro for necessário, e se quiser também não precisa passar nenhum atributo para ele.

O **varargs** é um facilitador, ao invés de criar um **array** ou **lista** e colocar os valores dentro dele para depois chamar o método, o mesmo pode ser chamado diretamente passando os **n** valores e os parâmetros enviados são automaticamente adicionados em um **array** do mesmo tipo do **varargs**.

Também podemos usar o **vargars** em um método que recebe outros parâmetros, mas quando tem mais parâmetros o **varargs** precisa ser o ultimo parâmetro recebido pelo método, por exemplo:

{% highlight java %}
/**
 * Classe utilizada demostrar a chamada de um método
 * que recebe parâmetros variados.
 */
public class VarargsTeste2 {
    public static void main(String[] args) {
        /* Aqui estamos chamando o método somarValores, passando o
         * primeiro valor (2) para o atributo multiplicador, e os
         * valores (10, 5, 15, 20) vão para o varargs valores. */
        int soma = somarValores(2, 10, 5, 15, 20);
        System.out.println(soma);
    }
    
    /**
     * Método que soma valores inteiros não importando
     * a quantidade de parâmetros.
     * @param valores - Parâmetros variados.
     * @return Soma total dos parâmetros.
     */
    public static int somarValores(int multiplicador, int... valores) {
        int soma = 0;
        for(int valor : valores) {
            soma += valor;
        }
        return soma * multiplicador;
    }
}
{% endhighlight %}

Neste exemplo, o método **somarValores** recebe dois parâmetros, um inteiro e um varargs do tipo inteiro, então quando chamamos este método passamos o atributo **multiplicador** recebe o valor **2** e o atributo **valores** recebe os valores **10, 5, 15 e 20**.

Lembrando que o varargs precisa ser o ultimo parâmetro do método e não pode ter mais que um varargs, caso contrario ocorrerá erro de compilação, por exemplo:

{% highlight java %}
/**
 * Classe utilizada demostrar formas erradas do uso de varargs.
 */
public class VarargsTeste3 {
    public int somarValores(float... numeros, int... valores) { }
    
    public void executarOperacao(double... precos, String operacao) { }
}
{% endhighlight %}

Portanto, estes dois métodos ficam com erro de compilação.

O método que tem na assinatura um parâmetro do tipo varargs só é chamado quando nenhuma outra assinatura de método corresponder com a invocação, por exemplo:

{% highlight java %}
/**
 * Classe utilizada demostrar a chamada de um método
 * que recebe parâmetros variados.
 */
public class VarargsTeste4 {
    public static void main(String[] args) {
        //Vai chamar o método que recebe um parâmetro inteiro.
        int valor = somarValores(2);
        System.out.println(valor);
        
        //Vai chamar o método que recebe um varargs.
        int valor2 = somarValores(2, 5);
        System.out.println(valor2);
    }
    
    public static int somarValores(int valores) {
        return valores;
    }

    public static int somarValores(int... valores) {
        int soma = 0;
        
        for(int valor : valores) {
            soma += valor;
        }
        
        return soma;
    }
}
{% endhighlight %}

Neste exemplo, na primeira chamada do método somarValores passando apenas um atributo inteiro, é chamado o método **somarValores(int numero)**. Na segunda chamada são passados vários parâmetros do tipo inteiro, neste caso o método chamado será o **somarValores(int... numeros)**.