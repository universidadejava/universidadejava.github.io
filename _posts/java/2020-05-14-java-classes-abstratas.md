---
layout: article
title: "Java - Classes Abstratas"
categories: java
author: sakurai
date: 2020-05-14 06:25:00
tags: [java, classe abstrata, herança, abstract]
published: true
excerpt: As classes abstratas é um tipo especial de classe que não pode ser instanciada.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Classes Abstratas

Em Java, temos um tipo especial de classe chamado **classe abstrata**. Este tipo de classe possui uma característica muito específica, que é o de não permitir que novos objetos sejam instanciados a partir desta classe. Por este motivo, as classes abstratas possuem o único propósito de servirem como super classes a outras classes do Java.

Em Java definimos uma classe como abstrata utilizando a palavra-chave **abstract** na declaração da classe, por exemplo:

{% highlight java %}
package material.abstrata;

/**
 * Classe Abstrata representando uma Pessoa.
 */
public abstract class Pessoa {
  private String nome;

  public Pessoa(String nome) {
    this.nome = nome;
  }

  public String getNome() {
    return nome;
  }
  public void setNome(String nome) {
    this.nome = nome;
  }
}
{% endhighlight %}

Na linha 6 estamos criando uma classe abstrata através da palavra-chave abstract.

Uma classe abstrata é desenvolvida para representar entidades e conceitos abstratos, sendo utilizada como uma classe pai, pois não pode ser instanciada. Ela define um modelo (template) para uma funcionalidade e fornece uma implementação incompleta - a parte genérica dessa funcionalidade - que é compartilhada por um grupo de classes derivadas. Cada uma das classes derivadas completa a funcionalidade da classe abstrata adicionando um comportamento específico.

Uma classe abstrata normalmente possui métodos abstratos. Esses métodos são implementados nas suas classes derivadas concretas com o objetivo de definir o comportamento específico. O método abstrato define apenas a assinatura do método e, portanto, não contém código assim como feito nas Interfaces.

Uma classe abstrata pode também possuir atributos e métodos implementados, componentes estes que estarão integralmente acessíveis nas subclasses, a menos que o mesmo seja do tipo **private**.

Como todo método abstrato precisa ser implementado pela classe filha, então não pode ser private, pois não seria visível na subclasse.

Neste exemplo, criaremos uma classe abstrata chamada **Tela**. Nesta classe implementaremos alguns métodos que devem ser utilizados por todas as telas do computador de bordo de um veículo, como por exemplo, **setTitulo()** para informar o título da tela e **imprimir()**, que imprime as informações na tela.

Nesta classe definiremos também a assinatura de um método abstrato chamado **obterInformacao()**, que deve ser implementado pelas classes filhas.

{% highlight java %}
package material.abstrata;

/**
 * Classe abstrata que possui os métodos básicos para
 * as telas do computador de bordo de um veiculo.
 */
public abstract class Tela {
  private String titulo;

  public void setTitulo(String titulo) {
    this.titulo = titulo;
  }

  public abstract String obterInformacao();

  public void imprimir() {
    System.out.println(this.titulo);
    System.out.println("\t" + obterInformacao());
  }
}
{% endhighlight %}

Agora criaremos a classe **TelaKilometragem**, que será uma subclasse da classe abstrata Tela. Esta classe será utilizada para mostrar a km atual percorrida pelo veículo.

{% highlight java %}
package material.abstrata;

/**
 * Tela que mostra a kilometragem percorrida por um veiculo.
 */
public class TelaKilometragem extends Tela {
  /* Atributo que guarda o valor da km atual do veiculo. */
  int km = 0;

  /**
   * Construtor que iniciliza o titulo da tela.
   */
  public TelaKilometragem() {
    /* Atribui o valor do titulo desta tela. */
    super.setTitulo("Km Atual");
  }

  /**
   * Implementa o método abstrato da classe Tela,
   * neste método buscamos a km atual do veiculo.
   * 
   * @return Texto com a km atual.
   */
  @Override
  public String obterInformacao() {
    km += 10;
    return String.valueOf(km) + " km";
  }
}
{% endhighlight %}

Vamos, agora, criar uma classe para testar a nossa aplicação. Neste teste, criaremos um objeto da classe TelaKilometragem e chamar o método imprimir(), que esta classe herdou da classe Tela. 

{% highlight java %}
package material.abstrata;

/**
 * Classe utilizada para testar a Tela Kilometragem.
 */
public class TesteTelaKm {
  public static void main(String[] args) {
    TelaKilometragem tk = new TelaKilometragem();
    tk.imprimir();

    System.out.println("\n------------\n");

    tk.imprimir();
  }
}
{% endhighlight %}

Quando definimos **Tela tk = new TelaKilometragem()**, estamos guardando um objeto do tipo TelaKilometragem dentro de uma variável do tipo Tela. Podemos fazer isso porque toda TelaKilometragem é uma subclasse de Tela.

Quando chamamos o método **tk.imprimir()**, estamos chamando o método que foi implementado na classe abstrata Tela, mas note que o método **imprimir()** usa o método **obterInformacao()**, que foi implementado na classe **TelaKilometragem**. Em tempo de execução, a JVM verifica o tipo do objeto e chama  o método que foi implementado nele, isto chama-se **Polimorfismo**.

Temos a seguinte saída no console:

{% highlight java %}
Km Atual
	10 km

----------

Km Atual
	20 km
{% endhighlight %}

Segue abaixo um quadro comparativo entre **Interface** e **Classe Abstrata**:

| Classe Abstrata | Interface |
| Pode possui atributos de instância | Possui apenas constantes |
| Possui métodos de diversas visibilidade e métodos implementados ou abstratos | Todos os métodos são public e abstract |
| É estendida por classes (sub-classes) | É implementada por classes |
| Uma subclasse só pode estender uma única classe abstrata | Uma classe pode implementar mais de uma interface |
| Hierarquia de herança com outras classes abstratas | Hierarquia de herança com outras  interfaces| 