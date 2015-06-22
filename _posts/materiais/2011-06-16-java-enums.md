---
layout: article
title: "Java - Enums"
categories: materiais
author: sakurai
date: 2011-06-16 22:35:00
tags: [java]
published: true
excerpt: As Enums surgiram na linguagem Java a partir da versão 5 como uma alternativa ao uso de constantes, e para atender de maneira melhor algumas das situações específicas que podem ocorrer durante a programação.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

As Enums surgiram na linguagem Java a partir da versão 5 como uma alternativa ao uso de constantes, e para atender de maneira melhor algumas das situações específicas que podem ocorrer durante a programação.

## Justificativas do uso de Enums a constantes

Abaixo temos algumas justificativas do porque de se utilizar Enums em Java em relação às constantes:

**1) As constantes não oferecem segurança** – Como normalmente constantes são variáveis de um tipo específico, caso sua constante sirva para ser utilizada em algum método, qualquer classe pode passar um valor de mesmo tipo da sua constante, mas que na verdade não existe.

Por exemplo, veja a seguinte classe:

{% gist 484fdcdc3fe2ae8a525d ExemploUsarEnums.java %}

Caso outra classe invoque o método calcularAprovacao na linha 9 e passe o número 4 como parâmetro ele simplesmente não funcionará.

**2) Não há um domínio estabelecido** – Para evitar duplicidade entre os nomes de constantes nas diversas classes de um sistema, o desenvolvedor é forçado a padronizar um domínio. Perceba no exemplo acima que todas as constantes iniciam com o prefixo CONCEITO justamente com este fim.

**3) Fragilidade de modelo** - Como uma constante sempre requer um valor, caso você crie um novo valor intermediário aos já existentes, todos os atuais deverão ser alterados também. Pior exemplo, ainda considerando o exemplo acima, imagine que você precise criar um novo conceito chamado REGULAR. Qual seria seu valor uma vez que ele deveria estar entre os conceitos RUIM e BOM?  Provavelmente você teria de alterar todo o seu código para algo semelhante ao que segue abaixo:

{% gist 484fdcdc3fe2ae8a525d ExemploUsarEnums2.java %}

Note que ao invés de apenas criamos este novo valor, somos forçados a alterar os valores das variáveis BOM e OTIMO. Imagine isso em um contexto com duzentas ou trezentas variáveis, por exemplo.

**4) Seus valores impressos são pouco informativos** – Como constantes tradicionais de tipos primitivos são apenas valores numéricos em sua grande maioria, seu valor impresso pode não representar algo consistente. Por exemplo, o que o número 2 significa para você em seu sistema? Um conceito? Uma temperatura? Um modelo de carro? Etc.

## Como criar uma Enum

A estrutura de uma Enum é bem semelhante à de uma classe comum. Toda a Enum possui um construtor que pode ou não ser sobrecarregado. Vamos ao primeiro exemplo:

{% gist 484fdcdc3fe2ae8a525d ConceitosEnum.java %}

Observe que no exemplo acima quatro valores foram criados nas linhas 5 a 8, mas, diferentemente das constantes, eles não possuem um tipo específico sendo que todos eles são vistos como elementos da enumeração Conceitos. Isto já torna desnecessária a denominação de um prefixo de domínio para os nomes dos conceitos, visto que a própria enum já cumpre este papel.

Ainda sobre o exemplo acima, também vale ressaltar que uma enum pode ter métodos. O método calcularAprovacao na linha 10 recebe como parâmetro agora não mais um número inteiro qualquer, mas sim uma enum do tipo ConceitosEnum. Desta forma garante-se que ele sempre receberá um valor conhecido e que não teremos os mesmos problemas que poderiam ocorrer com uma variável de um tipo específico, tal como ocorria com as constantes.

Por fim, observe que como os elementos da enum não possuem mais um valor atrelado a si, a criação de um novo conceito, tal como um EXCELENTE ou PÉSSIMO em nada influenciaria no código já existente dentro da enum.

Uma enum, diferentemente de uma classe, já é inicializada quando você inicia sua aplicação Java, não necessitando ser instanciada. Vamos agora a um exemplo de uma enum com método construtor e com atributos:

{% gist 484fdcdc3fe2ae8a525d ConceitosEnumComConstrutor.java %}

Perceba que agora as mensagens estão sendo utilizadas no construtor e serão preenchidas no atributo mensagem para cada um dos elementos da enum. Desta forma, outra classe poderia acessar estes dados de maneira muito mais transparente, conforme o segue abaixo:

{% gist 484fdcdc3fe2ae8a525d PrincipalTesteEnum.java %}

Observe na linha 7 como a enum está sendo invocada. Não precisa criar uma instancia para usá-la.

{% highlight java %}
C:\>javac TestePrincipalEnum.java
C:\>java TestePrincipalEnum
Conceito..: Aprovado com louvor!
{% endhighlight %}
