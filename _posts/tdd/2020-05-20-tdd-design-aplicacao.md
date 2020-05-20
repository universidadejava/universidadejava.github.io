---
layout: article
title: "TDD - O impacto do TDD no design da aplicação"
categories: tdd
author: sakurai
date: 2020-05-20 12:20:00
tags: [java, tdd, testes]
published: true
excerpt: Durante o desenvolvimento guiado por testes (TDD) aprendemos a criar métodos e classes que possuem objetivos específicos, porque quando temos uma classe ou método com diversas responsabilidades os testes começam a ficar complexos ou muito difíceis de serem feitos.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## O impacto do TDD no design da aplicação

Durante o desenvolvimento guiado por testes (TDD) aprendemos a criar métodos e classes que possuem objetivos específicos, porque quando temos uma classe ou método com diversas responsabilidades os testes começam a ficar complexos ou muito difíceis de serem feitos.

## Exemplo rodízio de veículos

Para apresentar alguns exemplos de como os testes podem ajudar a criar um código melhor, vamos criar uma funcionalidade para validar se a placa de um veículo pode circular durante o rodízio de veículos, a tabela a seguir apresenta os dias e o finais das placas que não podem circular:

| Dia | Final Placa |
| -- | -- |
| Segunda-feira | 1 e 2 |
| Terça-feira | 3 e 4 |
| Quarta-feira | 5 e 6 |
| Quinta-feira | 7 e 8 |
| Sexta-feira | 9 e 0 |

Caso você prefira, gravei um vídeo com o exemplo de rodízio apresentado nesse post:

<iframe width="560" height="315" src="https://www.youtube.com/embed/7Nag2FOpe-c" frameborder="0" allowfullscreen></iframe>

Então como começamos a implementar o código? Criar uma classe valida as placas? Criar uma classe para representar os dias da semana? Nada disso, precisamos começar pensando o que precisamos testar e criar o primeiro teste que falhe.

Inicialmente vamos ter os seguintes testes:

* Testar se as placas de final 1 e 2 não podem circular na segunda, mas podem circular na terça, quarta, quinta e sexta.
* Testar se as placas de final 3 e 4 não podem circular na terça, mas podem circular na segunda, quarta, quinta e sexta.
* Testar se as placas de final 5 e 6 não podem circular na quarta, mas podem circular na segunda, terça, quinta e sexta.
* Testar se as placas de final 7 e 8 não podem circular na quinta, mas podem circular na segunda, terça, quarta e sexta.
* Testar se as placas de final 9 e 0 não podem circular na sexta, mas podem circular na segunda, terça, quarta e quinta.

Vamos começar criando a classe de teste para testar se as placas de final 1 ou 2 não podem circular na segunda, mas podem circular na terça, quarta, quinta e sexta.

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class PlacaUtilTest {
  @Test
  public void testarPlacaFinal1e2() {
    String placa = "AAA-1111";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));

    placa = "AAA-1112";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
  }
}
{% endhighlight %}

> Talvez você tenha pensado, mas porque ele fez duas verificações sabendo que as práticas pedem para criar um teste que falha e depois implementar essa verificação? Resposta: Acredito que depois de um tempo de prática com o desenvolvimento de testes, começamos a ganhar um pouco de confiança para pular pequenas fases, nesse caso me sinto confortável a criar duas verificações para o mesmo teste, pensando que quando for implementar já farei a implementação que funcione para placas com final 1 e 2.

Criamos a classe `PlacaUtilTest` para executar os testes usando o JUnit, nele criamos o método `testarPlacaFinal1e2()` que **verifica se a placa com final 1 ou 2 não pode circular na segunda-feira**.

Ao compilar o teste, teremos a seguinte saída:

{% highlight java %}
javac -cp junit-4.12.jar PlacaUtilTest.java
{% endhighlight %}

{% highlight java %}
PlacaUtilTest.java:8: error: cannot find symbol
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
                ^
  symbol:   variable PlacaUtil
  location: class PlacaUtilTest
PlacaUtilTest.java:11: error: cannot find symbol
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
                ^
  symbol:   variable PlacaUtil
  location: class PlacaUtilTest
2 errors
{% endhighlight %}

Informando que não existe a classe `PlacaUtil`. Então, precisamos criar o mínimo necessário para fazer este teste funcionar:

{% highlight java %}
public class PlacaUtil {
  public static boolean podeCircularNoRodizio(String placa,
    String diaSemana) {
    return false;
  }
}
{% endhighlight %}

Agora, se compilarmos e testarmos novamente, vamos ter um teste funcionando:

{% highlight java %}
javac -cp junit-4.12.jar PlacaUtilTest.java PlacaUtil.java
{% endhighlight %}

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore PlacaUtilTest
{% endhighlight %}

{% highlight java %}
JUnit version 4.12
.
Time: 0,007

OK (1 test)
{% endhighlight %}

Agora que o teste está funcionando, precisamos refatorar porque a implementação está funcionando, mas não esta implementado corretamente, pois podemos utilizar os parâmetros recebidos para verificar se a placa pode circular no dia da semana.

{% highlight java %}
public class PlacaUtil {
  public static boolean podeCircularNoRodizio(String placa, String diaSemana) {
    if ("Segunda".equals(diaSemana)) {
      return !(placa.endsWith("1") || placa.endsWith("2"));
    }

    return true;
  }
}
{% endhighlight %}

> A classe `String` possui o método `endsWith()` que informa se a String termina com uma determinada `String`, nesse caso estamos verificando se o **final da placa termina com a String `"1"` ou `"2"`**.

Agora validamos se o dia da semana for `"Segunda"` a placa não pode terminar com final `1` ou `2`. Vamos executar o teste novamente para verificar se ainda está funcionando:

{% highlight java %}
JUnit version 4.12
.
Time: 0,006

OK (1 test)
{% endhighlight %}

Acredito que podemos melhorar nossos testes validando se a placa com final 1 ou 2 pode circular na Terça, Quarta, Quinta e Sexta:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class PlacaUtilTest {
  @Test
  public void testarPlacaFinal1e2() {
    String placa = "AAA-1111";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Terca"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quarta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quinta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Sexta"));

    placa = "AAA-1112";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Terca"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quarta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quinta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Sexta"));
  }
}
{% endhighlight %}

Vamos compilar e executar novamente o teste:

{% highlight java %}
javac -cp junit-4.12.jar PlacaUtilTest.java PlacaUtil.java
{% endhighlight %}

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore PlacaUtilTest
{% endhighlight %}

{% highlight java %}
JUnit version 4.12
.
Time: 0,006

OK (1 test)
{% endhighlight %}

Como os testes estão funcionando e fizemos uma implementação que baseado nos parâmetros consegue verificar se a placa com final 1 ou 2 não pode circular apenas na segunda, vamos continuar e vamos criar mais um teste para **verificar se a placa com final 3 ou 4 não pode circular na terça**.

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class PlacaUtilTest {
  @Test
  public void testarPlacaFinal1e2() {
    String placa = "AAA-1111";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Terca"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quarta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quinta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Sexta"));

    placa = "AAA-1112";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Terca"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quarta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quinta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Sexta"));
  }

  @Test
  public void testarPlacaFinal3e4() {
    String placa = "AAA-1113";
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Terca"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quarta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quinta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Sexta"));

    placa = "AAA-1114";
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Segunda"));
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, "Terca"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quarta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Quinta"));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, "Sexta"));
  }
}
{% endhighlight %}

Vamos compilar novamente para verificar se o teste irá funcionar:

{% highlight java %}
JUnit version 4.12
..E
Time: 0,008
There was 1 failure:
1) testarPlacaFinal3e4(PlacaUtilTest)
java.lang.AssertionError
  ...
  at PlacaUtilTest.testarPlacaFinal3e4(PlacaUtilTest.java:26)
  ...

FAILURES!!!
Tests run: 2,  Failures: 1
{% endhighlight %}

Opa! Esta faltando a implementação para verificar se a placa com final 3 ou 4 não pode circular na terça, então vamos implementá-lo, já fizemos o código para a Segunda, o código da terça é bem parecido:

{% highlight java %}
public class PlacaUtil {
  public static boolean podeCircularNoRodizio(String placa,
    String diaSemana) {

    if("Segunda".equals(diaSemana)) {
      return !(placa.endsWith("1") || placa.endsWith("2"));
    } else if("Terca".equals(diaSemana)) {
      return !(placa.endsWith("3") || placa.endsWith("4"));
    }

    return true;
  }
}
{% endhighlight %}

Então adicionamos mais um `if` para verificar se na `“Terca”` (estou usando o nome sem cedilha só para não ter problema de em algum lugar escrever com cedilha e em outro sem cedilha) as placas com final 3 ou 4 não podem circular, vamos compilar e testar novamente para ver o resultado apresentado:

{% highlight java %}
JUnit version 4.12
..
Time: 0,006

OK (2 tests)
{% endhighlight %}

Mais um teste funcionando, agora vamos para a refatoração, o que fizemos agora para funcionar foi aumentar a complexidade deste código, porque se continuar assim vamos ter uma sequência de pelo menos 5 ifs, olha o caos chegando. Como podemos refatorar este código para evitar esse monte de ifs?

E se criarmos uma interface chamada `Dia` que possui um método `podeCircular(String placa)` que verifica se uma determinada placa pode circular e uma classe que implementa essa interface Dia? Teríamos algo como a figura a seguir:

<figure>
    <a href="/images/2020-05-20-tdd-design-aplicacao-01.png"><img src="/images/2020-05-20-tdd-design-aplicacao-01.png" alt="UML da interface Dia e suas implementações."></a>
</figure>

A interface `Dia`, possui uma classe para cada dia da semana que tem rodízio e cada uma delas será responsável por verificar se nesse dia uma determinada placa pode circular.

Vamos criar a interface `Dia` e a classe `Segunda` que irá validar se a placa `podeCircular`. Opa! Calma ai, estamos esquecendo algo? Seguindo o TDD primeiro vamos testar se isso funciona, então a primeira coisa a se fazer é criar outra classe de teste.

> Observação: As vezes quando estamos refatorado e encontramos uma solução, queremos sair implementando o código, mas precisamos lembrar que estamos seguindo as boas práticas do TDD, por isso temos sempre que testar antes de começar a escrever alguma implementação.

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class DiaTest {
  @Test
  public void podeCircularSegundaFinal1e2() {
    Dia dia = new Segunda();
    assertFalse(dia.podeCircular("AAA-1111"));
    assertFalse(dia.podeCircular("AAA-1112"));
  }
}
{% endhighlight %}

Criamos a classe `DiaTest` que terá os métodos de teste dos dias, nele criamos o método `podeCircularSegundaFinal1e2()` para verificar se na `Segunda` não pode circular as placas de final 1 ou 2. Vamos executar o teste:

{% highlight java %}
javac -cp junit-4.12.jar DiaTest.java
{% endhighlight %}

{% highlight java %}
DiaTest.java:7: error: cannot find symbol
    Dia dia = new Segunda();
    ^
  symbol:   class Dia
  location: class DiaTest
DiaTest.java:7: error: cannot find symbol
    Dia dia = new Segunda();
                  ^
  symbol:   class Segunda
  location: class DiaTest
2 errors
{% endhighlight %}

Como ainda não criamos a interface `Dia` e a classe `Segunda` o código do teste nem compila, então agora vamos implementar o mínimo necessário para fazer este teste passar:

A interface `Dia` tem apenas a assinatura do método `podeCircular(String placa)`:

{% highlight java %}
public interface Dia {
  public boolean podeCircular(String placa);
}
{% endhighlight %}

E na classe `Segunda` implementamos a interface `Dia` e implementamos o método `podeCircular(String placa)`:

{% highlight java %}
public class Segunda implements Dia {
  @Override
  public boolean podeCircular(String placa) {
    return false;
  }
}
{% endhighlight %}

Vamos testar agora a classe `DiaTest` e verificar a saída do JUnit:

{% highlight java %}
javac -cp junit-4.12.jar DiaTest.java Dia.java Segunda.java
{% endhighlight %}

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore DiaTest
{% endhighlight %}

{% highlight java %}
JUnit version 4.12
.
Time: 0,006

OK (1 test)
{% endhighlight %}

Implementamos o mínimo necessário, agora vamos refatorar a classe `Segunda` para implementar se a placa recebida como parâmetro pode circular na segunda-feira:

{% highlight java %}
public class Segunda implements Dia {
  @Override
  public boolean podeCircular(String placa) {
    return !(placa.endsWith("1") || placa.endsWith("2"));
  }
}
{% endhighlight %}

Vamos executar novamente para verificar se o teste ainda está funcionando depois da refatoração:

{% highlight java %}
JUnit version 4.12
.
Time: 0,006

OK (1 test)
{% endhighlight %}

Agora precisamos testar os outros dias da semana, para não prolongar muito o texto vou criar agora todos os testes dos dias da semana (o correto é ir fazendo passo a passo cada teste), a classe `DiaTest` vai ficar com os seguintes testes:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class DiaTest {
  @Test
  public void podeCircularSegundaFinal1e2() {
    Dia dia = new Segunda();
    assertFalse(dia.podeCircular("AAA-1111"));
    assertFalse(dia.podeCircular("AAA-1112"));
  }

  @Test
  public void podeCircularTercaFinal3e4() {
    Dia dia = new Terca();
    assertFalse(dia.podeCircular("AAA-1113"));
    assertFalse(dia.podeCircular("AAA-1114"));
  }

  @Test
  public void podeCircularQuartaFinal5e6() {
    Dia dia = new Quarta();
    assertFalse(dia.podeCircular("AAA-1115"));
    assertFalse(dia.podeCircular("AAA-1116"));
  }

  @Test
  public void podeCircularQuintaFinal7e8() {
    Dia dia = new Quinta();
    assertFalse(dia.podeCircular("AAA-1117"));
    assertFalse(dia.podeCircular("AAA-1118"));
  }

  @Test
  public void podeCircularSextaFinal9e0() {
    Dia dia = new Sexta();
    assertFalse(dia.podeCircular("AAA-1119"));
    assertFalse(dia.podeCircular("AAA-1110"));
  }
}
{% endhighlight %}

Agora vamos implementar as classes. que cuidaram da verificação para cada dia da semana.

Na classe `Terca` verificamos que as placas com final 3 e 4 não podem circular:

{% highlight java %}
public class Terca implements Dia {
  @Override
  public boolean podeCircular(String placa) {
    return !(placa.endsWith("3") || placa.endsWith("4"));
  }
}
{% endhighlight %}

Na classe `Quarta` verificamos que as placas com final 5 e 6 não podem circular:

{% highlight java %}
public class Quarta implements Dia {
  @Override
  public boolean podeCircular(String placa) {
    return !(placa.endsWith("5") || placa.endsWith("6"));
  }
}
{% endhighlight %}

Na classe `Quinta` verificamos que as placas com final 7 e 8 não podem circular:

{% highlight java %}
public class Quinta implements Dia {
  @Override
  public boolean podeCircular(String placa) {
    return !(placa.endsWith("7") || placa.endsWith("8"));
  }
}
{% endhighlight %}

Na classe `Sexta` verificamos que as placas com final 9 e 0 não podem circular:

{% highlight java %}
public class Sexta implements Dia {
  @Override
  public boolean podeCircular(String placa) {
    return !(placa.endsWith("9") || placa.endsWith("0"));
  }
}
{% endhighlight %}

Agora vamos testar novamente:

{% highlight java %}
javac -cp junit-4.12.jar DiaTest.java Dia.java Segunda.java Terca.java Quarta.java Quinta.java Sexta.java
{% endhighlight %}

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore DiaTest
{% endhighlight %}

{% highlight java %}
JUnit version 4.12
.....
Time: 0,008

OK (5 tests)
{% endhighlight %}

Os testes estão funcionando para todos os dias. Agora queremos usar as classes que implementam o Dia para verificar as placas, então o vamos alterar o teste inicial:

{% highlight java %}
import org.junit.Test;
import static org.junit.Assert.*;

public class PlacaUtilTest {
  @Test
  public void testarPlacaFinal1e2() {
    String placa = "AAA-1111";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, new Segunda()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Terca()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quarta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quinta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Sexta()));

    placa = "AAA-1112";
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, new Segunda()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Terca()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quarta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quinta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Sexta()));
  }

  @Test
  public void testarPlacaFinal3e4() {
    String placa = "AAA-1113";
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Segunda()));
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, new Terca()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quarta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quinta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Sexta()));

    placa = "AAA-1114";
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Segunda()));
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, new Terca()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quarta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Quinta()));
    assertTrue(PlacaUtil.podeCircularNoRodizio(placa, new Sexta()));
  }
}
{% endhighlight %}

Outra vantagem que ganhamos ao utilizar as classes é que não precisamos ficar escrevendo os dias da semana como `String`, basta utilizar a referencia das classes, então vamos testá-lo:

{% highlight java %}
javac -cp junit-4.12.jar PlacaUtilTest.java PlacaUtil.java Segunda.java Terca.java
Quarta.java Quinta.java Sexta.java Dia.java
{% endhighlight %}

{% highlight java %}
PlacaUtilTest.java:8: error: incompatible types: Segunda cannot be converted to String
    assertFalse(PlacaUtil.podeCircularNoRodizio(placa, new Segunda()));
                                                       ^
    ...

Note: Some messages have been simplified; recompile with -Xdiags:verbose to get full output
20 errors
{% endhighlight %}

Os testes falharam, porque o método `podeCircularNoRodizio` não recebe um `Dia` como parâmetro ele está esperando receber uma `String`, se tivéssemos seguindo o raciocínio inicial teríamos esse monte de ifs aninhados:

{% highlight java %}
public class PlacaUtil {
  public static boolean podeCircularNoRodizio(String placa,
    String diaSemana) {

    if("Segunda".equals(diaSemana)) {
      return !(placa.endsWith("1") || placa.endsWith("2"));
    } else if("Terca".equals(diaSemana)) {
      return !(placa.endsWith("3") || placa.endsWith("4"));
    } else if("Quarta".equals(diaSemana)) {
      return !(placa.endsWith("5") || placa.endsWith("6"));
    } else if("Quinta".equals(diaSemana)) {
      return !(placa.endsWith("7") || placa.endsWith("8"));
    } else if("Sexta".equals(diaSemana)) {
      return !(placa.endsWith("9") || placa.endsWith("0"));
    }

    return true;
  }
}
{% endhighlight %}

Mas como criamos as classes para representar os dias da semana, podemos alterar este método para receber como parâmetro uma classe que implementa a interface `Dia` e deixar esta classe fazer a verificação da placa, então a classe `PlacaUtil` fica assim:

{% highlight java %}
public class PlacaUtil {
  public static boolean podeCircularNoRodizio(String placa, Dia dia){
    return dia.podeCircular(placa);
  }
}
{% endhighlight %}

Note que a orientação a objetos permitiu melhorar o código ele ficou mais limpo e menos complexo. Vamos executar novamente para verificar se os testes vão funcionar:

{% highlight java %}
javac -cp junit-4.12.jar PlacaUtilTest.java PlacaUtil.java Segunda.java Terca.java
Quarta.java Quinta.java Sexta.java Dia.java
{% endhighlight %}

{% highlight java %}
java -cp .;junit-4.12.jar;hamcrest-core-1.3.jar org.junit.runner.JUnitCore PlacaUtilTest
{% endhighlight %}

{% highlight java %}
JUnit version 4.12
..
Time: 0,007

OK (2 tests)
{% endhighlight %}

Implementei apenas os testes para as placas que não podem funcionar na segunda e na terça, agora termine e implemente para os demais dias da semana.

> Note como cada classe tem uma função bem especifica. Se for necessário adicionar um rodizio no sábado você apenas precisaria criar um teste para o sábado e uma classe `Sabado` que implementa `Dia`, não sendo mais necessário alterar a classe `PlacaUtil` para tratar essa verificação.


## Exercícios

**Exercício 01)** Crie uma aplicação para o Jogo da Velha, cada jogador deve informar a posição do tabuleiro que será sua jogada e o computador deve verificar se a jogada é possível e se algum jogador venceu o jogo.

* Crie uma lista com todos os testes que serão criados;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para jogar o jogo da velha com o usuário.

**Exercício 02)** Crie uma aplicação para validação de contratos, que recebe um arquivo no formato TXT, com as informações na seguinte ordem: número do contrato, nome do cliente, cpf, email, código do produto comprado, data de inicio do contrato, data de fim do contrato e vendedor. Ao final do processo deve informar todos os contratos que estão com algum problema de validação.

Cada linha deste arquivo possui a informação de um contrato, com as informações do contrato separada por ponto e virgula (;). É necessário fazer as seguintes validações:

* O número do contrato, deve ser um número inteiro e não pode ter mais que 10 dígitos;
* O nome do cliente não pode ter mais que 35 caracteres.
* O CPF deve ter 11 dígitos e deve ser válido;
* O email do cliente não pode ter mais que 150 caracteres e deve ter um caractere de arroba (@);
* O código do produto, deve ser um número inteiro e não pode ter mais que 10 dígitos;
* A data de inicio do contrato, deve estar no formato dd/MM/yyyy e não pode ser maior que a data de hoje;
* A data de fim do contrato, deve estar no formato dd/MM/yyyy e não pode ser anterior a data de inicio do contrato;
* O nome do vendedor não pode ter mais que 35 caracteres.

> Observações: Para verificar o formato da data utilize a classe java.text.DateFormat. A leitura do arquivo TXT pode ser feita usando a classe java.util.Scanner.

* Crie uma lista com todos os testes que serão criados;
* Crie um arquivo TXT com alguns exemplos de contratos;
* Crie a classe de teste e sua implementação seguindo as boas práticas do TDD;
* Crie uma classe principal para carregar o arquivo TXT dos contratos e apresente os contratos que estão com problemas.


### Conteúdos relacionados

- [Introdução ao TDD](http://www.universidadejava.com.br/tdd/tdd-introducao/)
- [Começando a criar testes passo a passo](http://www.universidadejava.com.br/tdd/tdd-ola-testes/)
- [Entendendo o ciclo de desenvolvimento com TDD](http://www.universidadejava.com.br/tdd/tdd-ciclo-desenvolvimento/)
- [Introdução a refatoração](http://www.universidadejava.com.br/tdd/tdd-refatoracao/)