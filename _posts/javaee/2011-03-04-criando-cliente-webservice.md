---
layout: article
title: "JAX-WS - Criando cliente para Web Service"
categories: javaee
author: sakurai
date: 2011-03-04 09:14:00
tags: [javaee, jaxws, webservice, ejb]
published: true
excerpt: Criando uma aplicação que consome um Web Services.
comments: true
image:
  teaser: 2011-03-04-teaser-criando-cliente-webservice.png
ads: true
---

O NetBeans possui uma maneira automática para geração de clientes de web services, para isto basta termos o WSDL do WS, no exemplo abaixo vamos criar um cliente para o WS de Ola Mundo.

Crie um projeto de Aplicativo Java chamado, por exemplo: **ExemploWSCliente**.

Para gerar um cliente para o web service, clique com o botão direito em cima de Pacotes de código-fonte, selecione Novo e clique em **Cliente para serviço Web...**.

<figure>
    <a href="/images/2011-03-04-criando-cliente-webservice-01.png"><img src="/images/2011-03-04-criando-cliente-webservice-01.png" alt="Criando um cliente de Web Service."></a>
</figure>

Ou clique em **Novo** e selecione **Outro...**, na tela de Novo arquivo selecione a categoria **Serviços Web** e marque a opção **Cliente para serviço web**.

<figure>
    <a href="/images/2011-03-04-criando-cliente-webservice-02.png"><img src="/images/2011-03-04-criando-cliente-webservice-02.png" alt="Criando um cliente de Web Service."></a>
</figure>

Para especificar o Web Service que será gerado o cliente, podemos fazer isso de duas formas:

- Quando conhecemos o projeto do Web Service:

Na tela de **Novo Cliente para Serviço Web** selecione a opção **Projeto** e clique em **Procurar...**, irá aparecer uma tela onde podemos encontrar os web services criados nos projetos que temos aberto no NetBeans, selecione o Web Service desejado e clique em **OK**.

<figure>
    <a href="/images/2011-03-04-criando-cliente-webservice-03.png"><img src="/images/2011-03-04-criando-cliente-webservice-03.png" alt="Criando um cliente de Web Service."></a>
</figure>

Informe o **Pacote** onde será gerado o código do cliente do serviço web e clique em **Finalizar**.

<figure>
    <a href="/images/2011-03-04-criando-cliente-webservice-04.png"><img src="/images/2011-03-04-criando-cliente-webservice-04.png" alt="Criando um cliente de Web Service."></a>
</figure>

- Quando o Web Service é desenvolvido por outra pessoa e temos apenas o URL do WSDL:

Na tela de **Novo Cliente para Serviço Web** selecione a opção **WSDL URL** e informe a url do WSDL. Informe o **Pacote** onde será gerado o código do cliente do serviço web e clique em **Finalizar**.

<figure>
    <a href="/images/2011-03-04-criando-cliente-webservice-05.png"><img src="/images/2011-03-04-criando-cliente-webservice-05.png" alt="Criando um cliente de Web Service."></a>
</figure>

Após criar o cliente para o serviço web podemos testa-lo, para isto vamos criar uma classe para chamar o Web Service:

{% highlight java %}
package pbc.exemplo.ws.cliente;

import pbc.exemplo.ws.ExemploWS;
import pbc.exemplo.ws.ExemploWSService;

/**
 * Classe utilizada para testar o web service.
 * @author Rafael Guimarães Sakurai
 */
public class TesteWS {
  public static void main(String[] args) {
    ExemploWSService service = new ExemploWSService();
    ExemploWS port = service.getExemploWSPort();

    System.out.println(port.olaMundo());
  }
}
{% endhighlight %}

Quando executarmos esta classe teremos a saída no console:

{% highlight java %}
Ola Mundo!!!
{% endhighlight %}
