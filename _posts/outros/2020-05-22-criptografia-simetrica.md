---
layout: article
title: "Criptografia de chave privada - simétrica"
categories: outros
author: sakurai
date: 2020-05-22 23:07:00
tags: [java, criptografia, simetrica, AES]
published: true
excerpt: 
comments: true
image:
  teaser: 2020-05-22-teaser-criptografia-simetrica.png
ads: false
---

## Criptografia de chave privada - simétrica

<figure>
    <a href="/images/2020-05-22-criptografia-simetrica-01.png"><img src="/images/2020-05-22-criptografia-simetrica-01.png" alt="Criptografia de chave privada - simétrica"></a>
</figure>

A criptografia usando uma chave privada tem como objetivo utilizar uma chave que serve tanto para criptografar como para descriptografar. Este tipo de criptografia é utilizado quando ambos origem e destino possuem acesso a mesma chave.


## Implementando criptografia com AES

> O código de implementação do uso da criptografia simétrica usando [AES](https://pt.wikipedia.org/wiki/Data_Encryption_Standard), apresentado a seguir é uma adaptação do código disponível no livro de DASWANI, N.; KERN, C.; KESAVAN, A. página 211.

Vamos começar declarando os atributos que vamos utilizar na classe e o construtor:

{% highlight java %}
public classe CriptografiaSimetrica {
  private static final int KEY_SIZE = 16;
  private static final int IV_SIZE = 16;
  private static final int BUFFER_SIZE = 1024;

  private Cipher cifra;
  private SecretKey chave;
  private AlgorithmParameterSpec ivSpec;
  private byte[] buf = new byte[BUFFER_SIZE];
  private byte[] ivBytes = new byte[IV_SIZE];

  public CriptografiaSimetrica(SecretKey chave) throws Exception {
    this.cifra = Cipher.getInstance("AES/CBC/PKCS5Padding");
    this.chave = chave;
  }

  // Implemente os métodos a seguir aqui.
{% endhighlight %}

No método `main` vamos criar uma chave privada e guardar ela dentro de um arquivo chamado `MinhaChave`. Depois vamos utilizar a criptografia simetrica AES para ler o conteúdo do arquivo `TextoOriginal.txt` e gravar seu conteúdo criptografado no arquivo `Criptografado.txt`.

{% highlight java %}
  public static void main(String[] args) throws Exception {
    // Nome do arquivo com que será salvo a chave gerada.
    String arquivoChave = "MinhaChave";
    // Cria e salva a chave no arquivo.
    criarChave(arquivoChave);
    //Carrega a chave que foi gerada.
    SecretKeySpec keySpec = lerChave(arquivoChave);

    // Inicializa a classe de criptografia com a chave simétrica.
    CriptografiaSimetrica aes = new CriptografiaSimetrica(keySpec);
    // Criptografa o conteúdo do arquivo.
    aes.criptografar(new FileInputStream("TextoOriginal.txt"), new
      FileOutputStream("Criptografado.txt"));
    // Descriptografa o conteúdo do arquivo.
    aes.descriptografar(new FileInputStream("Criptografado.txt"), new 
      FileOutputStream("Saida.txt"));
  }
{% endhighlight %}

No construtor foi definido `this.cifra = Cipher.getInstance("AES/CBC/PKCS5Padding");` que informa o algoritmo de criptografia AES que será utilizado e também recebe a `chave` que será utilizada pela criptografia.

Essa `chave` recebida no construtor deve ser criada previamente, o método `public static void criarChave(String nomeArquivo) throws Exception` implementa uma forma de criar a chave privada usando a criptografia AES. Esse método recebe como parâmetro um caminho para salvar o arquivo contendo a chave privada.

{% highlight java %}
  public static void criarChave(String nomeArquivo) throws Exception {
    // Define o gerador de chaves AES.
    KeyGenerator kg = KeyGenerator.getInstance("AES");
    // Tamanho da chave. Nesse exemplo KEY_SIZE é igual 16, portanto a chave é de 128 bits.
    kg.init(KEY_SIZE * 8);
    // Gera uma nova chave.
    SecretKey chave = kg.generateKey();

    // Salva a chave em um arquivo.
    FileOutputStream fos = new FileOutputStream(nomeArquivo);
    fos.write(chave.getEncoded());
    fos.close();
  }
{% endhighlight %}

O método `public static SecretKeySpec lerChave(String nomeArquivo) throws Exception` recebe como parâmetro o caminho do arquivo contendo a chave privada e carrega um objeto `SecretKeySpec` que representa essa chave.

{% highlight java %}
public static SecretKeySpec lerChave(String nomeArquivo) throws Exception {
  // Lê o conteúdo do arquivo e guarda no vetor de bytes.
  byte bytesChave[] = new byte[KEY_SIZE];
  FileInputStream fis = new FileInputStream(nomeArquivo);
  fis.read(bytesChave);

  // Cria uma chave AES com os bytes lidos.
  return new SecretKeySpec(bytesChave, "AES");
}
{% endhighlight %}

O método `public void criptografar(InputStream in, OutputStream out) throws Exception` recebe um arquivo de entrada com o conteúdo original e um arquivo de saída no qual será gravado o conteúdo criptografado do arquivo de entrada usando o AES.

{% highlight java %}
public void criptografar(InputStream in, OutputStream out) throws Exception {
  // Gera um conjunto de bytes aleatórios.
  ivBytes = createRandBytes(IV_SIZE);
  // Guarda os bytes gerados aleatoriamente no arquivo com o conteúdo criptografado.
  out.write(ivBytes);
  // Cria um vetor de inicialização para a criptografia.
  ivSpec = new IvParameterSpec(ivBytes);
  // Inicializa o algoritmo de criptografia definindo: modo de criptografia, chave e o vetor de inicialização.
  cifra.init(Cipher.ENCRYPT_MODE, chave, ivSpec);
  // Cria um stream que recebe uma entrada descriptografada para criptografar.
  CipherOutputStream cipherOut = new CipherOutputStream(out, cifra);
  // Lê uma parte da entrada, se essa parte lida tiver um tamanho maior que zero, então criptografa e guarda no arquivo de saída.
  int numLido = 0;
  while ((numLido = in.read(buf)) >= 0)
    cipherOut.write(buf, 0, numLido);
  cipherOut.close();
}
{% endhighlight %}

O método `public static byte[] createRandBytes(int numBytes) throws NoSuchAlgorithmException` cria um array de bytes contendo uma sequência aleatória.

{% highlight java %}
public static byte[] createRandBytes(int numBytes) throws NoSuchAlgorithmException {
  byte[] bytesBuffer = new byte[numBytes];

  /* Utiliza um sistema mais seguro de geração de números aleatórios, que 
     dificulta adivinhar qual será o próximo número aleatório gerado. */
  SecureRandom sr = SecureRandom.getInstance("SHA1PRNG");
  // Guarda os bytes gerados nesse array.
  sr.nextBytes(bytesBuffer);

  return bytesBuffer;
}
{% endhighlight %}

O método `public void descriptografar(InputStream in, OutputStream out) throws Exception` recebe como entrada no parâmetro `in` o arquivo que está criptografado e no parâmetro `out` o caminho do arquivo onde será gravado o arquivo descriptografado.

{% highlight java %}
public void descriptografar(InputStream in, OutputStream out) throws Exception {
  // Lê os bytes aleatórios e cria o vetor de inicialização para a criptografia.
  in.read(ivBytes);
  ivSpec = new IvParameterSpec(ivBytes);
  // Inicializa o algoritmo de criptografia definindo: modo de descriptografia, chave e o vetor de inicialização.
  cifra.init(Cipher.DECRYPT_MODE, chave, ivSpec);
  // Cria um stream que recebe uma entrada criptografada para descriptografar.
  CipherInputStream cipherIn = new CipherInputStream(in, cifra);
  /* Lê uma parte da entrada, se essa parte lida tiver um tamanho maior que zero, 
     então descriptografa e guarda no arquivo de saída. */
  int numLido = 0;
  while ((numLido = cipherIn.read(buf)) >= 0) {
    out.write(buf, 0, numLido);
  }
  out.close();
}
{% endhighlight %}

<figure>
    <a href="/images/2020-05-22-criptografia-simetrica-02.png"><img src="/images/2020-05-22-criptografia-simetrica-02.png" alt="Criptografar e descriptografar usando AES."></a>
</figure>


## Referência

[DASWANI, N.; KERN, C.; KESAVAN, A.] **Foundations of Security.** United States of America: Apress, 2007.

## Conteúdos relacionados

- [Introdução a criptografia](http://www.universidadejava.com.br/outros/introducao-criptografia/)
- [Criptografia assimétrica utilizando um par de chaves pública/privada](http://www.universidadejava.com.br/outros/criptografia-assimetrica/)
- [Aprenda a aplicar a função de hash em um texto e como é usado na assinatura digital](http://www.universidadejava.com.br/outros/criptografia-funcao-hash/)
- [Conhecendo e previnindo a vulnerabilidade SQL Injection](http://www.universidadejava.com.br/outros/vulnerabilidade-sql-injection)