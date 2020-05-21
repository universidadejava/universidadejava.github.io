---
layout: article
title: "Collection"
categories: java
author: sakurai
date: 2020-05-16 18:43:00
tags: [java, collection, list, set, queue, map]
published: true
excerpt: Uma coleção (collection) é um objeto que serve para agrupar muitos elementos em uma única unidade.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Collection

Uma coleção (collection) é um objeto que serve para agrupar muitos elementos em uma única unidade, estes elementos precisão ser Objetos, pode ter coleções homogêneas e heterogêneas, normalmente utilizamos coleções heterogêneas de um tipo especifico.

O núcleo principal das coleções é formado pelas interfaces da figura a abaixo, essas interfaces permitem manipular a coleção independente do nível de detalhe que elas representam.

<figure>
    <a href="/images/2020-05-16-java-collection-01.png"><img src="/images/2020-05-16-java-collection-01.png" alt="Algumas interfaces e classes que formam as collections."></a>
</figure>

Temos quatro grandes tipos de coleções: `Set` (conjunto), `List` (lista), `Queue` (fila) e `Map` (mapa), a partir dessas interfaces, temos muitas subclasses concretas que implementam varias formas diferentes de se trabalhar com cada coleção.

Todas as interfaces e classes são encontradas dentro do pacote (package) `java.util`, embora a interface `Map` não ser filha direta da interface `Collection` ela também é considerada uma coleção devido a sua função.


### java.util.Collection

A interface `Collection` define diversos métodos que são implementados pelas classes que representam coleções, dentro das coleções são adicionados Objetos também chamados de elementos.
	
Alguns dos métodos que devem ser implementados por todas as subclasses de `Collection`:

- `add(Object e)` – Adiciona um Objeto dentro da coleção.
- `addAll(Collection c)` – Adiciona uma coleção de Objetos dentro da coleção.
- `contains(Object o)` – Verifica se um Objeto está dentro da coleção.
- `clear()` - Remove todos os Objetos da coleção. 	
- `isEmpty()` - Retorna um `boolean` informando se a coleção está vazia ou não.
- `remove(Object o)` – Remove um Objeto da coleção.
- `size()` - Retorna o tamanho da coleção.
- `toArray()` - Converte uma coleção em um vetor.

A imagem a seguir mostra em azul as principais filhas da classe Collection, com exceção da interface Map, também mostra em verde as classes concretas mais utilizadas que implementam as interfaces.

<figure>
    <a href="/images/2020-05-16-java-collection-02.png"><img src="/images/2020-05-16-java-collection-02.png" alt="Interfaces e classes filhas de Collection e Map."></a>
</figure>

### java.util.Set

A interface `Set` é uma coleção do tipo conjunto de elementos. As características principais deste tipo de coleção são: os elementos não possuem uma ordem de inserção e não é possível ter dois objetos iguais.


### java.util.Queue

A interface `Queue` é uma coleção do tipo fila. As principais características deste tipo de coleção são: a ordem que os elementos entram na fila é a mesma ordem que os elementos saem da fila (FIFO - First In First Out), podemos também criar filas com prioridades.


### java.util.Map

A interface `Map` é uma coleção do tipo mapa. As principais características deste tipo de coleção são: os objetos são armazenados na forma de chave / valor, não pode haver chaves duplicadas dentro do mapa.

Para localizar um objeto dentro do mapa é necessário ter sua chave ou percorra o mapa por completo.


### java.util.List

A interface `List` é uma coleção do tipo lista, em que a ordem dos elementos é dado através de sua inserção dentro da lista.

{% highlight java %}
import java.util.ArrayList;
import java.util.List;

/**
 * Classe utilizada para demonstrar o uso da estrutura
 * de dados Lista.
 */
public class ExemploLista {
  public static void main(String[] args) {
    List nomes = new ArrayList();
    nomes.add("Zezinho");
    nomes.add("Luizinho");
    nomes.add("Joãozinho");

    for(int cont = 0; cont < nomes.size(); cont++) {
      String nome = (String) nomes.get(cont);
      System.out.println(nome);
    }
  }
}
{% endhighlight %}

Neste exemplo irá imprimir Zezinho, Luizinho e Joãozinho, pois é a ordem que os elementos foram adicionados na lista.

Outra forma de percorrer os elementos de uma lista é através da interface `java.util.Iterator`.

{% highlight java %}
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

/**
 * Classe utilizada para demonstrar o uso da estrutura
 * de dados Lista.
 */
public class ExemploLista2 {
  public static void main(String[] args) {
    List nomes = new ArrayList();
    nomes.add("Zezinho");
    nomes.add("Luizinho");
    nomes.add("Joãozinho");

    for(Iterator it = nomes.iterator(); it.hasNext();) {
      String nome = (String) it.next();
      System.out.println(nome);
    }
  }
}
{% endhighlight %}


### Um pouco sobre Generics

Usualmente, não há uso interessante uma lista com vários tipos de objetos misturados. Por este motivo, podemos utilizar uma nova estrutura criada a partir do Java 5.0 chamada de **Generics**.

A estrutura de Generics serve para restringir as listas a um determinado tipo de objetos (e não qualquer `Object`), assim como segue o exemplo:

{% highlight java %}
import java.util.ArrayList;
import java.util.List;

/**
 * Classe utilizada para demonstrar o uso da estrutura
 * de dados Lista com Generics.
 */
public class ListaPessoa {
  public static void main(String[] args) {
    List<Pessoa> pessoas = new ArrayList<Pessoa>();
    pessoas.add(new Pessoa("Rafael"));
    pessoas.add(new Pessoa("Cristiano"));

    /* Observe que o casting não é necessário, pois o compilador
       já sabe que dentro da lista só tem objetos do tipo Pessoa. */
    Pessoa p = pessoas.get(0);

    /* Podemos utilizar o for-each para percorrer os elementos da lista. */
    for(Pessoa pessoa : pessoas) {
      System.out.println(pessoa.getNome());
    }
  }
}
{% endhighlight %}

Repare no uso de um parâmetro `< ... >` ao lado do `List` e `ArrayList`. Este parâmetro indica que nossa lista foi criada para trabalhar exclusivamente com objetos de um tipo específico, como em nosso caso a classe `Pessoa`. A utilização deste recurso nos traz uma segurança maior em tempo de compilação de código, pois temos certeza que apenas terá objetos do tipo `Pessoa` dentro da lista.


### Conteúdos relacionados

- [Entenda o funcionamento do polimorfismo](http://www.universidadejava.com.br/java/java-polimorfismo/)
- [Conexão com banco de dados usando JDBC](http://www.universidadejava.com.br/java/java-jdbc/)
- [Leitura e escrita de arquivos em Java](http://www.universidadejava.com.br/java/java-leitura-arquivo/)
- [Tratamento de exceções no Java](http://www.universidadejava.com.br/java/java-excecoes/)