---
layout: article
title: "TDD - Introdução"
categories: tdd
author: sakurai
date: 2020-05-19 23:00:00
tags: [java, tdd, testes]
published: true
excerpt: O foco do Desenvolvimento Guiado por Testes (Test Driven Development - TDD) são os testes unitário, em que são testadas pequenas partes da aplicação e no final ter uma cobertura ampla da aplicação através dos testes.
comments: true
image:
  teaser: teaser-java.png
ads: false
---

## Introdução ao desenvolvimento guiado por testes

Antes de começarmos a programar, precisamos ter em mente o que precisa ser desenvolvido, para isso, podemos por exemplo: montar uma lista de itens ou funcionalidades que são necessárias ou elaborar um algoritmo do programa, e porque não aproveitamos esse momento para testar o sistema?

Mas como vamos testar uma aplicação que nem começamos a desenvolver? O que é um teste? O que eu tenho que testar?

No desenvolvimento de software, precisamos testar as aplicações para verificar se estão funcionado de acordo com o que foi solicitado. Nem sempre é possível testar 100% da aplicação, as vezes o programador confiando nas suas habilidades acaba acreditando que é perfeito e que não comete erros, ou por uma falta de conhecimento ou experiência com o desenvolvimento, acaba esquecendo de testar corretamente alguma funcionalidade, ou o prazo para desenvolvimento está muito curto e não há tempo para testar totalmente a aplicação. Existem diversos fatores que atrapalham a execução dos testes.

Há diversas técnicas de testes do software, como:

* **Teste do sistema** - no qual, após o termino do desenvolvimento da aplicação, um grupo de usuários começam a utilizar as funcionalidades do sistema, para verificar o seu comportamento;
* **Teste de integração** - em que os programadores testam as integrações entre os componentes da aplicação;
* **Teste de aceitação** - em que usuários finais da aplicação, verificam se a aplicação foi desenvolvida de acordo com o que foi solicitado.

O foco do Desenvolvimento Guiado por Testes (*Test Driven Development - TDD*) são os **testes unitário**, em que são testadas pequenas partes da aplicação e no final ter uma cobertura ampla da aplicação através dos testes.

Garantindo que cada pequena parte da aplicação está funcionando como deveria, podemos ter um controle melhor das alterações, que estão sendo feitas dentro da aplicação. Após uma alteração na aplicação, podemos executar todos os testes unitários criados, verificando se não houve alteração em nenhuma outra parte da aplicação, pois caso algum outro teste unitário da aplicação falhe, sabemos que foi causado pela alteração atual.

Os testes podem ser utilizados como documentação do sistema, dado que podemos definir nomes informativos para cada teste, eles também podem dar mais confiança durante o desenvolvimento, já que se algum erro acontecer ou estragar alguma funcionalidade que está funcionando, os testes falharão.

> Desenvolvimento guiado por testes é um conjunto de técnicas que qualquer engenheiro de software pode seguir, que encoraja projetos simples e conjuntos de testes que inspiram confiança." (Kent Beck, 2010)

Utilizando as técnicas do desenvolvimento guiado por testes (TDD) será necessário dedicar um tempo pensando no que deve ser testado antes que possa realmente colocar a mão na massa e sair implementando um monte de código. O TDD te guiará a desenvolver um código limpo evitando diversos problemas que nós programadores temos no dia-a-dia.

## Três leis do TDD

Existem três leis no TDD definidas por Martin Fowler (2004) que servem como guia durante o processo de desenvolvimento:

1. Você não pode escrever código de produção a menos que ele tenha um teste de unidade que falha;
2. Você não pode escrever mais teste de unidade que o suficiente para falhar; e erros de compilação são falhas;
3. Você não pode escrever mais que o suficiente, para o código de produção passar em um teste de unidade.

Durante o desenvolvimento usando TDD deve-se ter em mente essas três leis, porque desta forma você será levado a testar sempre se a aplicação está funcionando e caso ocorra alguma falha saberá que nas poucas linhas de código que foram alterados encontra-se o problema que causou uma falha nos testes.

## Ciclo do desenvolvimento com TDD

A Figura 01 apresenta o ciclo de desenvolvimento do TDD, no qual o desenvolvedor deve criar um teste unitário que irá falhar, logo em seguida deverá implementar o mínimo necessário para que este teste unitário funcione e por último deve ser feito uma refatoração na implementação para remover qualquer código duplicado.

<figure>
    <a href="/images/2020-05-19-tdd-introducao-01.png"><img src="/images/2020-05-19-tdd-introducao-01.png" alt="Ciclo de desenvolvimento do TDD."></a>
</figure>

Figura 01 - O ciclo fundamental do TDD adaptado de Steve Freeman e Nat Price (2009).

Escrever um teste unitário que falha permitirá ao desenvolvedor ter um objetivo rápido: fazer o teste funcionar implementando seu código.

Fazer o teste funcionar implementando o mínimo necessário irá mostrar para o desenvolvedor que ele deve implementar apenas o suficiente para o teste funcionar sem ficar perdendo tempo em implementar algo que possa ocorrer no futuro.

Refatorar irá melhorar a qualidade do código, pois algumas vezes a implementação mínima para fazer o teste funcionar possui muitos códigos duplicados ou não utilizou da melhor forma possível os conceitos da orientação a objetos.

O objetivo final deste ciclo é que a aplicação possa ser implementada com um código limpo, com boas utilizações do conceito de orientação a objetos e permitindo que uma fácil manutenção e expansão da aplicação.

## O que é Teste Unitário?

Teste unitário é um teste de uma pequena parte da aplicação, normalmente é testado se um método (as vezes pode ser mais que um método) está funcionando corretamente.

Garantindo que cada pequena parte da aplicação está funcionando como deveria, podemos ter um controle melhor das alterações que estão sendo feitas dentro da aplicação. Após uma alteração na aplicação podemos executar todos os testes unitários criados, verificando se não houve alteração em nenhuma outra parte da aplicação, pois caso algum outro teste unitário da aplicação falhe sabemos que foi causado pela alteração atual.

Os testes podem ser utilizados como documentação do sistema, dado que podemos definir nomes informativos para cada teste, eles também podem dar mais confiança durante o desenvolvimento, já que se algum erro acontecer ou impactar outra funcionalidade do sistema algum teste pode falhar.


### Conteúdos relacionados

- [Começando a criar testes passo a passo](http://www.universidadejava.com.br/tdd/tdd-ola-testes/)
- [Entendendo o ciclo de desenvolvimento com TDD](http://www.universidadejava.com.br/tdd/tdd-ciclo-desenvolvimento/)
- [Introdução a refatoração](http://www.universidadejava.com.br/tdd/tdd-refatoracao/)
- [O impacto do TDD no design da aplicação](http://www.universidadejava.com.br/tdd/tdd-design-aplicacao/)