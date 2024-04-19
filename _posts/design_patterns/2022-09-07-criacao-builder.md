---
layout: media
title: "Padrão de criação - Builder"
categories: design_patterns
author: cristiano
date: 2021-09-07 02:00:00
tags: [java, design patterns, builder]
published: true
excerpt: Exemplo do padrão de projeto Builder em Java.
comments: true
image:
  teaser: 2022-09-07-teaser-design-pattern-builder.png
ads: false
---

> "Separar a construção de um objeto complexo da sua representação, para que o mesmo processo de construção possa criar diferentes representações" (GAMMA, E. et al, pág. 97)

public static class Builder {
    private SalaMultimidia salaMultimidia;

    public Builder() {
      this.salaMultimidia = new SalaMultimidia();
    }
    public Builder comNome(String nome) {
      this.salaMultimidia.nome = nome;
      return this;
    }
    public Builder comCapacidade(int capacidade) {
      this.salaMultimidia.capacidade = capacidade;
      return this;
    }
    public Builder noLocal(Local local) {
      this.salaMultimidia.local = local;
      return this;
    }
    public Builder temProjetor(boolean temProjetor) {
      this.salaMultimidia.temProjetor = temProjetor;
      return this;
    }
    public Builder aPortaEstaAberta(boolean portaAberta) {
      this.salaMultimidia.portaAberta = portaAberta;
      return this;
    }
    public SalaMultimidia build() {
      return this.salaMultimidia;
    }
}

Podemos colocar restrição para criação da sala multimídia:

public SalaMultimidia build() {
  if(this.salaMultimidia.nome == null) 
    throw new IllegalArgumentException("Nome da sala obrigatório!");

  if(this.salaMultimidia.capacidade <= 0) 
    throw new IllegalArgumentException("Capacidade da sala deve ser maior que zero!");

  if(this.salaMultimidia.local == null)
    throw new IllegalArgumentException("O local da sala é obrigatório!");

  return this.salaMultimidia;
}

Vantagens:
- Encapsula a complexidade da criação dos objetos
- Permite criar os objetos em partes
- Podemos esconder a representação interna do objeto

Desvantagem:
- O cliente precisa conhecer o domínio para realizar criação do objeto
- Pode ser necessário manter também o padrão get e set do JavaBean