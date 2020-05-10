---
layout: article
title: "Java - exercícios sobre métodos"
categories: java
author: sakurai
date: 2020-05-09 23:25:00
tags: [java]
published: true
excerpt: Exercícios sobre métodos.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Exercícios sobre métodos

1. Crie uma classe chamada **Calculadora**. Esta classe deve possuir os seguintes métodos:

**public double operacao(double num1, double num2);**
Retorna a soma dos dois números.

**public double operacao(int num1, double num2);**
Retorna a subtração dos dois números.

**public double operacao(double num1, int num2);**
Retorna o produto dos dois números.

**public double operacao(int num1, int num2);**
Retorna o resultado da divisão dos dois números.

**public double operacao(int num1, short num2);**
Retorna o resto da divisão dos dois números.

Elabore um roteiro de teste para a sua calculadora e observe os resultados.


2. Crie uma classe chamada **Pessoa** sem atributo algum. Para trabalharmos os conceitos de sobrecarga de métodos, crie os seguintes métodos:

**public String dizerInformacao(String nome);**
Deve retornar um texto dizendo: " Meu nome é " + nome .

**public String dizerInformacao(int idade);**
Deve retornar um texto dizendo: " Minha idade é " + idade .

**public String dizerInformacao(double peso, double altura);**
Deve retornar um texto dizendo: " Meu peso é " + peso +  " e minha altura é " + altura .

Munido do retorno de cada um destes métodos. Imprima-o em tela. Para praticarmos o uso da classe Scanner, leia estas quatro informações que devem ser inseridas pelo usuário.


3. Crie as classes e implemente os métodos para o seguinte diagrama de classe:

<figure>
    <a href="/images/2020-05-09-java-exercicios-metodos-ex03.png"><img src="/images/2020-05-09-java-exercicios-metodos-ex03.png" alt="Exercício 3."></a>
</figure>

Crie uma classe de teste e elabore um roteiro de teste da suas classes.


4. Crie uma classe Conta com os seguintes métodos e atributos:

<figure>
    <a href="/images/2020-05-09-java-exercicios-metodos-ex04.png"><img src="/images/2020-05-09-java-exercicios-metodos-ex04.png" alt="Exercício 4."></a>
</figure>

No método depositar por questões de segurança, não deve permitir depósitos superiores a R$ 1.000,00 caso a conta seja do tipo "Corrente". 

O método sacar não deve permitir saques superiores ao saldo total da conta.

O método transferir, pode realizar transferência da conta corrente para a conta poupança, mas o contrario não é permitido, também não deve permitir que o saldo da conta fique negativo.

Feito isso, trace o seguinte roteiro:
- Crie duas contas, uma do tipo "Corrente" e outra do tipo "Poupança" com um saldo inicial qualquer.
- Tente depositar R$ 1.500,00 reais na conta corrente.
- Tente depositar R$ 1.500,00 reais na conta poupança.
- Deposite R$ 98,52 na conta poupança.
- Tente sacar R$ 100,00 da poupança.
- Transfira R$ 1.800,00 da corrente para a conta poupança.
- Transfira R$ 700,00 da poupança para a conta corrente.
- Saque R$ 1.000,00 da poupança.
- Saque R$ 1.000,00 da corrente.

Na classe de teste, exiba mensagens indicando o retorno e ou o resultado (êxito ou falha) de cada um dos métodos.