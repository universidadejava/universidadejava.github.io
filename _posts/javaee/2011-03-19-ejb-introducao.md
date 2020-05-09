---
layout: article
title: "EJB - Introdução ao Enterprise JavaBeans 3.0"
categories: javaee
author: sakurai
date: 2011-03-19 11:32:00
tags: [javaee, ejb]
published: true
excerpt: Introdução ao EJB.
comments: true
image:
  teaser: teaser-ejb.png
ads: true
---

## Enterprise JavaBeans 3.0 (EJB)

EJB é um framework que simplifica o processo de desenvolvimento de componentes para aplicações distribuídas em Java.

Utilizando EJB o desenvolvedor pode utilizar mais tempo desenvolvendo lógica de negocio, preocupando-se menos com transação, persistência de dados, serviços de rede e outros.

## Container EJB

Para utilizarmos EJB dentro do Servidor Web, é necessário ter um container EJB dentro do Servidor Web, pois é este container que irá gerenciar todo o ciclo de vida de um EJB.

Exemplos de Servidores de Aplicação Web que suportam EJB 3.0: JBoss (Wildfly), GlassFish, WebSphere, etc.

Quando nossa aplicação utiliza o container EJB, este disponibiliza diversos serviços para a aplicação EJB como, por exemplo:

* Gerenciamento de transação;
* Segurança;
* Controle de concorrência;
* Serviço de rede;
* Gerenciamento de recursos;
* Persistência de dados;
* Serviço de mensageria.

## Tipos de EJB

Existe três tipos de EJB que podemos utilizar, cada um com sua finalidade:

* Session Beans – Utilizado para guardar a lógica de negocio da aplicação.
* Message-Driven Bean – Utilizado para troca de mensagens.
* Entity Bean (até versão EJB 2.x) – Utilizado para representar as tabelas do banco de dados.
