---
layout: article
title: "Criptografia de chave pública ou assimétrica"
categories: outros
author: sakurai
date: 2020-05-23 23:07:00
tags: [java, criptografia, assimetrica, AES]
published: true
excerpt: 
comments: true
image:
  teaser: 2020-05-23-teaser-criptografia-assimetrica.png
ads: false
---

## Criptografia de chave pública ou assimétrica

<figure>
    <a href="/images/2020-05-23-criptografia-assimetrica-01.png"><img src="/images/2020-05-23-criptografia-assimetrica-01.png" alt="Criptografia de chave pública ou assimétrica"></a>
</figure>

Se criptografar um texto usando a chave privada ele só poderá ser descriptografado com a chave pública e vice-versa. Desse modo com a mesma chave não é possível criptografar e descriptografar, por isso o nome assimétrica.

A ideia dessa criptografia é você usar a chave pública para criptografar um arquivo e enviar este arquivo para quem possui a chave privada, porque somente com a chave privada será possível descriptografar o arquivo.


## Usando o RSA implementado no Java

No método `main` vamos gerar um par de chaves pública/privada e vamos utilizar a criptografia RSA para criptografar e descriptografar um arquivo chamado `texto.txt`. O arquivo criptografado chama `criptografado.txt` e o arquivo que será descriptografado chama `saida.txt`.

{% highlight java %}
public classe CriptografiaAssimetrica {
  public static void main(String[] args) throws Exception {
    // Gera um par de chaves para o RSA
    gerarChaves();

    // Carrega as chaves pública e privada que foram geradas.
    Key chavePrivada = obterKey(new File("ChavePrivada"), PrivateKey.class);
    Key chavePublica = obterKey(new File("ChavePublica"), PublicKey.class);
    
    // Criptografa com a chave privada o conteúdo do arquivo texto.txt e salva no arquivo criptografado.txt
    RSAEncrypter rsa = new RSAEncrypter();
    rsa.criptografar(chavePrivada, new FileInputStream("texto.txt"), new FileOutputStream("criptografado.txt"));

    // Descriptografa o conteúdo do arquivo criptografado.txt e salva no arquivo saida.txt
    rsa.descriptografar(chavePublica, new FileInputStream("criptografado.txt"), new FileOutputStream("saida.txt"));
  }

  // Implemente os métodos a seguir aqui.
}
{% endhighlight %}

O método `public static Key obterKey(File arquivo, Class <? extends Key> clazz) throws Exception` carrega um arquivo contendo o objeto da chave, essa chave pode ser a pública ou privada.

{% highlight java %}
  public static Key obterKey(File arquivo, Class <? extends Key> clazz) throws Exception {
    // Carrega o arquivo usando um stream de objetos
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream(arquivo));
    // Lê o objeto da chave que foi salvo no arquivo
    Key chave = clazz.cast(ois.readObject());
    // Fecha o arquivo e retorna a chave lida
    ois.close();
    return chave;
  }
{% endhighlight %}

O método `public static void gerarChaves() throws Exception` gera um par de chaves pública/privada e salva seus objetos nos arquivos chamado `ChavePublica` e `ChavePrivada`.

{% highlight java %}
  public static void gerarChaves() throws Exception {
    // Cria um gerador de chaves para o RSA
    KeyPairGenerator kpg = KeyPairGenerator.getInstance("RSA");
    // Inicializa o tamanho da chave e seu expoente público
    kpg.initialize(new RSAKeyGenParameterSpec(1024, RSAKeyGenParameterSpec.F4));
    // Gera um par de chaves
    KeyPair par = kpg.generateKeyPair();

    // Grava o objeto da chave pública no arquivo
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(new File("ChavePublica")));
    oos.writeObject(par.getPublic());
    oos.close();

    // Grava o objeto da chave privada no arquivo
    oos = new ObjectOutputStream(new FileOutputStream(new File("ChavePrivada")));
    oos.writeObject(par.getPrivate());
    oos.close();
  }
{% endhighlight %}

O método `public void criptografar(Key chave, InputStream in, OutputStream out) throws Exception` usa uma das chaves pública ou privada (normalmente é usado a chave pública) para criptografar o arquivo recebido no parâmetro `in` e grava o arquivo criptografado usando o algoritmo RSA no arquivo do parâmetro `out`.

{% highlight java %}
  // Define um vetor de bytes do tamanho da chave
  private byte[] buf = new byte[1024];

  public void criptografar(Key chave, InputStream in, OutputStream out) throws Exception {
    // Obtém uma instância da criptografia RSA
    Cipher cifra = Cipher.getInstance("RSA");
    // Inicializa o RSA com o modo de criptografia e a chave que será usada
    cifra.init(Cipher.ENCRYPT_MODE, chave);
    // Cria um stream que recebe uma entrada descriptografada para criptografar
    CipherOutputStream cipherOut = new CipherOutputStream(out, cifra);

    // Criptografa o conteúdo da entrada (in) e guarda o resultado na saída (out).
    int numLido = 0;
    while ((numLido = in.read(buf)) >= 0) {
      cipherOut.write(buf, 0, numLido);
    }
    cipherOut.close();
  }
{% endhighlight %}

O método `public void descriptografar(Key chave, InputStream in, OutputStream out) throws Exception` usa uma das chaves pública ou privada (normalmente é usado a chave privada) para decriptografar o arquivo recebido no parâmetro `in` e grava o arquivo descriptografado usando o algoritmo RSA no arquivo do parâmetro `out`.

{% highlight java %}
  public void descriptografar(Key chave, InputStream in, OutputStream out) throws Exception {
    // Obtém uma instância da criptografia RSA
    Cipher cifra = Cipher.getInstance("RSA");
    // Inicializa o RSA com o modo de descriptografia e a chave que será usada
    cifra.init(Cipher.DECRYPT_MODE, chave);
    // Cria um stream que recebe uma entrada criptografada para descriptografar
    CipherInputStream cipherIn = new CipherInputStream(in, cifra);

    // Descriptografa o conteúdo da entrada (in) e guarda o resultado na saída (out).
    int numLido = 0;
    while ((numLido = cipherIn.read(buf)) >= 0) {
      out.write(buf, 0, numLido);
    }
    out.close();
  }
{% endhighlight %}

## Combinando criptografia simétrica e assimétrica

<figure>
    <a href="/images/2020-05-23-criptografia-assimetrica-02.png"><img src="/images/2020-05-23-criptografia-assimetrica-02.png" alt="Combinando criptografia simétrica e assimétrica"></a>
</figure>

O uso apenas da criptografia simétrica possui o problema de como compartilhar a chave privada com o destinatário sem que nenhum invasor consiga interceptar e também obter essa chave privada.

Uma solução para esse problema é combinar o uso da criptografia simétrica e assimétrica, podemos fazer isso em quatro passos:

1. Origem manda chave pública (Assimétrica) para o Destino. Como a chave pública pode ser de conhecimento de qualquer um, não importa se um invasor também obter essa chave.

2. Destino utiliza a chave pública (Assimétrica) recebida e criptografa sua chave privada (Simétrica) e envia para a Origem. Nesse momento a chave simétrica está criptograda, e apenas a Origem possui a chave privada (Assimétrica) capaz de descriptografá-la.

3. Origem descriptografa usando sua chave privada (Assimétrica). Agora ambos origem e destino possuem a mesma chave privada (Simétrica).

4. Origem criptografa os dados usando a chave privada (Simétrica) e envia para o destino que também possui a mesma chave privada (Simétrica) e consegue descriptografar esses dados.


## Conteúdos relacionados

- [Introdução a criptografia](http://www.universidadejava.com.br/outros/introducao-criptografia/)
- [Criptografia simétrica utilizando chave privada](http://www.universidadejava.com.br/outros/criptografia-simetrica/)