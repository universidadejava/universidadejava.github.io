---
layout: article
title: "Web Services com EJB 3.0"
categories: materiais
author: sakurai
date: 2011-03-04 08:48:00
tags: [javaee, webservice, ejb]
published: true
excerpt: Introdução a Web Services a partir de componentes EJB.
comments: true
image:
  teaser: 2011-03-04-teaser-webservice-com-ejb.png
ads: true
---

Serviço Web (Web Service) é uma forma padronizada para integração entre aplicações diferentes.

Através do Web Service (WS) é possível integrar aplicações desenvolvidas com linguagens de programação diferente, pois o Web Service utiliza a troca de arquivos XML para realizar a comunicação.

Quando desenvolvemos uma aplicação com WS precisamos informar para quem for utilizar (consumir) o WS como ele funciona, para isso criamos um arquivo WSDL e enviamos este arquivo para quem for criar o cliente para o WS, após o cliente ser criado a comunicação feita entre as aplicações utiliza envelopes SOAP no formato XML.

<figure>
    <a href="/images/2011-03-04-webservice-com-ejb-01.png"><img src="/images/2011-03-04-webservice-com-ejb-01.png" alt="Web Services."></a>
</figure>

## Web Service Description Language (WSDL)

WSDL é um documento no padrão XML que descreve o funcionamento de um Web Service.

Neste documento podemos descrever:

* Onde encontrar o Web Service.
* Quais métodos possuem esse Web Service.
* Quais parâmetros os métodos recebem.
* O que os métodos retornam.

Exemplo de WSDL:

Neste exemplo temos o WSDL de um Web Service chamado **ExemploWS** que possui o método **olaMundo** que não recebe parametro e retorna uma String.

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<!-- Generated by JAX-WS RI at http://jax-ws.dev.java.net. RI's version is JAX-WSRI 2.1.3.1-hudson-749-SNAPSHOT. -->
<definitions targetNamespace="http://ws.exemplo.pbc/" name="ExemploWSService" xmlns="http://schemas.xmlsoap.org/wsdl/" xmlns:tns="http://ws.exemplo.pbc/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
  <ns1:Policy wsu:Id="ExemploWSPortBinding_olaMundo_WSAT_Policy" xmlns:ns1="http://www.w3.org/ns/ws-policy">
    <ns1:ExactlyOne>
      <ns1:All>
        <ns2:ATAlwaysCapability xmlns:ns2="http://schemas.xmlsoap.org/ws/2004/10/wsat"/>
        <ns3:ATAssertion ns1:Optional="true" ns4:Optional="true" xmlns:ns4="http://schemas.xmlsoap.org/ws/2002/12/policy" xmlns:ns3="http://schemas.xmlsoap.org/ws/2004/10/wsat"/>
      </ns1:All>
    </ns1:ExactlyOne>
  </ns1:Policy>
  <types>
    <xsd:schema>
      <xsd:import namespace="http://ws.exemplo.pbc/" schemaLocation="ExemploWSService_schema1.xsd"/>
    </xsd:schema>
  </types>
  <message name="olaMundo">
    <part name="parameters" element="tns:olaMundo"/>
  </message>
  <message name="olaMundoResponse">
    <part name="parameters" element="tns:olaMundoResponse"/>
  </message>
  <portType name="ExemploWS">
    <operation name="olaMundo">
      <input message="tns:olaMundo"/>
      <output message="tns:olaMundoResponse"/>
    </operation>
  </portType>
  <binding name="ExemploWSPortBinding" type="tns:ExemploWS">
    <soap:binding transport="http://schemas.xmlsoap.org/soap/http" style="document"/>
    <operation name="olaMundo">
      <ns5:PolicyReference URI="#ExemploWSPortBinding_olaMundo_WSAT_Policy" xmlns:ns5="http://www.w3.org/ns/ws-policy"/>
      <soap:operation soapAction=""/>
      <input>
        <soap:body use="literal"/>
      </input>
      <output>
        <soap:body use="literal"/>
      </output>
    </operation>
  </binding>
  <service name="ExemploWSService">
    <port name="ExemploWSPort" binding="tns:ExemploWSPortBinding">
      <soap:address location="REPLACE_WITH_ACTUAL_URL"/>
    </port>
  </service>
</definitions>
{% endhighlight %}

## Simple Object Access Protocol (SOAP)

SOAP é um padrão com tags XML utilizado para enviar requisições e receber respostas de um Web Service.

Exemplo de mensagem SOAP:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
  <S:Header/>
  <S:Body>
    <ns2:olaMundo xmlns:ns2="http://ws.exemplo.pbc/"/>
  </S:Body>
</S:Envelope>
{% endhighlight %}

Exemplo de resposta SOAP:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<S:Envelope xmlns:S="http://schemas.xmlsoap.org/soap/envelope/">
  <S:Body>
    <ns2:olaMundoResponse xmlns:ns2="http://ws.exemplo.pbc/">
    <return>Ola Mundo!!!</return>
    </ns2:olaMundoResponse>
  </S:Body>
</S:Envelope>
{% endhighlight %}
