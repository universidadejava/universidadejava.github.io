---
layout: article
title: "JPA - CascadeType"
categories: jee
author: sakurai
date: 2011-02-22 18:49:00
tags: [javaee, jpa]
published: true
excerpt: Veja como especificar o relacionamento entre as entidades.
comments: true
image:
  teaser: teaser-jpa.png
ads: false
---

## javax.persistence.CascadeType

Com o CascadeType podemos definir a forma como serão propagadas as operações em cascata de uma Entity para suas referencias.

### PERSIST
Quando salvar a Entidade A, também será salvo todas as Entidades B associadas.

### MERGE
Quando atual as informações da Entidade A, também será atualizado no banco de dados todas as informações das Entidades B associadas.

### REMOVE
Quando remober a Entidade A, também será removida todas as entidades B associadas.

### REFRESH
Quando houver atualização no banco de dados na Entidade A, todas as entidades B associadas serão atualizadas.

### ALL
Corresponde a todas as operações acima (MERGE, PERSIST, REFRESH e REMOVE).


## Conteúdos relacionados

- [Definindo o FetchType do relacionamento entre entidades](http://www.universidadejava.com.br/jee/jpa-fetchtype/)
- [Exercícios com JPA](http://www.universidadejava.com.br/jee/jpa-exercicios-03/)
- [Relacionamento de Um-para-Muitos entre entidades](http://www.universidadejava.com.br/jee/jpa-onetomany/)
- [Exemplo de CRUD com JPA](http://www.universidadejava.com.br/jee/jpa-exemplo-crud/)