---
layout: article
title: "Funções de hash"
categories: outros
author: sakurai
date: 2020-05-23 22:50:00
tags: [java, criptografia, hash]
published: true
excerpt: Funções de hash são utilizadas quando queremos criptografar uma informação para que ela não possa ser descriptografada. Muito usado para ocultar senhas e assinatura digital.
comments: true
image:
  teaser: 2020-05-23-teaser-criptografia-funcao-hash.png
ads: false
---

## Funções de Hash

Funções de Hash tem como objetivo gerar valores que são praticamente impossíveis de serem revertidos. Muito utilizado para ocultar senhas e uso em assinatura digital.

Função de hash (ou dispersão) mapeia um dado de tamanho variável para um tamanho fixo. Tem como objetivo gerar uma identificação única para uma informação ou arquivo.

## Secure Hash Algorithm 2 (SHA-2)

No Java temos uma implementação para o SHA-2, podemos obter uma instância dele usando o método `MessageDigest.getInstance("SHA-256")` e passando o conteúdo que queremos gerar o hash no método `digest`. O código a seguir apresenta um exemplo de como gerar o hash de uma String.

{% highlight java %}
import java.security.MessageDigest;

public class FuncaoHash {
  public static void main(String args[]) throws Exception {
    String senha = "Minha senha secreta";
    System.out.println(gerarHash(senha));
  }

  public static String gerarHash(String senha) throws Exception {
    MessageDigest algorithm = MessageDigest.getInstance("SHA-256");
    byte hash[] = algorithm.digest(senha.getBytes("UTF-8"));

    StringBuilder texto = new StringBuilder();
    for (byte b : hash) {
      texto.append(String.format("%02X", 0xFF & b));
    }
    return texto.toString();
  }
}
{% endhighlight %}

O hash do texto `Minha senha secreta` é:

{% highlight java %}
d7ef79729bb0897a3d3fc029e55cabc464a05cE15b99ca77bd1ecd6eb36ff1ec
{% endhighlight %}

Importante é que não importa o tamanho do texto ou do arquivo, o hash sempre possui o mesmo tamanho.

## Uso da função de hash na assinatura digital

Nesse exemplo, estou aplicando a função de hash em um texto, mas essa mesma função é muito usada para gerar o hash de um arquivo, porque note que apesar de usar uma String, o método `digest` recebe como parâmetro um array de bytes, por tanto podemos usar o array de bytes de qualquer coisa: uma imagem, um arquivo, um vídeo, etc, para gerar o seu hash.

Quando queremos transferir um arquivo de uma pessoa para outra pessoa, como é o caso da assinatura digital que queremos comprovar quem realmente está enviando o arquivo original, podemos fazer isso em três passos:

Primeiro, gerando o hash do arquivo original (um arquivo que criamos e estamos enviando para alguém).

<figure>
    <a href="/images/2020-05-23-criptografia-funcao-hash-01.png"><img src="/images/2020-05-23-criptografia-funcao-hash-01.png" alt="Gerando o hash do arquivo original."></a>
</figure>

Segundo, usando nossa chave privada, vamos criptografar o hash gerado. Então enviamos para o destinatario o arquivo original e o hash criptografado.

<figure>
    <a href="/images/2020-05-23-criptografia-funcao-hash-02.png"><img src="/images/2020-05-23-criptografia-funcao-hash-02.png" alt="Criptografando o hash gerado."></a>
</figure>

Terceito, o destinátario utilizando a chave publica do remetente, descriptografa o hash e verifica se o hash do arquivo é o mesmo do hash recebido.

<figure>
    <a href="/images/2020-05-23-criptografia-funcao-hash-03.png"><img src="/images/2020-05-23-criptografia-funcao-hash-03.png" alt="Descriptografando o hash com a chave pública."></a>
</figure>

Com isso conseguimos garantir três princípios básicos de segurança:

- **Autenticação** - É possível conferir se o documento é original ou se ele foi modificado. Isto é possível porque ao receber um arquivo e o hash criptografado deste arquivo, podemos usar a chave pública para descriptografar o hash e gerar também o hash deste mesmo arquivo para verificar se condiz com o documento original. Como só é possível gerar o hash criptografado utilizando a chave privada, e se o hash recebido é igual ao hash gerado do arquivo conseguimos garantir que o arquivo é autêntico.

- **Integridade** - Garantia que o documento recebido é o mesmo que o documento enviado. Se no meio do processo de envio, o arquivo original for modificado, o arquivo de hash gerado a partir do arquivo não será igual ao hash recebido de forma criptografada. Então se o hash recebido é igual ao hash gerado do arquivo conseguimos garantir que o arquivo é íntegro e não foi modificado.

- **Não repúdio** - Temos certeza de quem enviou o arquivo. Como somente o remetente possui a chave privada, conseguimos ter certeza de quem está enviando e podemos aceitar o arquivo se conseguirmos descriptografá-lo com a chave publica.

Agora um ponto importante, nesse processo criptografamos o hash do arquivo e enviamos este hash criptogragado. Então é importante entender que este processo **não garante a confidencialidade do documento**.


## Conteúdos relacionados

- [Introdução a criptografia](http://www.universidadejava.com.br/outros/introducao-criptografia/)
- [Criptografia simétrica utilizando chave privada](http://www.universidadejava.com.br/outros/criptografia-simetrica/)
- [Criptografia assimétrica utilizando um par de chaves pública/privada](http://www.universidadejava.com.br/outros/criptografia-assimetrica/)