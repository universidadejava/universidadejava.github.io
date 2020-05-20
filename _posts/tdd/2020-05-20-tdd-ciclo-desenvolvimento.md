---
layout: article
title: "TDD - Entendendo o ciclo do TDD"
categories: tdd
author: sakurai
date: 2020-05-20 12:15:00
tags: [java, tdd, testes]
published: true
excerpt: Pratique as três leis do TDD, (1) primeiro vamos criar o teste, (2) depois vamos implementar a funcionalidade e (3) por último vamos refatorar o código.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Entendendo o ciclo do TDD

No post [Olá testes](http://www.universidadejava.com.br/tdd/tdd-ola-testes/) vimos como criar um teste, mas primeiro desenvolvemos o programa e depois criamos o teste. Agora vamos começar a seguir as três leis do TDD, (1) primeiro vamos criar o teste, (2) depois vamos implementar a funcionalidade e (3) por último vamos refatorar o código.

Nesse exemplos vamos criar um programa para calcular operações matemáticas simples, como: somar, subtrair, multiplicar, dividir e resto da divisão.

> Caso você prefira, gravei um vídeo com o exemplo da calculadora apresentado nesse post.

<iframe width="560" height="315" src="https://www.youtube.com/embed/G7S5JMj8vY4" frameborder="0" allowfullscreen></iframe>

## Pensando no que vamos testar

Nesse exemplo vamos seguir as boas práticas do Desenvolvimento Guiado por Testes, para começar podemos criar uma lista de possíveis testes que devemos fazer:

* Testar a soma de dois números;
* Testar a subtração de dois números;
* Testar a divisão de dois números;
* Testar a multiplicação de dois números.

> Observação: Para facilitar vamos utilizar apenas valores inteiros, nos próximos exemplos utilizaremos também valores com casas decimais.

Não precisamos implementar os testes na ordem como escrevemos, e também podemos adicionar e remover itens na lista.

## Criando o teste somar

Crie uma classe de teste chamado `CalculadoraTest`. Vamos começar testando o primeiro item da lista **Testar a soma de dois números**, para isso vamos adicionar na classe `CalculadoraTest` o método `testarSoma`, como mostrado a seguir:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

/**
 * Classe para testar os métodos da calculadora.
 */
public class CalculadoraTest {
    @Test
    public void testarSoma() {
        Calculadora calc = new Calculadora();
    }
}
{% endhighlight %}

Então, temos o nosso primeiro teste que falha. Se tentarmos compilar o código com o comando:

{% highlight java %}
javac -cp junit-4.12.jar CalculadoraTest.java
{% endhighlight %}

Teremos a seguinte saída:

{% highlight java %}
CalculadoraTest.java:10: error: cannot find symbol
        Calculadora calc = new Calculadora();
        ^
  symbol:   class Calculadora
  location: class CalculadoraTest
CalculadoraTest.java:10: error: cannot find symbol
        Calculadora calc = new Calculadora();
                               ^
  symbol:   class Calculadora
  location: class CalculadoraTest
2 errors
{% endhighlight %}

O erro indica que não encontrou a classe `Calculadora`.

Agora precisamos implementar o mínimo necessário para fazer o teste funcionar, então vamos criar a classe `Calculadora`.

{% highlight java %}
/**
 * Executa as operações de soma, subtração, multiplicação, divisão e
 * resto da divisão.
 */
public class Calculadora {

}
{% endhighlight %}

Compilando novamente temos tudo funcionando:

{% highlight java %}
javac -cp junit-4.12.jar CalculadoraTest.java Calculadora.java
{% endhighlight %}

Mas espere um momento, o que esse teste está testando? Nada né, não temos nenhuma verificação, mas o que podemos verificar no teste?

1. Podemos verificar se 1 + 1 resulta em 2;
2. Podemos verificar se 1 + 0 resulta em 1;
3. Podemos verificar se 1 + -1 resulta em 0.

Então vamos adicionar algumas verificações no teste `testarSoma`.

{% highlight java %}
@Test
public void testarSoma() {
    Calculadora calc = new Calculadora();
    assertEquals(2, calc.somar(1,1));
    assertEquals(1, calc.somar(1,0));
    assertEquals(0, calc.somar(1,-1));
}
{% endhighlight %}

> Observação: Note que no nome do método `testarSoma` estamos definindo a primeira palavra em minúsculo e a partir da segunda palavra o seu primeiro caractere em maiúsculo, essa é uma boa prática da linguagem de programação Java, utilizaremos esse padrão em todos os métodos.

Adicionamos três verificações para o método `somar` que deve receber como parâmetro dois números inteiros e devolver um número inteiro que é a soma desses dois números.

Mas o método `somar` ainda não existe na classe `Calculadora`, se executarmos o teste teremos esses erros de compilação:

{% highlight java %}
CalculadoraTest.java:11: error: cannot find symbol
        assertEquals(2, calc.somar(1,1));
                            ^
  symbol:   method somar(int,int)
  location: variable calc of type Calculadora
CalculadoraTest.java:12: error: cannot find symbol
	    assertEquals(1, calc.somar(1,0));
	                        ^
  symbol:   method somar(int,int)
  location: variable calc of type Calculadora
CalculadoraTest.java:13: error: cannot find symbol
	    assertEquals(0, calc.somar(1,-1));
	                        ^
  symbol:   method somar(int,int)
  location: variable calc of type Calculadora
3 errors
{% endhighlight %}

Os três erros indicam a mesma coisa, está tentando usar o método `somar(int, int)` da classe `Calculadora`, mas esse método não existe.

Então vamos implementar o método `somar` na classe `Calculadora`.

{% highlight java %}
/**
 * Executa as operações de soma, subtração, multiplicação, divisão e resto da divisão.
 */
public class Calculadora {
    public int somar(int a, int b) {
        return a + b;
    }
}
{% endhighlight %}

O método somar recebe como parâmetro dois números inteiros (int) e devolve um número inteiro (int) com o valor da soma dos dois parâmetros recebidos.

Agora podemos compilar a classe CalculadoraTest:

{% highlight java %}
javac -cp junit-4.12.jar CalculadoraTest.java Calculadora.java
{% endhighlight %}

Não temos mais erro de compilação. Agora se executarmos o teste da classe `CalculadoraTest`:

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore CalculadoraTest

JUnit version 4.12
.
Time: 0,007

OK (1 test)
{% endhighlight %}

Legal, nosso primeiro teste está funcionando corretamente.

## Criando o teste para subtrair

Vamos adicionar o teste para o segundo item da lista `Testar a subtração de dois números` na classe `CalculadoraTest`.

Na classe `CalculadoraTest` após o método do `testarSoma`, vamos adicionar mais um método de teste chamado `testarSubtracao`. O código ficará assim:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

/**
 * Classe para testar os métodos da calculadora.
 */
public class CalculadoraTest {
    @Test
    public void testarSoma() {
        Calculadora calc = new Calculadora();
        assertEquals(2, calc.somar(1, 1));
        assertEquals(1, calc.somar(1, 0));
        assertEquals(0, calc.somar(1, -1));
    }

    @Test
    public void testarSubtracao() {
        Calculadora calc = new Calculadora();
        assertTrue(calc.subtrair(1,1) == 0);
        assertTrue(calc.subtrair(1,-1) == 2);
        assertTrue(calc.subtrair(-1,1) == -2);
    }
}
{% endhighlight %}

> Note que no nome do método não usamos espaço, acentos ou cedilhas e todos os métodos de teste são públicos (public), não tem retorno (void) e antes de sua declaração possuem a anotação @Test.

Para apresentar outras formas de verificações, no método `testarSubtracao` foi utilizado o método `assertTrue` que verifica se uma determinada condição é `true` (verdade). Então se chamar o método `calc.subtrair(1,1)` e seu retorno for igual a `0 (zero)` essa comparação devolverá `true` e a verificação será valida, caso devolva algum número diferente de 0 (zero) a condição será false (falso) e teremos um erro no teste.

Se executarmos a classe `CalculadoraTest` teremos novamente um erro de compilação, porque ainda não criamos o método subtrair.

{% highlight java %}
CalculadoraTest.java:19: error: cannot find symbol
        assertTrue(calc.subtrair(1,1) == 0);
                       ^
  symbol:   method subtrair(int,int)
  location: variable calc of type Calculadora
CalculadoraTest.java:20: error: cannot find symbol
        assertTrue(calc.subtrair(1,-1) == 2);
                       ^
  symbol:   method subtrair(int,int)
  location: variable calc of type Calculadora
CalculadoraTest.java:21: error: cannot find symbol
        assertTrue(calc.subtrair(-1,1) == -2);
                       ^
  symbol:   method subtrair(int,int)
  location: variable calc of type Calculadora
3 errors
{% endhighlight %}

Para corrigir o teste vamos implementar o método `subtrair` na classe `Calculadora`:

{% highlight java %}
/**
 * Executa as operações de soma, subtração, multiplicação e divisão.
 */
public class Calculadora {
    public int somar(int a, int b) {
        return a + b;
    }

    public int subtrair(int a, int b) {
        return a - b;
    }
}
{% endhighlight %}

Agora se executarmos novamente os testes teremos saída dos testes igual a apresentada a seguir:

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore
CalculadoraTest
{% endhighlight %}

{% highlight java %}
JUnit version 4.12
..
Time: 0,006

OK (2 tests)
{% endhighlight %}

Então note que dentro de uma mesma classe de testes podemos criar diversos testes, normalmente testes sobre a mesma classe ou mesmo assunto. E dentro de cada teste podemos colocar várias verificações diferentes.

Se algum teste falhar, uma mensagem de erro será apresentada, por exemplo:

{% highlight java %}
JUnit version 4.12
.E.
Time: 0,008
There was 1 failure:
1) testarSubtracao(CalculadoraTest)
java.lang.AssertionError
	at org.junit.Assert.fail(Assert.java:86)
	at org.junit.Assert.assertTrue(Assert.java:41)
	at org.junit.Assert.assertTrue(Assert.java:52)
	at CalculadoraTest.testarSubtracao(CalculadoraTest.java:19)
    ...

FAILURES!!!
Tests run: 2,  Failures: 1
{% endhighlight %}

Quando usamos o `assertTrue`, o teste que falha informa em qual teste e linha do código ocorreu a falha:

{% highlight java %}
at CalculadoraTest.testarSubtracao(CalculadoraTest.java:19)
{% endhighlight %}

E quando usamos o `assertEquals`, o teste que falha informa qual o valor esperado e qual o valor recebido:

{% highlight java %}
1) testarSoma(CalculadoraTest)
java.lang.AssertionError: expected:<3> but was:<2>
{% endhighlight %}

Note como estamos seguindo os passos do Desenvolvimento Guiado por Testes, primeiro testamos um código que ainda não existe e depois implementamos esse código.

## Testes para multiplicar, dividir e resto

Mostrei os dois primeiros, agora é hora de você práticar um pouco, implemente os testes para multiplicar (*), dividir (/) e resto da divisão (%).

Depois que você terminar compare com o código a seguir:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

/**
 * Classe para testar os métodos da calculadora.
 */
public class CalculadoraTest {
    @Test
    public void testarSoma() {
        Calculadora calc = new Calculadora();
        assertEquals(2, calc.somar(1, 1));
        assertEquals(1, calc.somar(1, 0));
        assertEquals(0, calc.somar(1, -1));
    }

    @Test
    public void testarSubtracao() {
        Calculadora calc = new Calculadora();
        assertTrue(calc.subtrair(1,1) == 0);
        assertTrue(calc.subtrair(1,-1) == 2);
        assertTrue(calc.subtrair(-1,1) == -2);
    }

    @Test
    public void testarDivisao() {
        Calculadora calc = new Calculadora();
        assertEquals(1, calc.dividir(3,2));
        assertFalse(calc.dividir(3, 3) == 3);
        assertTrue(calc.dividir(3,3) == 1);
    }

    @Test
    public void testarMultiplicacao() {
        Calculadora calc = new Calculadora();
        assertEquals(6, calc.multiplicar(3, 2));
        assertTrue(calc.multiplicar(3, 3) == 9);
    }

    @Test
    public void testarResto() {
        Calculadora calc = new Calculadora();
        assertEquals(1, calc.resto(3, 2));
        assertTrue(calc.resto(3, 3) == 0);
    }
}
{% endhighlight %}

{% highlight java %}
/**
 * Executa as operações de soma, subtração, multiplicação, divisão e resto da divisão.
 */
public class Calculadora {
    public int somar(int a, int b) {
        return a + b;
    }

    public int subtrair(int a, int b) {
        return a - b;
    }

    public int dividir(int a, int b) {
        return a / b;
    }

    public int multiplicar(int a, int b) {
        return a * b;
    }

    public int resto(int a, int b) {
        return a % b;
    }
}
{% endhighlight %}

## Criando a interação com o usuário

Ótimo, a calculadora já está pronta, faz todas as operações que planejamos inicialmente.

Agora vamos criar uma classe para fazer a interação com o usuário, podemos criar algo como isso:

{% highlight java %}
import java.util.Scanner;

/**
 * Classe que faz a interação do usuário com a calculadora.
 */
public class Principal {
  public static void main(String[] args) {
    Scanner s = new Scanner(System.in);
    System.out.println("##### Calculadora #####");
    System.out.println("Voce deseja: ");
    System.out.println("1 - Somar");
    System.out.println("2 - Subtrair");
    System.out.println("3 - Dividir");
    System.out.println("4 - Multiplicar");
    System.out.println("5 - Resto");
    System.out.print("Opcao: ");
    int op = s.nextInt();
    System.out.print("Legal, digite o primeiro numero: ");
    int numero1 = s.nextInt();
    System.out.print("E agora digite o segundo numero: ");
    int numero2 = s.nextInt();

    Calculadora calc = new Calculadora();

    if(op == 1) {
      System.out.println("A soma eh " +
        calc.somar(numero1, numero2));
    } else if(op == 2) {
      System.out.println("A subtracao eh " +
        calc.subtrair(numero1, numero2));
    } else if(op == 3) {
      System.out.println("A divisao eh " +
        calc.dividir(numero1, numero2));
    } else if(op == 4) {
      System.out.println("A multiplicacao eh " +
        calc.multiplicar(numero1, numero2));
    } else if(op == 4) {
      System.out.println("O resto eh " +
        calc.resto(numero1, numero2));
    }

    System.out.println("##### Fim #####");
  }
}
{% endhighlight %}

A classe Principal, vai executar e apresentar um menu de opções para que o usuário possa escolher qual operação da calculadora executar, o método `s.nextInt()` fica aguardando o usuário digitar um número.

Logo em seguida será perguntado quais os dois números usados no calculo. No final apresenta o resultado da operação.

Se executarmos essa classe então teremos uma saída parecida com essa:

{% highlight java %}
##### Calculadora #####
Voce deseja:
1 - Somar
2 - Subtrair
3 - Dividir
4 - Multiplicar
5 - Resto
Opcao: 4
Legal, entao digite o primeiro numero inteiro: 3
E agora digite o segundo numero inteiro: 5
A multiplicacao eh 15
##### Fim #####
{% endhighlight %}

## Experiência do usuário em ação

Mas espere um pouco, quando você usa uma calculadora ela te pergunta qual operação você quer executar?

O certo não seria passar uma operação como:

{% highlight java %}
3 * 5
{% endhighlight %}

E a calculadora mostrar o resultado:

{% highlight java %}
15
{% endhighlight %}

Acho que faz mais sentido para o usuário montar a operação matemática e apenas receber uma resposta do que ter que ficar escolhendo qual operação deseja usar.

Para fazer isso vamos precisar de algo a mais, precisamos criar mais um teste que recebe uma operação matemática e devolve o seu resultado. Então, vamos adicionar mais um item na nossa lista de testes:

* Testar se ao receber uma operação matemática devolverá o seu resultado.

Note que a lista de testes é apenas um guia para se lembrar de quais testes serão implementados, se você quiser a qualquer momento pode incluir ou retirar algum item da lista.

Vamos voltar para a classe de teste `CalculadoraTest` e vamos adicionar mais um teste chamado `testarCalculo`.

{% highlight java %}
@Test
public void testarCalculo() {
    Calculadora calc = new Calculadora();
    assertEquals(15, calc.calcular("3 * 5"));
}
{% endhighlight %}

Vamos implementar o método `calcular` na classe `Calculadora` que recebe como parâmetro uma String com a operação matemática e devolve seu resultado:

{% highlight java %}
public int calcular(String op) {
    String[] partes = op.split(" ");

    int numero1 = Integer.valueOf(partes[0]);
    int numero2 = Integer.valueOf(partes[2]);

    if("+".equals(partes[1])) {
        return somar(numero1, numero2);
    } else if("-".equals(partes[1])) {
        return subtrair(numero1, numero2);
    } else if("/".equals(partes[1])) {
        return dividir(numero1, numero2);
    } else if("*".equals(partes[1])) {
        return multiplicar(numero1, numero2);
    } else if("%".equals(partes[1])) {
        return resto(numero1, numero2);
    }

    return 0;
}
{% endhighlight %}

No método `calcular` primeiro pegamos a `String op` e utilizamos o seu método chamado `split` para dividir o texto de acordo com um separador, nesse caso o separador é um espaço em branco `" "`.

O método `split` vai devolver o texto dividido em três partes e cada uma dessas partes estará guardada em uma posição nesse vetor chamado `partes` como mostrado na tabela:

| Posição | 0 | 1 | 2 |
| -- | -- | -- | -- |
| Texto | "3" | "*" | "5" |

Depois pegamos a posição 0 e 2 das partes da String e convertemos para números inteiros através do método `Integer.valueOf`.

E por fim verificamos o texto que está na posição 1, que é a operação matemática, e executamos sua operação.

No final se o usuário utilizar alguma operação diferente de +, -, *, / e %, então será devolvido o número zero, porque não será possível calcular.

Se quiser, adicione mais algumas verificações no teste:

{% highlight java %}
@Test
public void testarCalculo() {
    Calculadora calc = new Calculadora();
    assertEquals(15, calc.calcular("3 * 5"));
    assertEquals(2, calc.calcular("4 / 2"));
    assertEquals(6, calc.calcular("1 + 5"));
    assertEquals(0, calc.calcular("2 - 2"));
}
{% endhighlight %}

A seguir temos a execução dos testes para verificar se está tudo funcionando.

{% highlight java %}
JUnit version 4.12
......
Time: 0,007

OK (6 tests)
{% endhighlight %}

Agora que implementamos mais esse item vamos modificar a classe `Principal` para usar o novo método `calcular`.

{% highlight java %}
import java.util.Scanner;

/**
 * Classe que faz a interação do usuário com a calculadora.
 */
public class Principal {
    public static void main(String[] args) {
        Scanner s = new Scanner(System.in);
        System.out.println("##### Calculadora #####");
        System.out.print("Digite sua operacao: ");
        String op = s.nextLine();
        Calculadora calc = new Calculadora();
        System.out.println("O resultado eh " + calc.calcular(op));
        System.out.println("##### Fim #####");
    }
}
{% endhighlight %}

Note como o código ficou bem mais enxuto e o melhor tiramos uma lógica de negócio que inicialmente tínhamos colocado na classe que realiza a interação com o usuário e passamos ela para a classe Calculadora que é responsável pela lógica.

E se executarmos essa classe teremos uma saída parecida com essa:

{% highlight java %}
##### Calculadora #####
Digite sua operacao: 4 / 2
O resultado eh 2
##### Fim #####
{% endhighlight %}

Nesse exemplo mostramos como seguir os passos do Desenvolvimento Guiado por Testes para implementar uma calculadora, o projeto é simples e para a calculadora poderia ser implementados muitas outras funcionalidades, mas para entender o passo a passo já é um bom exemplo.

## Exercícios

**Exercício 01)** No projeto `OlaTestes` criado no capítulo anterior adicione mais um método para receber o idioma e devolve uma saudação personalizada. A tabela a seguir apresenta alguns exemplos:

| Idioma | Saudação |
| -- | -- |
| Alemão | Hallo Rafael |
| Espanhol | Hola Rafael |
| Francês | Bonjour Rafael |
| Inglês | Hello Rafael |
| Japonês | Kon'nichiwa Rafael |

Lembre-se de criar o teste e as validações para todos os idiomas.

**Exercício 02)** Crie uma aplicação para calcular a área de um quadrado. Sabendo que:

{% highlight java %}
Área = Comprimento * Altura
{% endhighlight %}

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar para o usuário o comprimento e a altura do quadrado e imprima a sua área.
\end{itemize}

> Observação: Para facilitar a implementação e os testes utilize apenas números inteiros nos cálculos.

**Exercício 03)** Crie uma aplicação para uma venda de bolinhas de gude. Sua aplicação deve permitir que o cliente informe quantas bolinhas de gude ele deseja comprar, realize o calculo e mostre o valor que o cliente deve pagar, sendo que cada bolinha custa R$0,30.

Para facilitar a vida do cliente, devemos permitir que o seja informado a quantidade de bolinhas de diversas maneiras como, por exemplo:

* 10;
* 2 duzias = 24 bolinhas;
* 1 pote = 30 bolinhas;
* 1 cento = 100 bolinhas.

Então se pensarmos na execução da aplicação teremos algo como:

{% highlight java %}
##### Venda de Bolinhas de Gude #####
Quantas bolinhas deseja comprar: 2 duzias
O valor que deve ser pago eh R$7,20
##### Obrigado e volte sempre! #####
{% endhighlight %}

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar para o cliente quantas bolinhas ele quer comprar e mostre o valor que deve ser pago.


### Conteúdos relacionados

- [Introdução ao TDD](http://www.universidadejava.com.br/tdd/tdd-introducao/)
- [Começando a criar testes passo a passo](http://www.universidadejava.com.br/tdd/tdd-ola-testes/)
- [Introdução a refatoração](http://www.universidadejava.com.br/tdd/tdd-refatoracao/)
- [O impacto do TDD no design da aplicação](http://www.universidadejava.com.br/tdd/tdd-design-aplicacao/)