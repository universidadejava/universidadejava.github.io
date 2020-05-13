---
layout: article
title: "Java - Interfaces"
categories: java
author: sakurai
date: 2020-05-13 00:13:00
tags: [java]
published: true
excerpt: Interface é um recurso da linguagem Java que apresenta inúmeras vantagens no sentido da modelagem e instanciação de objetos.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Interfaces

Interface é um recurso da linguagem Java que apresenta inúmeras vantagens no sentido da modelagem e instanciação de objetos, porém deve-se entender claramente os conceitos básicos da orientação a objetos a fim de utilizá-la plenamente.

Uma interface é similar a um contrato, através dela podemos especificar quais métodos as classes que implementam esta interface são obrigados a implementar.

Exemplo de interface UML:

<figure>
    <a href="/images/2020-05-13-java-interface-01.png"><img src="/images/2020-05-13-java-interface-01.png" alt="Modelando uma interface com UML."></a>
</figure>

Estes são dois modos de se representar uma interface utilizando a UML, criamos uma interface chamada **Banco**, em Java está interface fica do seguinte modo:

{% highlight java %}
package material.exemploInterface;

/**
 * Interface utilizada para representar os métodos minimos que
 * os bancos precisam implementar.
 */
public interface Banco {
  public abstract void manutencaoConta(Conta conta);
  public abstract boolean saque(Conta conta, double valor);
  public abstract boolean deposito(Conta conta, double valor);
  public abstract void extrato(Conta conta);
}
{% endhighlight %}

Para criarmos uma interface em Java, utilizamos a palavra-chave **interface** antes do seu nome.

Na interface declaramos os métodos de forma abstrata, tendo apenas sua assinatura e terminando com ponto e vírgula ( ; ) e todos os métodos precisam ser públicos. Podemos declarar atributos, porém todos os atributos de uma interface são constantes publicas. Uma interface pode estender (sub-interface) outras interfaces.

Dentro da interface não podemos:
- implementar método
- construtor
- estender classe
- implementar outra interface
- não pode ser final

Quando uma classe implementa uma interface, está classe obrigatoriamente precisa implementar todos os métodos declarados na interface, apenas quando usamos classes abstratas implementando interface que não precisamos obrigatoriamente implementar todos os métodos (classes abstratas serão vistas mais para frente).

Na UML a implementação de uma interface é feita da seguinte forma:

<figure>
    <a href="/images/2020-05-13-java-interface-02.png"><img src="/images/2020-05-13-java-interface-02.png" alt="Interface do banco e suas implementações."></a>
</figure>

Neste exemplo, temos duas classes **BancoSakurai** e **BancoCristiano** implementando a interface **Banco**. Nas classes podem ser feitas implementações diferentes dos métodos, mas as assinaturas dos métodos precisam ser iguais as da interface, podemos também adicionar mais métodos que os obrigatórios.

Neste exemplo também estamos usando uma classe conta:

{% highlight java %}
package material.exemploInterface;

/**
 * Classe utilizada para representar uma Conta bancaria.
 */
public class Conta {
  /**
   * Nome do proprietario da Conta.
   */
  private String nomeProprietario;

  /**
   * Número da Conta.
   */
  private int numero;

  /**
   * Saldo da Conta.
   */
  private double saldo;

  public String getNomeProprietario() {
    return nomeProprietario;
  }

  public void setNomeProprietario(String nomeProprietario) {
    this.nomeProprietario = nomeProprietario;
  }

  public int getNumero() {
    return numero;
  }

  public void setNumero(int numero) {
    this.numero = numero;
  }

  public double getSaldo() {
    return saldo;
  }

  public void setSaldo(double saldo) {
    this.saldo = saldo;
  }
}
{% endhighlight %}

O código Java dessas duas classes ficam da seguinte forma:

{% highlight java %}
package material.exemploInterface;

/**
 * Classe utilizada para representar o Banco Sakurai
 */
public class BancoSakurai implements Banco {
  /**
   * Conta que representa o Banco Sakurai.
   */
  private Conta contaBancoSakurai;

  /**
   * Construtor padrão da classe.
   * 
   * Cria uma conta para o banco sakurai.
   */
  public BancoSakurai() {
    this.contaBancoSakurai = new Conta();
    this.contaBancoSakurai.setNomeProprietario("Banco Sakurai");
    this.contaBancoSakurai.setNumero(0);
    this.contaBancoSakurai.setSaldo(0d);
  }

  public void manutencaoConta(Conta conta) {
    boolean temSaldo = conta.getSaldo() >= 0.25;

    //Verifica se tem saldo para realizar a manutenção da conta.
    if(temSaldo) {
      double novoSaldo = conta.getSaldo() - 0.25;
      conta.setSaldo(novoSaldo);

      //Deposita o dinheiro da manutencao na conta do banco sakurai.
      deposito(this.contaBancoSakurai, 0.25);
    } else {
      System.out.println("Não conseguiu cobrar a manutenção da conta" +
          conta.getNumero() + " !!!");
    }
  }

  public boolean saque(Conta conta, double valor) {
    //Verifica se tem saldo suficiente para fazer o saque
    if(conta.getSaldo() >= valor) {
      //Realiza o saque na conta.
      double novoValor = conta.getSaldo() - valor;
      conta.setSaldo( novoValor );
      System.out.println("Saque efetuado!!!");

      //Toda vez que fizer um saque faz cobra a manutenção da conta.
      manutencaoConta(conta);
      return true;
    } else {
      System.out.println("Não conseguiu fazer o saque!!!");
      //Se não conseguir fazer o saque, mostra o extrato da conta.
      extrato(conta);
      return false;
    }
  }

  public boolean deposito(Conta conta, double valor) {
    //Realiza o deposito na conta.
    double novoValor = conta.getSaldo() + valor;
    conta.setSaldo(novoValor);
    System.out.println("Deposito efetuado!!!");
    return true;
  }

  public void extrato(Conta conta) {
    System.out.println("\n -- BANCO SAKURAI -- \n");
    System.out.println("-> EXTRATO CONTA\n");
    System.out.println("Nome: " + conta.getNomeProprietario());
    System.out.println("Numero: " + conta.getNumero());
    System.out.println("Saldo: " + conta.getSaldo());
    System.out.println("\n---------------------\n");
  }

  public boolean transferencia(Conta contaOrigem, Conta contaDestino, double valor) {
    boolean fezSaque = saque(contaOrigem, valor);
    //Verifica se conseguiu sacar dinheiro na conta de origem.
    if(fezSaque) {
      //Faz o deposito na conta de destino.
      deposito(contaDestino, valor);
      System.out.println("Transferencia efetuada.");
      return true;
    } else {
      System.out.println("Não conseguiu fazer a transferencia.");
      return false;
    }
  }
}
{% endhighlight %}

Nesta implementação o BancoSakurai alem de fornecer os métodos obrigatórios da interface Banco, também disponibiliza um método adicional chamado **transferencia(Conta contaOrigem, Conta contaDestino, double valor)**.

{% highlight java %}
package material.exemploInterface;

/**
 * Classe utilizada para representar o Banco Cristiano.
 */
public class BancoCristiano implements Banco {
  private Conta contaBancoCristiano;

  public BancoCristiano() {
    this.contaBancoCristiano = new Conta();
    this.contaBancoCristiano.setNomeProprietario("Banco Cristiano");
    this.contaBancoCristiano.setNumero(0);
    this.contaBancoCristiano.setSaldo(0d);
  }

  public void manutencaoConta(Conta conta) {
    //Sempre executa o saque na conta bancaria.
    double novoSaldo = conta.getSaldo() - 0.01;
    conta.setSaldo(novoSaldo);

    //Deposita o dinheiro da manutenção na conta do Banco Cristiano.
    double saldoBanco = this.contaBancoCristiano.getSaldo() + 0.01;
    this.contaBancoCristiano.setSaldo(saldoBanco);
  }

  public boolean saque(Conta conta, double valor) {
    //Verifica se tem saldo suficiente para fazer o saque
    if(conta.getSaldo() >= valor) {
      //Realiza o saque na conta.
      double novoValor = conta.getSaldo() - valor;
      conta.setSaldo( novoValor );
      System.out.println("Saque efetuado!!!");

      //Toda vez que fizer um saque faz cobra a manutenção da conta.
      manutencaoConta(conta);
      return true;
    } else {
      System.out.println("Não conseguiu fazer o saque!!!");
      //Se não conseguir fazer o saque, mostra o extrato da conta.
      extrato(conta);
      return false;
    }
  }

  public boolean deposito(Conta conta, double valor) {
    //Realiza o deposito na conta.
    double novoValor = conta.getSaldo() + valor;
    conta.setSaldo(novoValor);
    System.out.println("Deposito efetuado!!!");

    //Toda vez que fizer um deposito faz cobra a manutenção da conta.
    manutencaoConta(conta);

    return true;
  }

  public void extrato(Conta conta) {
    System.out.println("\n -- BANCO CRISTIANO -- \n");
    System.out.println("-> EXTRATO CONTA\n");
    System.out.println("Nome: " + conta.getNomeProprietario());
    System.out.println("Numero: " + conta.getNumero());
    System.out.println("Saldo: " + conta.getSaldo());
    System.out.println("\n---------------------\n");
  }
}
{% endhighlight %}

Nesta classe BancoCristiano, podemos ver que ele possui apenas os métodos obrigatórios da interface Banco.

Está implementação tem um algoritmo diferente do BancoSakurai, aqui o método **manutencaoConta()** tem um valor que será retirado da conta mesmo que a conta não tenha mais saldo.

Assim podemos perceber que a interface serve como uma base, informando os métodos mínimos que devem ser implementados e cada classe é responsável pela lógica de negocio utilizado nos métodos.

{% highlight java %}
package material.exemploInterface;

/**
 * Classe utilizada para testar a interface Banco, e as 
 * classes BancoCristiano e BancoSakurai.
 */
public class BancoTeste {
  public static void main(String[] args) {
    Banco bancoCristiano = new BancoCristiano();
    Conta contaC = new Conta();
    contaC.setNomeProprietario("Cristiano Camilo");
    contaC.setNumero(1);
    contaC.setSaldo(1000);

    bancoCristiano.deposito(contaC, 150.50);
    bancoCristiano.saque(contaC, 500);
    bancoCristiano.extrato(contaC);

    Banco bancoSakurai = new BancoSakurai();
    Conta contaS = new Conta();
    contaS.setNomeProprietario("Rafael Sakurai");
    contaS.setNumero(1);
    contaS.setSaldo(500);

    bancoSakurai.deposito(contaS, 40.99);
    bancoSakurai.saque(contaS, 300);
    bancoSakurai.extrato(contaS);
  }
}
{% endhighlight %}

Neste exemplo criamos dois bancos e duas contas e fizemos algumas operações em cima das contas e no final imprimimos o extrato das contas.

Note a seguinte sintaxe:

{% highlight java %}
Banco bancoCristiano = new BancoCristiano();
{% endhighlight %}

Repare que estamos criando uma variável bancoCristiano do tipo da interface Banco, depois estamos criando um objeto da classe BancoCristiano que é uma classe que implementa a interface Banco. Este tipo de sintaxe é muito usado quando usamos polimorfismo. (Polimorfismo será abordado mais adiante)

Quando executarmos a classe **BancoTeste**, temos a seguinte saída no console:

{% highlight java %}
C:\>javac material\exemploInterface\BancoTeste.java
C:\>java material.exemploInterface.BancoTeste
Deposito efetuado!!!
Saque efetuado!!!

 -- BANCO CRISTIANO --

-> EXTRATO CONTA

Nome: Cristiano Camilo
Numero: 1
Saldo 650.48

----------------------

Deposito efetuado!!!
Saque efetuado!!!
Deposito efetuado!!!

 -- BANCO SAKURAI --

-> EXTRATO CONTA

Nome: Rafael Sakurai
Numero: 1
Saldo: 240.74

----------------------
{% endhighlight %}

Quando usamos uma variável usando o tipo interface, podemos apenas chamar os métodos que possuem na interface, a seguinte linha causará um erro de compilação:

{% highlight java %}
bancoSakurai.transferencia(contaS, contaC, 1000.00);
{% endhighlight %}

Neste exemplo teremos um erro de compilação, pois a interface Banco não tem a assinatura para o método **transferencia(Conta contaOrigem, Conta contaDestino, double valor)**, este método é disponibilizado através da classe BancoSakurai, então para usá-lo precisaremos de uma variável do tipo BancoSakurai.

{% highlight java %}
BancoSakurai banco = new BancoSakurai();
{% endhighlight %}

Uma interface em Java, após sua compilação, também gera um arquivo .class, assim como qualquer outra classe em Java.