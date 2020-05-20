---
layout: article
title: "TDD - Olá testes"
categories: tdd
author: sakurai
date: 2020-05-19 23:32:00
tags: [java, tdd, testes]
published: true
excerpt: Nesse post mostro como criar classes, métodos, solicitar alguma informação do usuário (através da linha de comando) e como criar e executar um teste.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Olá testes!

Nesse post mostro como criar classes, métodos, solicitar alguma informação do usuário (através da linha de comando) e como criar e executar um teste.

## Criando uma saudação

Para começar vamos criar uma aplicação que faz uma saudação para o usuário. A ideia é perguntar qual o nome do usuário e mostrar uma mensagem simples como `Oi Fulano!`.

Com esse exemplo vamos criar:

1. A classe que realiza a saudação;
2. A classe que usa a saudação;
3. A classe que testa se a saudação está correta.

Vamos criar uma **classe** (para quem não conhece classe é basicamente um arquivo que contém atributos (informações) e métodos (operações) que representam algo do mundo real) que sabe fazer uma saudação, para isso crie um arquivo chamado `Ola.java` com o seguinte conteúdo:

> Esse arquivo pode ser criado usando um bloco de notas ou programa similar, o importante é verificar se o arquivo foi salvo com a extensão **.java**.

> Mas em qual local salvar os arquivos? Sugestão: salve em algum lugar fácil como `C:\Testes`.

{% highlight java %}
public class Ola {
	public String saudacao(String nome) {
		return "Oi " + nome + "!";
	}
}
{% endhighlight %}

---

### Seção perguntas e respostas

Algumas informações importantes para quem está começando:

1. Porque o arquivo chama `Alo.java`?
Resposta: Por padrão todo arquivo Java que contém uma classe pública (`public class ...`) deve ter o nome do arquivo igual ao nome da classe, nesse caso como a classe pública chama `Ola` o arquivo chama `Ola.java`.

2. O que é esse `public String saudacao(String nome)`?
Resposta: Isso é a declaração de um método público, que pode ser acessado por outras classes e que se chama **saudacao**, esse método recebe como parâmetro uma `String` (que representa um texto) chamado `nome` e devolve outra `String`.

3. O que faz o `return`?
Resposta: É uma palavra reservada do Java que indica o conteúdo que será retornado no final da execução do método.

4. Porque usar as aspas duplas `"..."`?
Resposta: Quando escrevemos textos estamos utilizando um tipo de dado chamado String que representa uma sequência de caracteres, esses textos escritos no meio do código são representados pelo uso das aspas duplas `"Oi"`.

---

## Criando a interação com o usuário

Continuando vamos criar uma classe que faz a interação com o usuário, essa classe é responsável por perguntar o nome do usuário e apresentar uma mensagem de saúdação. Vamos criar a classe chamada `Principal.java` que usa a classe `Ola` para realizar a saudação:

{% highlight java %}
import java.util.Scanner;

public class Principal {
	public static void main(String[] args) {
		Scanner s = new Scanner(System.in);
		System.out.println("Qual seu nome?");
		String nome = s.nextLine();

		Ola ola = new Ola();
		String saudacao = ola.saudacao(nome);
		System.out.println(saudacao);
	}
}
{% endhighlight %}

---

### Seção perguntas e respostas

Algumas informações importantes para quem está começando:

1. O que essa classe faz? Resposta: Essa classe realizará a interação com o usuário, ela perguntará o nome do usuário e fará uma saudação.
2. Para que serve esse `import java.util.Scanner`? Resposta: Todas as classes fora do pacote `java.lang` precisam ser importadas para serem utilizadas, para isso utilizamos a palavra-chave `import`. A declaração do import é feito no começo antes da declaração da classe.
3. Porque o arquivo chama `Principal.java` e a classe chama `Principal`? Resposta: Essa eu já respondi na seção de perguntas e repostas anterior. Qual a sua resposta?
4. Para que serve o método `public static void main(String[] args)`? Resposta: É através desse método que o programa começará a ser executado.
5. O que é esse `Scanner s = new Scanner(System.in)`? Resposta: A classe `Scanner` é usada para tratar entrada de dados, nesse caso quero tratar as entradas de dados do `System.in` que é o prompt de comando (console ou terminal).
6. O que faz o `System.out.println(...)`? Resposta: Imprime um texto no prompt de comando.
7. O que é o `s.nextLine()`? Resposta: Isso é a chamada do método `nextLine()` que está na classe `Scanner`, esse método vai ler o texto da linha que foi digitada no prompt de comando. O resultado será guardado na variável `String nome`.
8. O que faz o código `Ola ola = new Ola();`? Resposta: Serve para criar uma instância da classe `Ola`, em outra palavras, criamos um modelo de objeto chamado `Ola`, nesse modelo especificamos como será tratado a saudação ao usuário, e nesse caso estamos criando um novo objeto a partir desse modelo.
9. O que faz a chamada `ola.saudacao(nome)`? Resposta: Estamos chamamos o método `saudacao` da classe `Ola` passando o nome que o usuário informou.

---

## Compilar e executar

O que é compilar?

O código Java que acabamos de escrever é feito de tal forma que podemos ler e entender o que foi escrito, mas não é isso o que o seu computador entende ou no nosso caso não é isso que a Maquina Virtual do Java (**Java Virtual Machine - JVM**) entende.

A compilação é o ato que gerar um arquivo que a JVM entenda a partir do nosso código Java.

---

Para compilar o programa, vamos usar o prompt de comando (command), veja alguns comandos usados:

* `dir` - mostra o conteúdo do diretório atual;
* `cd` - pode ser usado para entrar em algum diretório, exemplos: `cd Testes` ou `cd D:` ou `cd Testes/Capitulo02`, também pode ser usado para sair de um diretório `cd ..`.

---

Para compilar um arquivo **.java** vamos utilizar o comando `javac`, exemplo:

{% highlight java %}
javac Principal.java
{% endhighlight %}

Se houver erros de compilação aparecerá algum texto no prompt, caso contrário todo o código foi compilado com sucesso. Note que no diretório que

Para executar um arquivo **.class** vamos usar o comando `java`, exemplo:

{% highlight java %}
java Principal
{% endhighlight %}

Quando executarmos a classe `Principal` no Prompt aparecerá a mensagem `Qual o seu nome?`, digite o seu nome e aperte ENTER.

O que aconteceu?

Esperamos que tenha aparecido uma mensagem na tela dizendo `Olá  seu_nome!`

O que acabamos de fazer é um **teste de usuário** para saber se a aplicação está funcionando corretamente, mas imagine você tendo que fazer esse teste em uma grande aplicação parte por parte, com certeza demandaria um grande tempo.

## Testando 1, 2, 3, testando

Agora que temos a classe que executa a função de saudação e já temos a classe que faz a interface com o usuário.

Vamos criar um **teste de unidade**, mas o que podemos testar? Vamos testar se a saudação está sendo feita corretamente, só que agora vamos criar um programa que executa o teste.

Crie um novo arquivo chamado `TesteOla.java`, junto com os demais arquivos criados, contendo o seguinte código:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class TesteOla {
	@Test
	public void testeSaudacao() {
		Ola ola = new Ola();
		assertEquals("Oi Rafael!", ola.saudacao("Rafael"));
	}
}
{% endhighlight %}

A classe de teste é uma classe Java comum, sendo as principais diferenças os imports do `org.junit` e os métodos com as anotações `@Test`.

O `import org.junit.Test;` informa que queremos usar a anotação chamada `Test`, essa anotação serve para informar quais métodos serão os testes do programa, portanto será declarado sempre antes da declaração do método de teste, exemplo:

{% highlight java %}
@Test
public void testeSaudacao() {
{% endhighlight %}

Nesse código também temos o uso do método `assertEquals`, esse método recebe duas informação para verificar se ambas são iguais: primeiro o valor esperado e segundo o valor gerado pelo código, exemplo:

{% highlight java %}
assertEquals("Oi Rafael!", ola.saudacao("Rafael"));
{% endhighlight %}

Nesse exemplo o teste está verificando ser o texto `"Oi Rafael!"` será igual ao texto devolvido pela chamada do método `ola.saudacao("Rafael")`.

O objetivo desse teste é verificar se o método **saudacao** da class **Ola** está funcionando corretamente, mas sem termos que manualmente executar o programar.

Agora que criamos o código de teste vamos compilar a classe de teste para isso execute o comando:

{% highlight java %}
javac -cp junit-4.12.jar TesteOla.java Ola.java
{% endhighlight %}

Agora para compilar a classe `TesteOla.java` precisamos especificar que temos como dependência o arquivo `junit-4.12.jar` que é a API do JUnit e precisa da classe `Ola`.

Depois, vamos executar a classe de testes `TesteOla`. Para executar o teste vamos utilizar a seguinte linha de comando:

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore
TesteOla
{% endhighlight %}

Nesse comando especificamos as dependências com as bibliotecas junit-4.12.jar e hamcrest-core-1.3.jar. A classe que executará os testes é a `org.junit.runner.JUnitCore` e essa classe que executará os testes que criamos na classe `TesteOla`.

> Se você estiver executando essa classe no Mac OS ou Linux, ao invés de usar ponto e virgula (;) deve usar dois pontos para separar as dependências.

E como resultado teremos a seguinte saída:

{% highlight java %}
JUnit version 4.12
.
Time: 0,007

OK (1 test)
{% endhighlight %}

Essa saída informa que um teste foi executado com sucesso. Note que agora executamos o mesmo teste que o usuário executou, mas agora estamos fazendo isso de forma automática.