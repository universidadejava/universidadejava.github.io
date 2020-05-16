---
layout: article
title: "Java - Leitura de arquivos"
categories: java
author: sakurai
date: 2020-05-14 22:46:00
tags: [java, file, io, read, write]
published: true
excerpt: Existem diversos meios de se manipular arquivos na linguagem de programação Java.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Leitura de arquivos

Existem diversos meios de se manipular arquivos na linguagem de programação Java. A classe **java.io.File** é responsável por representar arquivos ou diretórios do seu sistema de arquivos. Esta classe pode fornecer informações úteis assim como criar um novo arquivo, tamanho do arquivo, caminho absoluto, espaço livre em disco ou, ainda, excluí-lo.

A linguagem Java oferece uma classe específica para a leitura do fluxo de bytes que compõem o arquivo, esta classe chama-se **java.io.FileInputStream**. A FileInputStream recebe em seu construtor uma referencia File, ou uma String que deve representar o caminho completo do arquivo, dessa forma podemos ler as informações que estão dentro do arquivo.

Tanto a invocação do arquivo pela classe File, quanto diretamente pela FileInputStream fará com que o arquivo seja buscado no diretório em que o Java foi invocado (no caso do NetBeans vai ser dentro do diretório do projeto). Você também pode usar um caminho absoluto, para ler os arquivos que estão em outros diretórios do seu computador (ex: C:\arquivos\arquivo.txt).

### Exemplo de leitura de arquivo 

Exemplo de um programa que a partir de um caminho, verifica se este caminho é referente a um diretório ou um arquivo.

{% highlight java %}
package material.arquivo;

import java.io.File;

/**
 * Classe utilizada para demonstrar o uso da classe File.
 */
public class ExemploFile {
  public static void main(String[] args) {
    ExemploFile ef = new ExemploFile();
    ef.verificarCaminho("C:\\Arquivos");
    ef.verificarCaminho("C:\\Arquivos\\teste.txt");
  }

  public void verificarCaminho(String caminho) {
    File f = new File(caminho);

    System.out.println(caminho);
    if(f.isFile()) {
      System.out.println("Este caminho eh de um arquivo.");
    } else if(f.isDirectory()) {
      System.out.println("Este caminho eh de um diretorio.");
    }
  }
}
{% endhighlight %}

Criamos um objeto f do tipo File a partir de um caminho recebido, este objeto será utilizado para representar um arquivo ou diretório.

Verificamos se o objeto f é um arquivo, através do método isFile() da classe File, caso retorne true, então irá imprimir a mensagem informando que o caminho é referente a um arquivo.

Caso contrário, verificamos se o objeto f é um diretório, através do método isDirectory() da classe File, caso retorne true, então irá imprimir a mensagem informando que o caminho é referente a um diretório.

Ao executar a classe ExemploFile, teremos a seguinte saída no console:

{% highlight java %}
C:\>javac material\arquivo\ExemploFile.java
C:\>java material.arquivo.ExemploFile
C:\Arquivos
Este caminho eh de um diretorio
C:\Arquivos\teste.txt
Este caminho eh de um arquivo.
{% endhighlight %}

Neste exemplo, vamos ler um arquivo .txt e imprimir seu texto no console.

{% highlight java %}
package material.arquivo;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/**
 * Classe utilizada para ler as informações de um arquivo e imprimir
 * no console.
 */
public class ExemploFileInputStream {
  public static void main(String[] args) {
    ExemploFileInputStream exemplo = new ExemploFileInputStream();
    exemplo.lerArquivo("C:\\Arquivos\\teste.txt");
  }

  /**
   * Método utilizado para ler os dados de uma arquivo.
   *
   * @param caminho
   */
  public void lerArquivo(String caminho) {
    FileInputStream fis = null;

    try {
      /* Cria um objeto do tipo FileInputStream para ler o arquivo. */
      fis = new FileInputStream(caminho);
      /* Cria uma variavel interia para ler os caracteres do arquivo,
        lembrando que todo caractere no Java é representado por um
        numero inteiro. */
      int c;
      /* Le o caracter do arquivo e guarda na variavel inteira c, quando
        o caracter lido for igual a -1, significa que chegou ao final
        do arquivo. */
      while((c = fis.read()) != -1) {
        System.out.println((char) c);
      }
    } catch (FileNotFoundException ex) {
      System.out.println("Erro ao abrir o arquivo.");
    } catch (IOException ex) {
      System.out.println("Erro ao ler o arquivo.");
    } finally {
      try {
        if(fis != null) {
          /* Fecha o arquivo que está sendo lido. */
          fis.close();
        }
      } catch (IOException ex) {
        System.out.println("Erro ao fechar o arquivo.");
      }
    }
  }
}
{% endhighlight %}

Criamos um objeto do tipo **FileInputStream**, seu construtor recebe uma String com um caminho de arquivo, esta classe lê os bytes do arquivo.

Para percorrer os bytes do arquivo, vamos declarar uma variável **int c** que será utilizada para guardar os caracteres lidos do arquivo, lembre-se que todos os caracteres em Java são representados por um número inteiro.

Percorremos todo o arquivo e fazer o casting de inteiro para caractere, para imprimir o texto do arquivo, note que se o valor lido através do método **read()** for igual a **-1**, significa que chegamos ao final do arquivo.

Também precisamos tratar as exceções que podem ser lançadas durante a leitura de um arquivo.

Sempre que estiver manipulando um arquivo, lembre-se de fechar o arquivo utilizando o método **close()**.

Ao executar a classe **ExemploFileInputStream**, teremos a seguinte saída no console:

{% highlight java %}
C:\>javac material\arquivo\ExemploFileInputStream.java
C:\>java material.arquivo.ExemploFileInputStream
t
e
s
t
e
{% endhighlight %}

### Streams de caracteres

Streams de caracteres é a forma como trabalhamos com os próprios caracteres lidos do arquivo, e a partir desses caracteres podemos adicionar filtros em cima deles. Exemplos destes leitores são as classes abstratas **java.io.Reader** e **java.io.Writer**.

#### java.io.Reader

A classe abstrata **java.io.Reader** representa um fluxo de entrada de dados tratados como caracteres. Essa classe possui várias subclasses dedicadas a cada fonte de dados específica, tais como a classe **java.io.FileReader**, que representa a leitura de caracteres de um arquivo

Neste exemplo, leremos um arquivo e imprimiremos seus caracteres no console:

{% highlight java %}
package material.arquivo;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;
import java.util.Scanner;

/**
 * Classe utilizada para demonstrar a leitura de caracteres.
 */
public class ExemploReader {
  public static void main(String[] args) {
    ExemploReader exemplo = new ExemploReader();
    Scanner s = new Scanner(System.in);
    System.out.print("Digite o caminho do arquivo: ");
    String caminho = s.nextLine();
    exemplo.lerArquivo(caminho);
  }

  public void lerArquivo(String caminho) {
    Reader r = null;
    try {
      r = new FileReader(caminho);
      int c;
      while((c = r.read()) != -1) {
        System.out.print((char) c);
      }
    } catch (FileNotFoundException ex) {
      System.out.println(caminho + " nao existe.");
    } catch (IOException ex) {
      System.out.println("Erro de leitura de arquivo.");
    } finally {
      try {
        if(r != null) {
          r.close();
        }
      } catch (IOException ex) {
        System.out.println("Erro ao fechar o arquivo " + caminho);
      }
    }
  }
}
{% endhighlight %}

Criamos um objeto do tipo **FileReader**, seu construtor recebe uma String com um caminho de arquivo, está classe lê os caracteres do arquivo.

Declaramos uma variável **int c**, que será utilizada para guardar os caracteres lidos do arquivo, lembre-se que todos os caracteres em Java são representados por um número inteiro.

Percorremos todo o arquivo e vamos fazer o casting de inteiro para caractere, para imprimir o texto do arquivo, note que se o valor lido através do método **read()** for igual a **-1**, significa que chegamos ao final do arquivo.

Tratamos as exceções que podem ser lançadas durante a leitura de um arquivo.

Sempre que estiver manipulando um arquivo, lembre-se de fechar o arquivo utilizando o método **close()**.

Ao executar a classe ExemploReader, teremos a seguinte saída no console:

{% highlight java %}
C:\>javac material.arquivo.ExemploReader.java
C:\>java material.arquivo.ExemploReader
Digite o caminho do arquivo: C:\Arquivos\Cancao do Exilio.txt
Minha terra tem palmeiras,
Onde canta o Sabia;
As aves, que aqui gorjeiam,
Nao gorjeiam como la.
{% endhighlight %}

#### java.io.Writer

A classe **java.io.Writer** é uma classe abstrata que representa um fluxo de saída de dados tratados como caracteres. Essa classe possui várias subclasses dedicadas a cada fonte de dados específica, tais como a classe java.io.FileWriter.

Neste exemplo, leremos um arquivo e o copiaremos para outro arquivo:

{% highlight java %}
package material.arquivo;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.Reader;
import java.io.Writer;
import java.util.Scanner;

/**
 * Classe utilizada para ler um arquivo e copia-lo.
 */
public class ExemploWriter {
  public static void main(String[] args) {
    ExemploWriter exemplo = new ExemploWriter();
    Scanner s = new Scanner(System.in);
    System.out.print("Digite o caminho do arquivo de entrada: ");
    String entrada = s.nextLine();
    System.out.print("Digite o caminho do arquivo de saida: ");
    String saida = s.nextLine();
    exemplo.copiarArquivo(entrada, saida);
  }

  public void copiarArquivo(String entrada, String saida) {
    Reader reader = null;
    Writer writer = null;

    try {
      reader = new FileReader(entrada);
      writer = new FileWriter(saida);
      int c;
      while((c = reader.read()) != -1) {
        System.out.print((char) c);
        writer.write(c);
      }
      System.out.println("\n");
    } catch (FileNotFoundException ex) {
      System.out.println(entrada + " ou " + saida + " nao existem!");
    } catch (IOException ex) {
      System.out.println("Erro de leitura de arquivo!");
    } finally {
      try {
        if(reader != null) {
          reader.close();
        }
      } catch (IOException ex) {
        System.out.println("Erro ao fechar o arquivo " + entrada);
      }

      try {
        if(writer != null) {
          writer.close();
        }
      } catch (IOException ex) {
        System.out.println("Erro ao fechar o arquivo " + saida);
      }
    }
  }
}
{% endhighlight %}

Criamos um objeto do tipo **FileReader**, para ler o arquivo. Criamos um objeto do tipo **FileWriter**, para criar um novo arquivo e escrever nele as informações do arquivo que será lido.

Percorremos os caracteres do arquivo de entrada e gravaremos os caracteres lidos no arquivo de saída.

Para liberar os recurso lembre-se de fechar o arquivo de entrada e o arquivo de saída.

Ao executar a classe **ExemploWriter**, teremos a seguinte saída no console:

{% highlight java %}
C:\>javac material\arquivo\ExemploWriter.java
C:\>java material\arquivo\ExemploWriter
Digite o caminho do arquivo de entrada: C:\Arquivos\entrada.txt
Digite o caminho do arquivo de saida: C:\Arquivos\saida.txt
Exemplo de copia de arquivo.
{% endhighlight %}

### Streams de bytes

Como já mencionado no título deste material, Streams nada mais são do que fluxos de dados sejam eles de entrada ou de saída. Na linguagem Java, graças aos benefícios trazidos ao polimorfismo, é possível se utilizar fluxos de entrada (**java.io.InputStream**) e de saída (**java.io.OutputStream**) para toda e qualquer operação deste gênero, seja ela relativa a um arquivo, a uma conexão remota via sockets ou até mesmo a entrada e saída padrão de um programa (normalmente o teclado e o console).

<figure>
    <a href="/images/2020-05-14-java-leitura-arquivo-01.png"><img src="/images/2020-05-14-java-leitura-arquivo-01.png" alt="Stream de bytes."></a>
</figure>

As classes abstratas InputStream e OutputStream definem respectivamente o comportamento padrão dos fluxos em Java: em um fluxo de entrada é possível ler bytes e no fluxo de saída escrever bytes. Exemplos concretos destas classes podem ser vistos na utilização dos métodos **print()** e **println()** de **System.out**, pois **out** é uma **java.io.PrintStream**, que é filha (indiretamente) de **OutputStream**; e na utilização de **System.in**, utilizado na construção de um objeto da classe **java.util.Scanner**, que é um **InputStream**, por isso o utilizamos para entrada de dados.

O importante a se falar sobre as Streams, é que como ambas trabalham com leitura e escrita de bytes, é importante que seja aplicado um filtro sobre esse fluxo de bytes que seja capaz de converter caracteres para bytes e vice e versa, por este motivo utilizamos o Writer e Reader.

Quando trabalhamos com classes do pacote java.io, diversos métodos lançam java.io.IOException, que é uma exception do tipo checked, o que nos obriga a tratá-la ou declará-la no método.

#### java.io.InputStream

A classe InputStream é uma classe abstrata que representa um fluxo de entrada de dados. Esse fluxo de dados é recebido de maneira extremamente primitiva em bytes. Por este motivo, existem diversas implementações em Java de subclasses de InputStream que são capazes de tratar de maneira mais específica diferentes tipos de fluxos de dados, tais como as classes java.io.FileInputStream, javax.sound.sampled.AudioInputStream, entre outros mais especializada para cada tipo de fonte.

#### java.io.OutputStream

A classe OutputStream é uma classe abstrata que representa um fluxo de saída de dados. Assim como na InputStream, esse fluxo de dados é enviado em bytes, por este motivo, existem diversas implementações em Java de subclasses de InputStream que são capazes de tratar de maneira mais específica diferentes tipos de fluxos de dados, tais como as classes **java.io.FileOutputStream**, **javax.imageio.stream.ImageOutputStream**, entre outros mais especializados para cada tipo de destino.

### Serialização de objetos

Na linguagem Java existe uma forma simples de persistir objetos, uma delas é gravando o objeto diretamente no sistema de arquivos.

Seguindo a mesma idéia das já discutidas FileInputStream e FileOutputStream, existem duas classes específicas para a serialização de objetos. São elas: **java.io.ObjectOutputStream** e **java.io.ObjectInputStream**.

Para que seja possível a escrita de um objeto em um arquivo, ou seu envio via rede (ou seja, sua conversão em um fluxo de bytes) é necessário inicialmente que este objeto implemente uma interface chamada **java.io.Serializable**. Por este motivo, a classe do objeto que desejamos serializar deve implementar esta interface, alem do que afim de evitar uma warning (aviso) no processo de compilação será necessária a declaração de um atributo constante, privado e estático chamado **serialVersionUID**. Este atributo apenas será utilizado como um controlador (um ID) no processo de serialização.

A declaração do objeto deve ocorrer assim como segue:

{% highlight java %}
package material.arquivo;

import java.io.Serializable;

/**
 * Classe utilizada para demonstrar o uso da interface Serializable.
 */
public class Pessoa implements Serializable {
  private static final long serialVersionUID = 7220145288709489651L;

  private String nome;

  public Pessoa(String nome) {
    this.nome = nome;
  }

  public String getNome() {
    return nome;
  }
}
{% endhighlight %}

Este número da serialVersionUID pode ser adquirido através do programa serialver que vem junto com o JDK, localizado na pasta %JAVA_HOME%\bin.

{% highlight java %}
C:\>serialver
use: serialver [-classpath classpath] [-show] [classname...]
C:\>javac material\arquivo\Pessoa.java
C:\> serialver material.arquivo.Pessoa
material.arquivo.Pessoa:   static final long serialVersionUID = 7220145288709489651L;
{% endhighlight %}

Feito isso, vamos nos aprofundar nas classes de leitura e escrita de objetos em arquivos físicos:

{% highlight java %}
java.io.ObjectOutputStream
{% endhighlight %}

Esta classe possui um construtor que, por padrão, deve receber uma OutputStream qualquer, como uma FileOutputStream para serialização do objeto em um arquivo. 

Exemplo:

{% highlight java %}
ObjectOutputStream out = new ObjectOutputStream( 
	new FileOutputStream("arquivo.txt"));
{% endhighlight %}

Feita esta instanciação, podemos escrever um objeto qualquer no arquivo relacionado no construtor utilizando o método writeObject (Object obj), da seguinte forma:

{% highlight java %}
Object objeto = new Object();
out.writeObject(objeto);
{% endhighlight %}

Assim como no caso do método readObject(), da classe ObjectInputStream, e nos lembrando da premissa da herança, podemos utilizar qualquer objeto neste método, da seguinte forma:

{% highlight java %}
Pessoa Manuel = new Pessoa();
out.writeObject(manuel);
{% endhighlight %}

Após a escrita do arquivo, não se esqueça de finalizar a comunicação com o arquivo utilizando o método close().


#### java.io.ObjectInputStream

Esta classe possui um construtor que, por padrão, deve receber uma InputStream qualquer, como uma FileInputStream para converter um arquivo em Objeto.

Exemplo:

{% highlight java %}
ObjectInputStream in = new ObjectInputStream(new FileInputStream("arquivo.txt"));
{% endhighlight %}

Feita esta instanciação, podemos ler um objeto a partir do arquivo relacionado no construtor utilizando o método readObject(), da seguinte forma:

{% highlight java %}
Object objeto = in.readObject();
{% endhighlight %}

Sobre este método, é importante ressaltar que por ele retornar um objeto (lembrando que todos os objetos em Java são, direta ou indiretamente, filhos da classe java.lang.Object) é possível já armazenar este retorno no objeto que já sabemos ser compatível através de uma operação de casting, da seguinte forma:

{% highlight java %}
Pessoa manual = (Pessoa) in.readObject();
{% endhighlight %}

Após a leitura do arquivo, não se esqueça de finalizar a comunicação com o arquivo utilizando o método close() de InputStream.

Exemplo que armazena um objeto em um arquivo e vice-versa.

{% highlight java %}
package material.arquivo;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

/**
 * Classe utilizada para gravar um objeto em um arquivo e depois 
 * ler o objeto do arquivo.
 */
public class TestePessoa {
  public static void main(String[] args) {
    TestePessoa tp = new TestePessoa();
    String caminho = "arquivo.txt";
    Pessoa manuel = new Pessoa("Manuel");
    tp.gravarObjeto(manuel, caminho);

    Pessoa pessoa = tp.lerObjeto(caminho);
    System.out.println(pessoa.getNome());
  }

  private void gravarObjeto(Pessoa pessoa, String caminho) {
    try {
      ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream(caminho));
      oos.writeObject(pessoa);
      oos.close();
    } catch (FileNotFoundException ex) {
      System.out.println("Erro ao ler o arquivo: " + caminho);
    } catch (IOException ex) {
      System.out.println("Erro ao gravar o objeto no arquivo");
    }
  }

  private Pessoa lerObjeto(String caminho) {
    Pessoa pessoa = null;

    try {
      ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream(caminho));
      pessoa = (Pessoa) ois.readObject();
      ois.close();
    } catch (ClassNotFoundException ex) {
      System.out.println("Erro ao converter o arquivo em objeto");
    } catch (IOException ex) {
      System.out.println("Erro ao ler o objeto do arquivo");
    }

    return pessoa;
  }
}
{% endhighlight %}

O programa gera o arquivo.txt com o objeto Pessoa no formato que ele entende para depois converter de novo para objeto Pessoa.

<figure>
    <a href="/images/2020-05-14-java-leitura-arquivo-02.png"><img src="/images/2020-05-14-java-leitura-arquivo-02.png" alt="Objeto serializado."></a>
</figure>