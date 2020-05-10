---
layout: article
title: "Java - Pacotes"
categories: java
author: sakurai
date: 2020-05-09 23:56:00
tags: [java]
published: true
excerpt: Pacotes.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Pacotes

Na linguagem Java existe uma maneira simples e direta de se organizar arquivos, além de possibilitar que se utilize mais de uma classe de mesmo nome no mesmo projeto. Essa forma de organização recebe o nome de empacotamento. No Java utilizamos a palavra package para representar o pacote da classe.

A organização de pacotes ocorre de forma semelhante a uma biblioteca de dados. Vários arquivos são agrupados em pastas comuns, logo se tem por hábito agrupar arquivos de funções semelhantes em mesmas pastas.

<figure>
    <a href="/images/2020-05-09-java-pacotes-01.png"><img src="/images/2020-05-09-java-pacotes-01.png" alt="Organização em pacotes."></a>
</figure>

A árvore hierárquica do pacote não possui regras restritas de limitação de quantidade de nós, apenas recomenda-se notação semelhante a da Internet, a fim de reduzir ao máximo a possibilidade de termos mais de um pacote de arquivos de mesmo nome.

Observação: uma boa pratica para nomes de pacotes é utilizar apenas uma palavra como nome do pacote, por exemplo, biblioteca, pessoa, email, e o nome do pacote escrito em minúsculo. O pacote não pode ter o nome de uma palavra-chave do java e não pode começar com número ou caracteres especiais (exceto $ e _).

No exemplo a seguir vamos criar a estrutura de pacotes **br.biblioteca.telas** e dentro deste pacote vamos colocar a classe **CadastroLivro**:

<figure>
    <a href="/images/2020-05-09-java-pacotes-02.png"><img src="/images/2020-05-09-java-pacotes-02.png" alt="Organização em pacotes."></a>
</figure>

Criada então essa hierarquia, o próximo passo é declarar no arquivo da classe o nome do pacote ao qual ela pertence. Esta declaração deve ser feita obrigatoriamente na primeira linha do arquivo (sem contar comentários) da seguinte maneira:

{% highlight java %}
package br.biblioteca.telas;

/**
 * Tela utilizada para cadastrar livros da biblioteca.
 */
public class CadastroLivro {
}
{% endhighlight %}

Note na declaração acima que as pastas foram divididas por pontos. Logo a mesma hierarquia de pastas deve ser seguida nesta declaração.

Desta forma, os arquivos ficam realmente organizados e, de certa forma, isolados de outros pacotes. Vale lembrar que, quando criamos uma classe na raiz de nosso projeto esta classe não possui pacote.

O pacote de desenvolvimento Java já traz consigo muitos pacotes de funções especificas, sendo alguns eles:

| Pacote    | Descrição                   |
| --------- | --------------------------- |
| java.lang	| Pacote usual da linguagem   |
| java.util	| Utilitários da linguagem    |
| java.io   | Pacote de entrada e saída   |
| java.awt  | Pacote de interface gráfica |
| java.net  | Pacote de rede              |
| java.sql  | Pacote de banco de dados    |

Quando se está desenvolvendo uma classe e deseja-se utilizar uma classe de outro pacote, somos obrigados a declarar esta utilização. Para tal, utilizamos a palavra-chave **import**.

De forma semelhante a palavra-chave **package**, a palavra-chave **import** deve considerar a hierarquia de pastas e separá-las por pontos, da seguinte forma:

{% highlight java %}
package br.biblioteca.telas;

import br.biblioteca.modelo.Livro;
import br.biblioteca.util.MostrarTela;
import br.biblioteca.util.XmlConvert;

/**
 * Tela utilizada para cadastrar livros da biblioteca.
 */
public class CadastroLivro {
}
{% endhighlight %}

No exemplo anterior, demonstramos que a classe **CadastroLivro** irá utilizar a classe "Livro" do pacote "br.biblioteca.model".

Quando queremos utilizar muitas classes de um mesmo pacote, podemos importar o pacote inteiro utilizando o caractere asterisco ( * ) no lugar do nome das classes.

{% highlight java %}
package br.biblioteca.telas;

import br.biblioteca.modelo.*;

/**
 * Tela utilizada para cadastrar livros da biblioteca.
 */
public class CadastroLivro {
    Livro novoLivro = new Livro();
}
{% endhighlight %}

Neste exemplo é criado um novo objeto a partir da classe **Livro**, que está dentro do pacote **br.biblioteca.model**, como utilizamos o * no **import**, podemos utilizar qualquer classe do pacote **model**.

OBS: Durante o processo de compilação do arquivo **.java** para **.class**, o compilador primeiro importa as classes que estão com o pacote completo e depois procura pelas classes que tiveram o pacote inteiro importado, então normalmente é utilizado o nome inteiro do pacote e da classe, assim as classes são compiladas um pouco mais rápido.

