---
layout: article
title: "Java - Exercícios de exceções"
categories: java
author: sakurai
date: 2020-05-13 06:35:00
tags: [java]
published: true
excerpt: Exercícios para praticar o uso de exceções no Java. 
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Exercícios de exceções

1. Crie uma classe que aceite a digitação de dois números e faça a divisão entre eles exibindo seu resultado. Sua classe deve tratar as seguintes exceções:

- **ArithmeticException** quando ocorrer uma divisão por zero.

- **InputMismatchException** quando o valor informado não é numerico.


2. Crie uma classe que crie um vetor de inteiros de 10 posições. Feito isso, permita que o usuário digite valores inteiros a fim de preencher este vetor. Não implemente nenhum tipo controle referente ao tamanho do vetor, deixe que o usuário digite valores até que a entrada 0 seja digitada.

Uma vez digitado o valor 0, o mesmo deve ser inserido no vetor e a digitação de novos elementos deve ser interrompida. Feita toda a coleta dos dados, exiba-os em tela.

Sua classe deve tratar as seguintes exceções:

- **ArrayIndexOutOfBoundsException** quando o usuário informar mais que 10 valores.

- **InputMismatchException** quando o usuário informar um valor que não é numerico.


3. Crie uma classe Login com a seguinte modelagem:

Login

{% highlight java %}
private String usuario; // Determina o nome do usuário
private String senha; // Determina a senha do usuário.
{% endhighlight %}

E os métodos:

{% highlight java %}
public Login(String _usuario, String _senha) {
	// Construtor padrão
}

public void setSenha (String _senha) {
	// Troca a senha do usuário.
}

public boolean fazerLogin(String _usuario, String _senha){
/*
	Deve receber informações de usuário e senha e compara-las com as da classe. Caso sejam realmente iguais, deve retornar verdadeiro, ou então deve lançar uma nova exceção dizendo qual credencial está errada, tratar essa exceção dentro do próprio método imprimindo o erro em tela e por fim retornar false.
	Ex:

	try{
		if(<<usuário incorreto>>) {
			throw new Exception(“Usuário incorreto”);
		}
	} catch (Exception e) {
		System.out.println(“Erro”);
	}
*/
}
{% endhighlight %}

Feito isso, crie uma classe para testar a inicialização de um objeto do tipo Login e que utilize o método fazerLogin, com informações digitadas pelo usuário.


4. O que será impresso se tentarmos compilar e executar a classe TesteExcecao?

{% highlight java %}
class MinhaExcecao extends Exception {
	
}

public class TesteExcecao {
	public static void teste() throws MinhaExcecao {
		throw new MinhaExcecao();
	}

	public static void main(String[] args) {
		MinhaExcecao me = null;
		try {
			System.out.println("try ");
               teste();
		} catch (MinhaExcecao e) {
			System.out.println("catch ");
			me = e;
		} finally {
			System.out.println("finally ");
			throw me;
		}

		System.out.println("fim");
	}
}
{% endhighlight %}

a) Imprime "try catch"
b) Imprime "catch finally"
c) Imprime "try catch finally "
d) Imprime "try catch finally fim"
e) Erro de compilação


5-) Crie uma classe chamada ContaBancaria, pertencente ao pacote “exercicio. excecao.contas” com os seguintes atributos:

{% highlight java %}
private double saldo; // Determina o saldo da conta.
private double limite; // Determina o limite de crédito do cheque especial.
{% endhighlight %}

E os métodos:

{% highlight java %}
// Construtor padrão da classe. 
public ContaBancaria(double valorSaldo, double valorLimite){}

// Retorna o saldo da conta.
public double getSaldo(){}

// Retorna o limite da conta.
protected double getLimite(){}

// Retorna o saldo da conta somado ao limite.
public double getSaldoComLimite(){}

// Deve decrementar o valor do saque da Conta. Retorna “true” caso a operação tenha sido bem sucedida, ou seja, a conta possui este valor (lembre-se de considerar o limite).
public boolean sacar(double valor) throws ContaException {}

// Deve incrementar o valor a Conta. 
public void depositar(double valor) throws ContaException {}
{% endhighlight %}

E crie também a seguinte classe 

public classe ContaException extends Exception {
	// Construtor padrão da classe. 
	public ContaException (String _mensagem) {
		// Deve chamar o construtor da superclasse.
	}
}
{% endhighlight %}

Requisitos
A sua classe conta bancária deve permitir apenas saques inferiores a R$ 500,00 ou que não façam com que a soma entre o saldo e o limite da conta resultem em um valor menor do que zero. Caso estas condições não se cumpram, deve ser lançada uma **ContaException** com uma mensagem que identifique o tipo de erro. 
A conta não deve permitir depósitos superiores a R$ 1.000,00. Caso esta condição não se cumpra, deve ser lançada uma **ContaException** com uma mensagem que identifique o tipo de erro. 

Crie uma classe para testar a classe ContaBancaria