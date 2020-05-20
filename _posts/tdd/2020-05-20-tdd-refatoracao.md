---
layout: article
title: "TDD - Introdução a refatoração"
categories: tdd
author: sakurai
date: 2020-05-20 12:18:00
tags: [java, tdd, testes, refatoração]
published: true
excerpt: Refatorar é como melhoramos a qualidade do código sem alterar sua funcionalidade.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Uma breve introdução a refatoração

Agora que conhecemos o ambiente de desenvolvimento e vimos como criar um teste e sua implementação, vamos criar uma aplicação um pouco mais complexa seguindo as boas práticas do TDD.

## Exemplo ano do chassi
Dado o chassi de um veículo como por exemplo **9BP17164GF0000001**, será necessário identificar qual o ano de fabricação deste veículo, todo chassi possui um caractere alfa numérico (letra ou número) que informa o ano de fabricação, neste exemplo de chassi, o caractere **'F'** na **10º posição** informa que o do ano de fabricação do veículo é **2015**. A tabela a seguir possui alguns valores e os anos que eles representam.

| Valor | Ano |
| -- | -- |
| A | 2010 |
| B | 2011 |
| C | 2012 |
| D | 2013 |
| E | 2014 |
| F | 2015 |

Caso você prefira, gravei um vídeo com o exemplo de chassi apresentado nesse post:.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hxIbckFHOK8" frameborder="0" allowfullscreen></iframe>

> Observação: A estrutura de formação do chassi é um pouco mais complexa que isso e o código que representa o ano do chassi também pode ser um número, mas para facilitar o exemplo utilizaremos apenas caracteres para representar o ano. Para uma descrição mais detalhada da formação do chassi acesse o [Número de Identificação do Veículo no Wikipedia](http://pt.wikipedia.org/wiki/N%C3%BAmero_de_Identifica%C3%A7%C3%A3o_do_Ve%C3%ADculo).

Antes de começar essa funcionalidade, precisamos pensar quais os testes gostaríamos de fazer para que essa funcionalidade seja executada da melhor forma possível.

Poderíamos testar, por exemplo:

* Testar se o chassi com o valor A retorna o ano 2010;
* Testar se o chassi com o valor B retorna o ano 2011;
* Testar se o chassi com o valor C retorna o ano 2012;
* Testar se um chassi pode ser escrito com caracteres em maiúsculo ou minúsculo;
* Testar se um chassi incompleto retorna um erro de informação inválida.

Durante a implementação dos testes, novas dúvidas podem sugir e nada impede que adicionemos mais testes à essa lista inicial de testes.

Primeiro será necessário criar um teste de algo que ainda não foi implementado. Mas o que testar se nada existe?

Sabemos que ao informar o chassi `9BP17164GA0000001` e a posição `10` é esperado a aplicação informe que o ano desse chassi é `2010`, portanto podemos criar a classe `ChassiUtilTest` e adicionar um teste para validar nosso primeiro item da lista `Testar se o chassi com o valor A retorna o ano 2010`.

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class ChassiUtilTest {
  @Test
  public void testarAnoAChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9BP17164GA0000001", 10));
  }
}
{% endhighlight %}

Foi criado uma classe chamada **ChassiUtilTest** para realizar todos os testes, nela criamos um método chamado `testarAnoAChassi()` que será utilizado para testar se o valor da posição do ano no chassi for **A** será informado que o ano do chassi é **2010**.

Na linguagem de programação Java, podemos utilizar um framework de testes chamado **JUnit** para auxiliar na criação dos testes, neste momento utilizamos a anotação **@Test** que serve para informar que este método `testarAnoAChassi()` deve ser executado como um teste do JUnit, também foi utilizado o método `assertEquals(valor1, valor2)` que compara se os dois valores são iguais.

> O JUnit possui várias formas de testar condições afirmando valores, como por exemplo: assertArrayEquals, assertEquals, assertFalse, assertNotNull, assertNotSame, assertThat e assertTrue.

Neste primeiro momento a classe `ChassiUtilTest` não irá nem compilar, porque a classe ChassiUtil não existe, portanto ao executar os testes dessa classe teremos a saída:

{% highlight java %}
javac -cp junit-4.12.jar ChassiUtilTest.java
{% endhighlight %}

{% highlight java %}
ChassiUtilTest.java:7: error: cannot find symbol
    ChassiUtil util = new ChassiUtil();
    ^
  symbol:   class ChassiUtil
  location: class ChassiUtilTest
ChassiUtilTest.java:7: error: cannot find symbol
    ChassiUtil util = new ChassiUtil();
                          ^
  symbol:   class ChassiUtil
  location: class ChassiUtilTest
2 errors
{% endhighlight %}

Agora precisamos implementar apenas o suficiente para que o teste funcione. Crie a classe **ChassiUtil** que implementará o método `obterAno(String chassi, int posicaoAno)` retornando apenas o valor **2010**.

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    return 2010;
  }
}
{% endhighlight %}

> Talvez você pense: mas porque cargas d'água, ele implementou esse método e retornou o valor **2010**? A resposta é simples, porque no teste estamos esperando que esse método retorno o valor 2010, então estamos implementando o mínimo necessário para o teste funcionar.

Então foi criado uma implementação com o suficiente para que o teste funcione, agora ao executar o teste teremos a saída do JUnit:

{% highlight java %}
javac -cp junit-4.12.jar ChassiUtilTest.java ChassiUtil.java

java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore ChassiUtilTest

JUnit version 4.12
.
Time: 0,007

OK (1 test)
{% endhighlight %}

Como a implementação ainda é bem simples, podemos agora **refatorar** este método para implementar ele de uma maneira melhor, como por exemplo: verificar se o caractere na posição recebida como parâmetro tem o valor igual a **'A'**.

**Refatorar** é como melhoramos a qualidade do código sem alterar sua funcionalidade.

Então vamos modificar a implementação do método:

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    /* Como os caracteres começam na posição zero,
    é necessário subtrair 1 da posição do ano informada. */
    if(chassi.charAt(posicaoAno - 1) == 'A') {
      return 2010;
    }
    return 0;
  }
}
{% endhighlight %}

A classe `String` possui o método `charAt` que devolve o caracter que está na posição informada, nesse caso o método recebe uma posição do ano (lembra que nesse chassi a posição é 10º), mas como **o primeiro caractere da String começa na posição zero**, então subtraimos `-1` do parâmetro `posicaoAno`. Se o valor do caractere for igual a **'A'** então devolve o valor **2010**, caso contrário devolverá o valor **0**.

Agora que foi alterado a implementação é necessário testar novamente para verificar se a alteração não causará a falha dos testes:

{% highlight java %}
javac -cp junit-4.12.jar ChassiUtilTest.java ChassiUtil.java

java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore ChassiUtilTest

JUnit version 4.12
.
Time: 0,006

OK (1 test)
{% endhighlight %}

Agora que o teste esta funcionando e implementamos o mínimo necessário para ele funcionar, precisamos pensar em o que mais pode ser testado como, por exemplo:  se o chassi for informado com os caracteres em minúsculo 9bp17164ga0000001 será que ele irá funcionar?

Adicione mais um teste a classe `ChassiUtilTest` chamado `testarAnoAMinusculoChassi()` para testar se um chassi que possui os caracteres em minúsculo também irá retornar o ano corretamente.

> Note que definimos os nomes dos testes bem informativos e específicos com o que será testado.

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class ChassiUtilTest {
  @Test
  public void testarAnoAChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9BP17164GA0000001", 10));
  }

  @Test
  public void testarAnoAMinusculoChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9bp17164ga0000001", 10));
  }
}
{% endhighlight %}

Vamos executar novamente os testes da classe ChassiUtilTeste e teremos a saída:

{% highlight java %}
JUnit version 4.12
..E
Time: 0,008
There was 1 failure:
1) testarAnoAMinusculoChassi(ChassiUtilTest)
java.lang.AssertionError: expected:<2010> but was:<0>
	...
	at ChassiUtilTest.testarAnoAMinusculoChassi(ChassiUtilTest.java:14)
	...

FAILURES!!!
Tests run: 2,  Failures: 1
{% endhighlight %}

Ao executar os testes da classe ChassiUtilTest agora temos um erro no teste `testarAnoAMinusculoChassi()`, mas já havíamos refatorado a classe ChassiUtil e mesmo assim porque os testes não funcionam? A implementação do que está sendo testado cobre apenas o necessário para que os testes existentes funcionem, mas conforme novos testes são adicionados pode ocorrer da implementação atual não tratar uma determinada situação.

Vamos refatorar novamente a classe ChassiUtil para que os dois testes atuais funcionem, para isso podemos apenas sempre converter o texto do chassi para maiúsculo antes de verificar o valor do caractere.

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    /* Como os caracteres começam na posição zero,
    é necessário subtrair 1 da posição do ano informada. */
    if(chassi.toUpperCase().charAt(posicaoAno - 1) == 'A') {
      return 2010;
    }
    return 0;
  }
}
{% endhighlight %}

Estamos convertendo o chassi para maiúsculo com o método `toUpperCase()` e depois verificamos se o caractere é igual a **’A’**.

Podemos testar novamente:

{% highlight java %}
JUnit version 4.12
..
Time: 0,004

OK (2 tests)
{% endhighlight %}

Agora continuando os testes, precisamos testar se o chassi que possui o caractere **'B'** tem o ano **2011**, então vamos criar mais um teste:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class ChassiUtilTest {
  @Test
  public void testarAnoAChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9BP17164GA0000001", 10));
  }

  @Test
  public void testarAnoAMinusculoChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9bp17164ga0000001", 10));
  }

  @Test
  public void testarAnoBChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2011, util.obterAno("9BP17164GB0000001", 10));
  }
}
{% endhighlight %}

O método chamado `testarAnoBChassi()` foi utilizado para testar se o valor da posição do ano no chassi **9BP17164GB0000001** é **2011**. Ao testar novamente esta classe teremos a saída:

{% highlight java %}
JUnit version 4.12
..E.
Time: 0,008
There was 1 failure:
1) testarAnoBChassi(ChassiUtilTest)
java.lang.AssertionError: expected:<2011> but was:<0>
	…
	at ChassiUtilTest.testarAnoBChassi(ChassiUtilTest.java:20)
	…
FAILURES!!!
Tests run: 3,  Failures: 1
{% endhighlight %}

O teste falhou porque esperava 2011, mas retornou 0. Ainda não foi implementado, portanto vamos implementar uma condição que quando o caractere da posição informada for igual a **'B'** deve retornar o ano **2011**.

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    /* Como os caracteres começam na posição zero,
    é necessário subtrair 1 da posição do ano informada. */
    if(chassi.toUpperCase().charAt(posicaoAno - 1) == 'A') {
      return 2010;
    } else if(chassi.toUpperCase().charAt(posicaoAno - 1) == 'B') {
      return 2011;
    }

    return 0;
  }
}
{% endhighlight %}

Agora que criamos uma implementação podemos testar novamente os testes para verificar se todos eles ainda estão funcionando, o resultado da saída é:

{% highlight java %}
JUnit version 4.12
...
Time: 0,006

OK (3 tests)
{% endhighlight %}

Ao criar cada teste e executá-lo, temos um retorno rápido informando se o que foi implementado funcionará, desta maneira podemos verificar se as novas alterações não irão causar a falha de algum e caso isso ocorra sabemos que a ultima alteração realizada na implementação que causou a falha dos testes, isso é chamado de **Feedback**.

Continuando a implementação precisamos testar se o chassi que possui o caractere **'C'** tem o ano **2012**, então vamos criar mais um teste:

{% highlight java %}
package br.metodista.ads1.guia.tdd;

import org.junit.Test;
import static org.junit.Assert.*;

public class ChassiUtilTest {
  @Test
  public void testarAnoAChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9BP17164GA0000001", 10));
  }

  @Test
  public void testarAnoAMinusculoChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2010, util.obterAno("9bp17164ga0000001", 10));
  }

  @Test
  public void testarAnoBChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2011, util.obterAno("9BP17164GB0000001", 10));
  }

  @Test
  public void testarAnoCChassi() {
    ChassiUtil util = new ChassiUtil();
    assertEquals(2012, util.obterAno("9BP17164GC0000001", 10));
  }
}
{% endhighlight %}

O método chamado `testarAnoCChassi()` foi utilizado para testar se o valor da posição do ano no chassi **9BP17164GC0000001** será informado que o ano do chassi é **2012**.

Note que em todos os métodos criamos um objeto da classe `ChassiUtil` chamado `util`, para poder testar os métodos que ele possui. O JUnit possui uma anotação chamada **@Before** que é adicionado a um método para ser executado antes que comece a execução dos testes e uma anotação chamada **@After** que é adicionado a um método para ser executado após a execução de todos os métodos.

Então, podemos alterar a classe de teste para criar o objeto da classe ChassiUtil antes de iniciar os testes, ao invés de ficar criando um objeto em cada teste.

{% highlight java %}
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class ChassiUtilTest {
  private ChassiUtil util = null;

  @Before
  public void inicializar() {
    util = new ChassiUtil();
  }

  @Test
  public void testarAnoAChassi() {
    assertEquals(2010, util.obterAno("9BP17164GA0000001", 10));
  }

  @Test
  public void testarAnoAMinusculoChassi() {
    assertEquals(2010, util.obterAno("9bp17164ga0000001", 10));
  }

  @Test
  public void testarAnoBChassi() {
    assertEquals(2011, util.obterAno("9BP17164GB0000001", 10));
  }

  @Test
  public void testarAnoCChassi() {
    assertEquals(2012, util.obterAno("9BP17164GC0000001", 10));
  }
}
{% endhighlight %}

O método `inicializar()` possui a anotação `@Before`, então esse método será chamado antes da execução de cada teste criando um novo objeto **ChassiUtil**. Então vamos testar o código:

{% highlight java %}
JUnit version 4.12
.E...
Time: 0,008
There was 1 failure:
1) testarAnoCChassi(ChassiUtilTest)
java.lang.AssertionError: expected:<2012> but was:<0>
	...
	at ChassiUtilTest.testarAnoCChassi(ChassiUtilTest.java:30)
	...

FAILURES!!!
Tests run: 4,  Failures: 1
{% endhighlight %}

O teste falhou porque esperava 2012, mas retornou 0. Ainda não foi implementado a verificação para o código **'C'**, portanto vamos implementar uma condição que quando o caractere da posição informada for igual a **'C'** deve retornar o ano **2012**.

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    /* Como os caracteres começam na posição zero,
    é necessário subtrair 1 da posição do ano informada. */
    if(chassi.toUpperCase().charAt(posicaoAno - 1) == 'A') {
      return 2010;
    } else if(chassi.toUpperCase().charAt(posicaoAno - 1) == 'B') {
      return 2011;
    } else if(chassi.toUpperCase().charAt(posicaoAno - 1) == 'C') {
      return 2012;
    }

    return 0;
  }
}
{% endhighlight %}

Vamos compilar e executar novamente:

{% highlight java %}
JUnit version 4.12
....
Time: 0,006

OK (4 tests)
{% endhighlight %}

Pronto agora temos os quatro testes funcionando.

> Note que as últimas implementações foram apenas uma copia da implementação do primeiro teste apenas alterando o valor do caractere e o retorno, nosso código está ficando cheio de **if / else**.

Vamos parar um pouco com esse ciclo de adicionar um novo caracterer **'D'**, **'E'**, **'F'**, **etc** e pensar um pouco, podemos escrever esse mesmo código sem esse monte de **if / else**? Pensa ai, como você arrumaria esse código?

## Refatoração \o/

Vamos refatorar esta classe para ao invés de utilizamos diversos **if / else** vamos calcular o ano baseado no caractere.

Em Java todo caractere é representado por um código [Unicode](http://pt.wikipedia.org/wiki/Unicode), o caractere **'A'** equivale ao número **65**, o caractere **'B'** equivale ao número **66** e assim por diante, portanto ao subtrair o caractere do chassi por **'A'** teremos o valor que deve ser adicionado com **2010** para encontrar o ano informado.

Então se pegarmos o caracter que está no chassi e converter para maiúsculo:

{% highlight java %}
char caractere = chassi.toUpperCase().charAt(posicaoAno - 1);
{% endhighlight %}

E desse caractere subtrairmos o valor de 'A' e somarmos o ano 2010, teremos nossa resposta:

{% highlight java %}
int resposta = (caractere - 'A') + 2010;
{% endhighlight %}

Então, a classe **ChassiUtil** ficará com o seguinte código:

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    /* Como os caracteres começam na posição zero, é necessário
       subtrair 1 da posição do ano informada. */
    char caractere = chassi.toUpperCase().charAt(posicaoAno - 1);
    return (caractere - 'A') + 2010;
  }
}
{% endhighlight %}

Muito mais clean :D

Após essa alteração execute novamente os testes para verificar se tudo está funcionando:

{% highlight java %}
JUnit version 4.12
....
Time: 0,005

OK (4 tests)
{% endhighlight %}

Muito bom. Vamos adicionar mais um teste que espera obter uma exceção quando um chassi inválido for informado.

{% highlight java %}
import org.junit.Before;
import org.junit.Test;
import static org.junit.Assert.*;

public class ChassiUtilTest {
  private ChassiUtil util = null;

  @Before
  public void inicializar() {
    util = new ChassiUtil();
  }

  @Test
  public void testarAnoAChassi() {
    assertEquals(2010, util.obterAno("9BP17164GA0000001", 10));
  }

  @Test
  public void testarAnoAMinusculoChassi() {
    assertEquals(2010, util.obterAno("9bp17164ga0000001", 10));
  }

  @Test
  public void testarAnoBChassi() {
    assertEquals(2011, util.obterAno("9BP17164GB0000001", 10));
  }

  @Test
  public void testarAnoCChassi() {
    assertEquals(2012, util.obterAno("9BP17164GC0000001", 10));
  }

  @Test(expected=IllegalArgumentException.class)
  public void testarExcecaoDeChassiInvalido() {
    util.obterAno("teste", 10);
  }
}
{% endhighlight %}

O método `testarExcecaoDeChassiInvalido()` testa se o método `obterAno` recebendo um chassi inválido como por exemplo o valor `"teste"` lança uma exceção do tipo `IllegalArgumentException` (exceção de argumento inválido ou ilegal), para tratar que espera esta exceção adicionamos na anotação `@Test` a propriedade `expected` que recebe como parâmetro a classe da exceção que será lançada.

{% highlight java %}
JUnit version 4.12
..E...
Time: 0,009
There was 1 failure:
1) testarExcecaoDeChassiInvalido(ChassiUtilTest)
java.lang.Exception: Unexpected exception, expected<java.lang.IllegalArgumentException>
but was<java.lang.StringIndexOutOfBoundsException>
	...
Caused by: java.lang.StringIndexOutOfBoundsException: String index out of range: 9
	at java.lang.String.charAt(String.java:646)
	at ChassiUtil.obterAno(ChassiUtil.java:5)
	at ChassiUtilTest.testarExcecaoDeChassiInvalido(ChassiUtilTest.java:35)
	...

FAILURES!!!
Tests run: 5,  Failures: 1
{% endhighlight %}

O teste mostra que era esperado a exceção IllegalArgumentException, mas o método lançou o erro `StringIndexOutOfBoundException`, porque não tratamos na implementação os textos de chassi inválidos.

Vamos alterar a implementação para validar se o chassi possui um tamanho mínimo de 17 caracteres:

{% highlight java %}
public class ChassiUtil {
  public int obterAno(String chassi, int posicaoAno) {
    if(chassi == null || chassi.trim().length() < 17) {
      throw new IllegalArgumentException("O chassi informado é inválido!");
    }

    /* Como os caracteres começam na posição zero, é necessário
     subtrair 1 da posição do ano informada. */
    char caractere = chassi.toUpperCase().charAt(posicaoAno - 1);
    return (caractere - 'A') + 2010;
  }
}
{% endhighlight %}

Agora que validamos se o chassi informado é nulo ou possui um tamanho menor que 17 caracteres será lançado uma exceção `IllegalArgumentException`, para informar que o chassi é inválido. Vamos executar novamente os testes:

{% highlight java %}
JUnit version 4.12
.....
Time: 0,006

OK (5 tests)
{% endhighlight %}

Chegamos ao final do exemplo, cobrimos diversos testes para tentar garantir que o método `obterAno` retorne o ano correto baseado no código e posição do ano no chassi, pode ser que exista mais testes para serem realizados, mas para a solicitação inicial acredito que a quantidade de testes que fizemos cobrirá grande parte dos problemas que poderiam ocorrer.

Agora é hora da mão na massa, elaborei alguns exercícios para te ajudar a pensar antes de sair escrevendo o código, aprender um pouco de lógica e fixar os conceitos de testes.

## Exercícios

**Exercício 01)** Crie uma aplicação para criptografar e descriptografar usando a Cifra de César. Esta é uma forma simples de criptografia, que consistem em mover as letras do alfabeto. Dado um número n, vamos mover as letras no alfabeto, por exemplo:

Dado n = 3 e o texto “rafael guimaraes sakurai”, vamos aumentar em 3 cada caractere, assim a versão criptografada ficará “udidho jzlpdudhv vdnzudl”, e para descriptografar basta diminuir em 3 cada caractere.

Dica: todo caractere é representado por um número, sendo assim é possível somar uma quantidade que se deseja aumentar em cada caractere, exemplo:

{% highlight java %}
char letra = 'd';
/* é necessário fazer a conversão para char, porque a
soma da letra com o numero 3, retorna um valor do tipo
int. */
char novaletra = (char) (letra + 3);
System.out.println(letra + ((int) letra));
System.out.println(novaletra + ((int) novaletra));
{% endhighlight %}

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar para o usuário uma frase e mostrar essa frase criptografada.

**Exercício 02)** Crie uma aplicação para converter número decimal para romano. Para restringir a quantidade de opções, converta apenas os números decimais de 1 a 10, como apresentado na tabela:

Sendo:

| Decimal | Romano |
| -- | -- |
| 1 | I |
| 2 | II |
| 3 | III |
| 4 | IV ou IIII |
| 5 | V |
| 6 | VI |
| 7 | VII |
| 8 | VIII |
| 9 | IV ou VIIII |
| 10 | X |

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar para o usuário o número em decimal e imprima o número convertido para romano.

**Exercício 03)** Crie uma aplicação para calcular quantos dígitos possui um número inteiro. Por exemplo: o número **13** possui **2 dígitos**, o número **9346** possui **4 dígitos**, o número **19000283** possui **8 dígitos**.

> Observação: O calculo deve ocorrer independente da quantidade de dígitos do números.

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar ao usuário um número e imprima a quantidade de dígitos que esse número possui.

**Exercício 04)** Crie uma aplicação para inverter números, por exemplo: dado o número **3208** deve ser impresso o número **8023**.

> Observação: A conversão deve ocorrer independente da quantidade de dígitos do números.

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar ao usuário um número e imprima o número invertido.

**Exercício 05)** Crie uma aplicação para calcular a média das notas de um aluno, sendo que por semestre o aluno fará 4 avaliações. Por exemplo dado as notas 5.5, 7.4, 8.2, 7.1, teremos a média 7.05.

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para solicitar ao usuário as quatro notas das avaliações e imprima o valor da média.

Dica: a comparação de números com casas decimais use o método assertEquals passando um terceiro parâmetro para cobrir erros de arredondamento como:

{% highlight java %}
@Test
public void testarDecimal() {
    assertEquals(1.2, conta.multiplicar(0.3, 4), 0.01);
}
{% endhighlight %}

Que possui um erro de arredondamento de **0.01**, ou se preferir tente usar a classe **java.math.BigDecimal** que serve exatamente para trabalhar com números com casas decimais.


### Conteúdos relacionados

- [Introdução ao TDD](http://www.universidadejava.com.br/tdd/tdd-introducao/)
- [Começando a criar testes passo a passo](http://www.universidadejava.com.br/tdd/tdd-ola-testes/)
- [Entendendo o ciclo de desenvolvimento com TDD](http://www.universidadejava.com.br/tdd/tdd-ciclo-desenvolvimento/)
- [O impacto do TDD no design da aplicação](http://www.universidadejava.com.br/tdd/tdd-design-aplicacao/)