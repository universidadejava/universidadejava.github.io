---
layout: article
title: "Conhecendo e previnindo a vulnerabilidade Cross-site scripting"
categories: outros
author: sakurai
date: 2020-05-24 16:47:00
tags: [java, vulnerabilidade, sql, injection]
published: true
excerpt: Cross-site scripting (XSS) é uma forma de inserir códigos maliciosos que não tratam os conteúdos que são gerados dinamicamente pelos usuários.
comments: true
image:
  teaser: 2020-05-24-teaser-vulnerabilidade-xss.png
ads: false
---

## Cross-site scripting

Cross-site scripting (XSS) é uma forma de inserir códigos maliciosos que não tratam os conteúdos que são gerados dinamicamente pelos usuários.

Os ataques XSS podem ser escritos em várias linguagens: HTML, JavaScript, VBScript, ActionScript e outros.

> “Vulnerabilidades de XSS vem desde 1996 no início da Web. Um tempo onde não existiam grandes aplicações comerciais e milhares de páginas eram escritas apenas com HTML e a linguagem de programação JavaScript entrou em cena, prenúncio de um desconhecido código dinâmico para páginas web, que mudou a segurança das aplicações para sempre.” (FOGIE; GROSSMAN; HANSEN; PETKOV; RAGER, 2007, p. 2).

Principais técnicas / formas de ataque:
- Ataque XSS de reflexão
- Ataque XSS de armazenamento
- Ataque XSS baseado em DOM

### Ataque XSS de reflexão

Usado para verificar se o site possui a vulnerabilidade XSS. Apenas o usuário que está realizado o XSS verá o resultado.

Para identificar a vulnerabilidade basta usar algum campo de entrada e verificar se permite a execução de scripts, exemplo:

{% highlight javascript %}
<script>alert('Teste XSS');</script>
{% endhighlight %}

Esse teste consiste em verificar se a aplicação executará o script.

Outra forma de testar a vulnerabilidade de XSS é por meio dos parâmetros passados via URL, exemplo:

{% highlight javascript %}
www.meusite.com.br?parametro=<script>alert('Teste XSS');</script>
{% endhighlight %}


### Ataque XSS de armazenamento

A aplicação permite entradas de dados com scripts. Os scripts são armazenados no banco de dados da aplicação. A cada vez que os dados contendo scripts são consultados eles são executados.


### Ataque XSS baseado em DOM

A aplicação permite entrada de dados com scripts. Esses scripts manipulam o Document Object Model (DOM) da página. Pode ser usado para incluir ou remover informações na página.

O XSS também permite a manipulação do DOM da página html, exemplo:

{% highlight javascript %}
<body onload="javascript=alert('Teste XSS')";>
{% endhighlight %}


## Como previnir

Como prevenir ou garantir a segurança?
- Técnicas de Sanitização (verificação de entrada, seleção indireta, cleansing, conversão, ferramentas)
- Constante atualizações de software (Patches/Fixes)
- Ferramentas especializadas (WAF, Proxys, IDS)
- Auditoria de código (processo + ferramentas)
- Testes de invasão (pessoas + ferramentas)
- Aplicação de boas práticas de configuração e design de software
- Gestão de conhecimento
- Comprometimento de alta gestão

## Previnindo a vulnerabilidade XSS na sua aplicação web Java

Em uma aplicação que utiliza páginas JSP, quando imprimimos os dados diretamente com:

{% highlight java %}
${meuBean.atributo}
{% endhighlight %}

Estamos vulneráveis ao XSS, a tag `c:out` possui e escape de caracteres que impede o ataque com XSS:

<c:out value="${meuBean.atributo}" escapeXml="true" />

Se sua aplicação usa JSF, por padrão os campos de saída aplicam o escape nos caracteres recebidos. Mas pode ser manualmente desabilitado para algum campo que permita formações com HTML ou scripts:

{% highlight java %}
<h:outputText value="#{meuMB.atributo}" escape="false" />
{% endhighlight %}

Ficando vulnerável ao ataque de XSS.


## Referências

[FOGIE, S.; GROSSMAN, J.; HANSEN, R.; PETKOV, P. D.; RAGER, A.] **XSS Attacks: Cross site scripting exploits and defense.** United States of America: Syngress, 2007.


## Conteúdos relacionados

- [Conhecendo e previnindo a vulnerabilidade SQL Injection](http://www.universidadejava.com.br/outros/vulnerabilidade-sql-injection)
- [Introdução a criptografia](http://www.universidadejava.com.br/outros/introducao-criptografia/)
- [Criptografia simétrica utilizando chave privada](http://www.universidadejava.com.br/outros/criptografia-simetrica/)
- [Criptografia assimétrica utilizando um par de chaves pública/privada](http://www.universidadejava.com.br/outros/criptografia-assimetrica/)