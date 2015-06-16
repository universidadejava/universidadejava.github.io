---
layout: article
title: "Java - Comentários de código"
categories: materiais
author: sakurai
date: 2011-06-13 18:36:00
tags: [java]
published: true
excerpt: Adicionando comentários no meio do código Java.
comments: true
image:
  teaser: teaser-java.png
ads: true
---

## Definindo comentários no código Java

Durante o desenvolvimento de um software é muito importante escrever comentários explicando os códigos fontes, pois facilita tanto o desenvolvimento do código como sua manutenção, a linguagem Java fornece três formas diferentes de escrever comentários:

{% highlight java %}
// -> Comentário de uma única linha

/* Comentário longo com mais
de uma linha */

/**
  * Javadoc
  * Javadoc é um utilitário do pacote de desenvolvimento Java utilizado
  * para a criação de um documento HTML com todos os métodos e
  * atributos das classes contidas em seu projeto.
  */
{% endhighlight %}

Exemplo:

{% gist dd788f19e685ef675715 %}

Depois de criado este arquivo, acessando a linha de comando iremos executar o seguinte comando para compilar a classe **PrimeiraClasse.java**:

Na linha 1 até a linha 7 estamos criando um comentário do tipo javadoc.

Na linha 10 usamos o comentário simples de uma linha.

Na linha 13 até a linha 14 usamos o comentário longo que pode ser utilizado por varias linhas.

## Javadoc

Javadoc é um utilitário do pacote de desenvolvimento Java utilizado para a criação de um documento HTML com todos os métodos e atributos das classes contidas em seu projeto, além dos comentários inseridos com as tags especiais:

{% highlight java %}
 /**
  * Comentário do JavaDoc.
  */
{% endhighlight %}

Os comentários do Javadoc são usados para descrever classes, variáveis, objetos, pacotes, interfaces e métodos. Cada comentário deve ser colocado imediatamente acima do recurso que ele descreve.

Também é possível utilizar-se de comandos especiais, que servem como marcação, para que na geração do documento determinadas informações já sejam colocadas em campos específicos, tais como o autor do método descrito ou sua versão. Segue abaixo alguns destes comandos:

#### Comentários gerais

**@deprecated** - adiciona um comentário de que a classe, método ou variável deveria não ser usada. O texto deve sugerir uma substituição.

**@since** - descreve a versão do produto quando o elemento foi adicionado à especificação da API.

**@version** - descreve a versão do produto.

**@see** - essa marca adiciona um link à seção "Veja também" da documentação.

#### Comentários de classes e interfaces

**@author** - autor do elemento.

**@version** - número da versão atual.

#### Comentários de métodos

**@param** - descreve os parâmetros de um método acompanhado por uma descrição.

**@return** - descreve o valor retornado por um método.

**@throws** - indica as exceções que um dado método dispara com uma descrição associada.

#### Comentários de serialização

**@serial** - para documentar a serialização de objetos.

Exemplo:

{% gist f8d371432d6d264168c4 %}

No NetBeans ou Eclipse, a utilização de tal notação já é automaticamente interpretada pela IDE, e ao se invocar este método de qualquer classe do projeto, nos será exibido uma versão compacta do javadoc da mesma, assim como segue:

{% highlight java %}
conexao.ConexaoBD(String usuario, String senha, String ipDoBanco, String nomeDaBase)
  Método construtor.
  Você deve utiliza-lo para conectar a base de dados.
Parameters:
  usuario usuário do banco de dados
  senha senha do usuário de acesso
  ipDoBanco endereço IP do banco de dados
  nomeDaBase nome da base de dados
Throws:
  SQLException
  Exception
@author
  Cristiano Camilo
@since
  1.0
@version
  1.0
{% endhighlight %}

## Gerando a documentação em HTML

Depois comentar seu programa usando as tags acima, basta somente deixar o javadoc fazer o seu trabalho, pois o mesmo vai se encarregar de gerar um conjunto de páginas HTML.

No diretório que contém os arquivos-fonte execute o comando:

{% highlight java %}
javadoc -d dirDoc nomeDoPacote
{% endhighlight %}

No qual **dirDoc** é o nome do diretório que deseja colocar os arquivos HTML.

## Gerando a documentação em HTML com Eclipse

Também é possível se gerar essa documentação a partir do Eclipse, para isso faça:

Clique no se projeto com o botão direito do mouse e selecione a opção **Export...**

Na janela de opções que será exibida, selecione a opção **Javadoc** e clique em **Next...**

A próxima janela a ser exibida pede que mais informações sejam fornecidas.

A primeira delas corresponde ao caminho do arquivo executável do Javadoc, mais abaixo você deve marcar a partir de qual pacote do seu projeto o Javadoc será gerado. É possível deixar a pasta do projeto marcada para que o Java gere documentação de todo o seu projeto.

Mais abaixo existe a opção de visibilidade dos membros os quais você deseja criar seu Javadoc. Deixe marcada a opção public, para que a documentação seja gerada para todos os membros de suas classes que estarão disponíveis para as demais classes. O importante aqui é notar que será gerada a documentação sempre do nível selecionado até os outros níveis menos restritos, ou seja, selecionando private, a documentação gerada será de todos os termos private, default ou package, protected e public.

Por fim temos a opção de Doclet, que corresponde ao padrão de fonte que será utilizado, deixe marcada a opção Standard Doclet, e aponte o destino de geração de sua documentação na opção Destination.

Feito isso, basta clicar em **Finish** e sua documentação estará disponível na pasta solicitada.
