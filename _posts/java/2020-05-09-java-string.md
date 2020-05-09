---
layout: article
title: "Java - java.lang.String"
categories: java
author: sakurai
date: 2020-05-09 00:06:00
tags: [java]
published: true
excerpt: Usando a classe java.lang.String.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## A classe java.lang.String

A classe **java.lang.String** é utilizada para representar textos (sequência de caracteres). O tamanho que uma **String** suporta é igual ao tamanho disponível de memória.

Para criar uma **String** podemos utilizar qualquer uma das seguintes formas:

{% highlight java %}
String nome = new String("Rafael");

// ou

String sobrenome = "Sakurai";
{% endhighlight %}

Para fazer a concatenação de Strings podemos usar o sinal **+** ou usar o método **concat** da classe String, por exemplo:

{% highlight java %}
String nomeCompleto = nome + " " + sobrenome;

// ou

String nomeCompleto2 = "Cristiano".concat(" Camilo");
{% endhighlight %}

O valor de uma String é imutável, não se pode alterar seu valor. Quando alteramos o valor de uma String, estamos criando uma nova String.

Então, quando fazemos:

{% highlight java %}
String novoNome = "Cris" + "tiano”;
{% endhighlight %}

Estamos criando 3 Strings:
{% highlight java %}
	“Cris”
	“tiano”
	“Cristiano”
{% endhighlight %}

Alguns caracteres não podem ser simplesmente colocados dentro de uma String, como, as aspas duplas **(“)**.Este símbolo é usado para indicar o início e o fim de uma String. Por este motivo, caso tenhamos:

{% highlight java %}
String aspas = """; // Erro de compilação
{% endhighlight %}

Teremos um erro de compilação, pois estamos deixando uma aspas duplas “ fora do texto. Para os caracteres que não podem ser simplesmente adicionados dentro da String, usamos a barra invertida \ como escape de caracteres. Segue abaixo uma tabela com alguns escapes de caracteres e o que eles representam dentro da String.

| \t | Tabulação horizontal |
| \n | Nova linha           |
| \” | Aspas duplas         |
| \’ | Aspas simples        |
| \\ | Barra invertida      |

A classe **java.lang.String** tem alguns métodos para se trabalhar com os textos, por exemplo:

- **compareTo** – Compara se duas Strings são iguais. Ele retorna um número inteiro sendo 0 apenas caso ambas as String sejam idênticas.
- **compareToIgnoreCase** – Compara se duas Strings são iguais sem diferenciar letras maiúsculas e minúsculas.
- **equals** – Verifica se uma String é igual a outra. Retorna um tipo booleano.
- **replace** – Altera um caractere de uma String por outro caractere.
- **replaceAll** – Altera cada substring dentro da String com uma nova substring.
- **split** – Divide uma String em varias substrings a partir de uma dada expressão regular.

Mais métodos da classe **String** podem ser encontrados em: [http://java.sun.com/javase/6/docs/api/java/lang/String.html](http://java.sun.com/javase/6/docs/api/java/lang/String.html).

Cuidado ao utilizar os métodos da **String**, quando não houver uma instancia da classe **String**, no caso **null**. Invocar um método de uma referência nula gera uma exceção **NullPointerException**, um erro que ocorre em tempo de execução, por exemplo:

{% highlight java %}
String abc = null;
abc.equals("xpto");
{% endhighlight %}

Este código lançará uma **NullPointerException**, pois a **String abc** está vazia (**null**), e está tentando invocar o método **equals()** de um **String null** (exceções serão abordadas).


### Conversão para String (texto)

Para transformar qualquer tipo primitivo para **String**, basta utilizar o método:

{% highlight java %}
String.valueOf( <<informações que se tornará texto>> );
{% endhighlight %}

Para utilizá-lo, você deve atribuir a saída do método a uma **String**, e preencher o campo interno do método com o valor desejado, por exemplo:

Conversão de inteiro para String:

{% highlight java %}
int numero = 4;
String texto = String.valueOf(numero);
{% endhighlight %}

Observe que na segunda linha, a variável do tipo inteira está sendo utilizada no método valueOf() para que a conversão possa ser executada, e o resultado da conversão está sendo atribuída a **String** chamada texto.

O método **valueOf()** pode converter os seguintes tipos para **String**:

{% highlight java %}
int para String
String texto = String.valueOf(1);

long para String
String.valueOf(580l);

float para String
	String.valueOf(586.5f);

double para String
	String.valueOf(450.25);

boolean para String
	String.valueOf(true);

char para String
String.valueOf('a');
{% endhighlight %}

Note que os tipos **byte** e **short** não constam nesta lista, logo, para realizar a sua conversão é necessário transformar estes tipos primeiramente em **int** e só então convertê-los para **String**.


### Conversão de String para tipos primitivos

Assim como tratamos da manipulação de tipos de valores primitivos para **String**, nesta seção falaremos sobre a conversão de tipos primitivos para **String**.

Em Java é possível transformar qualquer **String** em um tipo primitivo, para tal, é necessário utilizar um dos conversores específicos de cada tipo, são eles:

{% highlight java %}
String texto = "15";

String para byte
byte o = Byte.parseByte(texto);

String para short
short a = Short.parseShort(texto);

String para int
int b = Integer.parseInt(texto);

String para long
long c = Long.parseLong(texto);

String para float
float d = Float.parseFloat(texto);

String para double
double e = Double.parseDouble(texto);

String para boolean
boolean  g = Boolean.parseBoolean("true");

String para char
char f = texto.charAt(1); //Pega o primeiro caractere da String.
{% endhighlight %}

### Recebendo Strings como parâmetro diretamente no método main

Quando executamos uma classe que possui o método **public static void main(String[] args)**, podemos passar parâmetros para este método.

Para passar os parâmetros para o método **main** via console, utilizamos a seguinte sintaxe:

{% highlight java %}
java nomeDaClasse parametro1 parametro2
{% endhighlight %}

No exemplo a seguir, criamos um programa que recebe alguns nomes como parâmetro:

{% highlight java %}
/**
 * Classe utilizada para mostrar a passagem de parâmetros
 * para o método main.
 */
public class ParametrosMain {
    public static void main(String[] args) {
        if(args.length > 0) {
            for(String nome : args) {
                System.out.println(nome);
            }
        } else {
            System.out.println("Nenhum nome foi passado como parametro.");
        }
    }
}
{% endhighlight %}

Se executarmos esta classe sem passar parâmetros, será impresso a seguinte mensagem:

{% highlight java %}
C:\>javac ParametrosMain.java
C:\>java ParametrosMain
Nenhum nome foi passado como parametro.
{% endhighlight %}

Quando executamos a aplicação passando os nomes como parâmetro, estes nomes são impressos no console. Note que quando precisamos passar um parâmetro que possua espaços em brancos, devemos utilizar as aspas duplas para delimitar a área do parâmetro, por exemplo **Cristiano** e **"Rafael Sakurai"**.

{% highlight java %}
C:\>javac ParametrosMain.java
C:\>java ParametrosMain Cristiano "Rafael Sakurai"
Cristiano
Rafael Sakurai
{% endhighlight %}