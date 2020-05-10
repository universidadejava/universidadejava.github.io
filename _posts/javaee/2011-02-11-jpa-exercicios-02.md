---
layout: article
title: "JPA - Lista de exercícios 02"
categories: javaee
author: sakurai
date: 2011-02-22 12:30:00
tags: [javaee, jpa]
published: true
excerpt: Exercícios de JPA para praticar.
comments: true
image:
  teaser: 2011-02-11-teaser-jpa-exercicios-02.png
ads: false
---

## Exercício 1

Neste exercício vamos abordar como funciona os relacionamentos entre as entidades, vamos utilizar o relacionamento entre as entidades Cliente e Endereco, quando salvar o cliente também deve salvar o endereço e quando o cliente for consultado deve trazer também as informações do endereço.

Crie um projeto Java chamado **ExercicioJPA4**, adicione as bibliotecas **Hibernate JPA** e **Driver do Oracle ojdbc7.jar** (neste exercício estou usando o driver do Oracle, se quiser faça com outro banco de dados, só lembre de alterar o arquivo persistence.xml e a forma de geração do ID das entidades) e crie:

* Classe entity para representar um endereço com os atributos id, estado, cidade, bairro, logradouro e complemento.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.modelo;

import java.io.Serializable;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.SequenceGenerator;

/**
 * Classe utilizada para representar um Endereço.
 */
@Entity
@SequenceGenerator(name = "ENDERECO_SEQ", sequenceName = "END_SEQ", initialValue = 1, allocationSize = 1)
public class Endereco implements Serializable {
  private static final long serialVersionUID = 5331450149454053703L;

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "ENDERECO_SEQ")
  private Long id;
  private String estado;
  private String cidade;
  private String bairro;
  private String logradouro;
  private String complemento;

  public String getBairro() { return bairro; }
  public void setBairro(String bairro) { this.bairro = bairro; }
  public String getCidade() { return cidade; }
  public void setCidade(String cidade) { this.cidade = cidade; }
  public String getComplemento() { return complemento; }
  public void setComplemento(String complemento) {
    this.complemento = complemento;
  }
  public String getEstado() { return estado; }
  public void setEstado(String estado) { this.estado = estado; }
  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public String getLogradouro() { return logradouro; }
  public void setLogradouro(String logradouro) { this.logradouro = logradouro; }
}
{% endhighlight %}

* Classe entity para representar um cliente com id, nome e endereço, note que vamos utilizar a anotação javax.persistence.OneToOne para definir o relacionamento de um-para-um entre as entidades Cliente e Endereco.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.modelo;

import java.io.Serializable;
import javax.persistence.CascadeType;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToOne;
import javax.persistence.SequenceGenerator;

/**
 * Classe utilizada para representar um Cliente.
 */
@Entity
@SequenceGenerator(name = "CLIENTE_SEQ", sequenceName = "CLI_SEQ", initialValue = 1, allocationSize = 1)
public class Cliente implements Serializable {
  private static final long serialVersionUID = 4521490124826140567L;

  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "CLIENTE_SEQ")
  private Long id;
  private String nome;
  @OneToOne(cascade=CascadeType.ALL)
  private Endereco endereco;

  public Endereco getEndereco() { return endereco; }
  public void setEndereco(Endereco endereco) { this.endereco = endereco; }
  public Long getId() { return id; }
  public void setId(Long id) { this.id = id; }
  public String getNome() { return nome; }
  public void setNome(String nome) { this.nome = nome; }
}
{% endhighlight %}

* Crie o banco de dados para guardar os dados de Cliente e Endereco:

{% highlight sql %}
CREATE SEQUENCE CLI_SEQ INCREMENT BY 1 START WITH 1 NOCACHE NOCYCLE;
CREATE SEQUENCE END_SEQ INCREMENT BY 1 START WITH 1 NOCACHE NOCYCLE;

CREATE TABLE ENDERECO (
  id number(5) NOT NULL PRIMARY KEY,
  estado VARCHAR2(50) NOT NULL,
  cidade VARCHAR2(50) NOT NULL,
  bairro VARCHAR2(50) NOT NULL,
  logradouro VARCHAR2(50) NOT NULL,
  complemento VARCHAR2(50) NOT NULL
);

CREATE TABLE CLIENTE (
  id number(5) NOT NULL PRIMARY KEY,
  nome VARCHAR2(100) NOT NULL,
  endereco_id number(5) NOT NULL
);
{% endhighlight %}

* Crie o arquivo persistence.xml dentro da pasta META-INF do projeto, note que neste arquivo vamos informar qual o banco de dados iremos utilizar e quais classes são entidades do banco:

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="1.0" xmlns="http://java.sun.com/xml/ns/persistence"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

  <persistence-unit name="ExercicioJPA1PU" transaction-type="RESOURCE_LOCAL">
    <provider>org.hibernate.ejb.HibernatePersistence</provider>
    <class>br.universidadejava.jpa.exercicio1.modelo.Endereco</class>
    <class>br.universidadejava.jpa.exercicio1.modelo.Cliente</class>
    <properties>
      <property name="hibernate.connection.username" value="usuario"/>
      <property name="hibernate.connection.password" value="senha"/>
      <property name="hibernate.connection.driver_class" value="oracle.jdbc.driver.OracleDriver"/>
      <property name="hibernate.connection.url" value="jdbc:oracle:thin:@ipDoBanco:1521:nomeDaBase"/>
      <property name="hibernate.cache.provider_class" value="org.hibernate.cache.NoCacheProvider"/>
      <property name="hibernate.dialect" value="org.hibernate.dialect.Oracle9Dialect"/>
      <property name="hibernate.show_sql" value="true"/>
    </properties>
  </persistence-unit>

</persistence>
{% endhighlight %}

* Crie a classe **ClienteDAO** que será responsável por utilizar o **EntityManager** para manipular (salvar, alterar, remover e consultar por id) as informações referentes ao Cliente.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.dao;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;
import br.universidadejava.jpa.exercicio1.modelo.Cliente;

/**
 * Classe utilizada para fazer as operações de banco de dados sobre a entity Cliente.
 */
public class ClienteDAO {

  /**
   * Método utilizado para obter o entity manager.
   * @return
   */
  private EntityManager getEntityManager() {
    EntityManagerFactory factory = null;
    EntityManager entityManager = null;
    try {
      //Obtem o factory a partir da unidade de persistencia.
      factory = Persistence.createEntityManagerFactory("ExercicioJPA4PU");
      //Cria um entity manager.
      entityManager = factory.createEntityManager();
    } catch (Exception ex) {
      ex.printStackTrace();
    } finally {
      factory.close();
    }
    return entityManager;
  }

  /**
   * Método que salva ou atualiza as informações do cliente.
   * @param cliente
   * @return
   * @throws java.lang.Exception
   */
  public Cliente salvar(Cliente cliente) throws Exception {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      System.out.println("Salvando as informações do cliente.");
      // Verifica se o cliente ainda não está salvo no banco de dados.
      if(cliente.getId() == null) {
        entityManager.persist(cliente);
      } else {
        cliente = entityManager.merge(cliente);
      }
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } catch(Exception ex) {
      ex.printStackTrace();
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
    // Retorna o cliente salvo.
    return cliente;
  }

  /**
   * Método que apaga as informações do cliente do banco de dados.
   * @param id
   */
  public void apagar(Long id) {
    EntityManager entityManager = getEntityManager();
    try {
      // Inicia uma transação com o banco de dados.
      entityManager.getTransaction().begin();
      // Consulta o cliente na base de dados através do seu ID.
      Cliente cliente = entityManager.find(Cliente.class, id);
      System.out.println("Excluindo o cliente: " + cliente.getNome());
      // Remove o cliente da base de dados.
      entityManager.remove(cliente);
      // Finaliza a transação.
      entityManager.getTransaction().commit();
    } catch(Exception ex) {
      ex.printStackTrace();
      entityManager.getTransaction().rollback();
    } finally {
      entityManager.close();
    }
  }

  /**
   * Consulta o cliente pelo ID.
   * @param id
   * @return
   */
  public Cliente consultarPorId(Long id) {
    EntityManager entityManager = getEntityManager();
    Cliente cliente = null;
    try {
      //Consulta o cliente pelo ID.
      cliente = entityManager.find(Cliente.class, id);
    } catch (Exception ex) {
      ex.printStackTrace();
    } finally {
      entityManager.close();
    }
    //Retorna o cliente consultado.
    return cliente;
  }
}
{% endhighlight %}

Vamos desenvolver a interface gráfica em SWING que será responsável por chamar o ClienteDAO para executar as operações no banco de dados.

<figure>
    <a href="/images/2011-02-11-jpa-exercicios-02-01.png"><img src="/images/2011-02-11-jpa-exercicios-02-01.png" alt="Exercício cadastro de cliente."></a>
</figure>

Código fonte da tela de cadastro do cliente.

> este código não está completo possui apenas implementado o código dos botões.

{% highlight java %}
package br.universidadejava.jpa.exercicio1.tela;

import javax.swing.JOptionPane;
import br.universidadejava.jpa.exercicio1.dao.ClienteDAO;
import br.universidadejava.jpa.exercicio1.modelo.Cliente;
import br.universidadejava.jpa.exercicio1.modelo.Endereco;

/**
 * Classe utilizada para representar o cadastro do Cliente.
 */
public class CadastroCliente extends javax.swing.JFrame {
  private static final long serialVersionUID = -6011351657657723638L;

  public CadastroCliente() {
    initComponents();
  }

  /**
   * Código para montar a tela.
   */
  @SuppressWarnings("unchecked")
  private void initComponents() {
    //Código que monta a tela mostrada na imagem anterior.
  }

  /**
   * Botão que salva as informações do cliente.
   * @param evt
   */
  private void botaoSalvarActionPerformed(java.awt.event.ActionEvent evt) {
    try {
      //Cria um objeto endereco;
      Endereco e = new Endereco();
      if(!this.id.isEditable()) {
        e.setId(new Long(this.id.getText()));
      }
      e.setEstado(this.estado.getText());
      e.setCidade(this.cidade.getText());
      e.setBairro(this.bairro.getText());
      e.setLogradouro(this.logradouro.getText());
      e.setComplemento(this.complemento.getText());

      //Cria um objeto cliente.
      Cliente c = new Cliente();
      c.setNome(this.nome.getText());
      c.setEndereco(e);

      //Salva o cliente.
      ClienteDAO dao = new ClienteDAO();
      c = dao.salvar(c);

      JOptionPane.showMessageDialog(this, "Cliente " + c.getId() + " - " + c.getNome(), "INFORMAÇÃO", JOptionPane.INFORMATION_MESSAGE);
      limparDados();
    } catch (Exception ex) {
      JOptionPane.showMessageDialog(this, ex.getMessage(), "ERRO", JOptionPane.ERROR_MESSAGE);
    }
  }

  /**
   * Botão que consulta as informações do cliente.
   * @param evt
   */
  private void botaoConsultarActionPerformed(java.awt.event.ActionEvent evt) {
    try {
      ClienteDAO dao = new ClienteDAO();
      Cliente c = dao.consultarPorId(Long.valueOf(this.id.getText()));

      if(c != null) {
        this.id.setEditable(false);
        this.nome.setText(c.getNome());
        this.estado.setText(c.getEndereco().getEstado());
        this.cidade.setText(c.getEndereco().getCidade());
        this.bairro.setText(c.getEndereco().getBairro());
        this.logradouro.setText(c.getEndereco().getLogradouro());
        this.complemento.setText(c.getEndereco().getComplemento());
      } else {
        limparDados();
        JOptionPane.showMessageDialog(this, "Cliente não foi encontrado!", "ERRO", JOptionPane.ERROR_MESSAGE);
      }
    } catch (NumberFormatException ex) {
      JOptionPane.showMessageDialog(this, "O campo código precisa ser um número inteiro", "ERRO", JOptionPane.ERROR_MESSAGE);
    }
  }

  /**
   * Botão para limpar as informações do formulario para cadastrar um
   * novo cliente.
   * @param evt
   */
  private void botaoNovoActionPerformed(java.awt.event.ActionEvent evt) {
    limparDados();
  }

  /**
   * Botão para remover as informações referentes a um cliente.
   * @param evt
   */
  private void botaoApagarActionPerformed(java.awt.event.ActionEvent evt) {
    try {
      ClienteDAO dao = new ClienteDAO();
      dao.apagar(Long.valueOf(this.id.getText()));
      limparDados();
      JOptionPane.showMessageDialog(this, "As informações do cliente foram apagadas do sistema.", "INFORMAÇÃO", JOptionPane.INFORMATION_MESSAGE);
    } catch (NumberFormatException ex) {
      JOptionPane.showMessageDialog(this, "O campo código precisa ser um número inteiro", "ERRO", JOptionPane.ERROR_MESSAGE);
    }
  }

  /**
   * Limpa os dados do formulario.
   */
  private void limparDados() {
    this.id.setText(null);
    this.id.setEditable(true);
    this.nome.setText(null);
    this.estado.setText(null);
    this.cidade.setText(null);
    this.bairro.setText(null);
    this.logradouro.setText(null);
    this.complemento.setText(null);
  }

  /**
   * @param args the command line arguments
   */
  public static void main(String args[]) {
    java.awt.EventQueue.invokeLater(new Runnable() {
      public void run() {
        new CadastroCliente().setVisible(true);
      }
    });
  }

  private javax.swing.JTextField bairro;
  private javax.swing.JButton botaoApagar;
  private javax.swing.JButton botaoConsultar;
  private javax.swing.JButton botaoNovo;
  private javax.swing.JButton botaoSalvar;
  private javax.swing.JTextField cidade;
  private javax.swing.JTextField complemento;
  private javax.swing.JTextField estado;
  private javax.swing.JTextField id;
  private javax.swing.JLabel jLabel1;
  private javax.swing.JLabel jLabel2;
  private javax.swing.JLabel jLabel3;
  private javax.swing.JLabel jLabel4;
  private javax.swing.JLabel jLabel5;
  private javax.swing.JLabel jLabel6;
  private javax.swing.JLabel jLabel7;
  private javax.swing.JLabel jLabel8;
  private javax.swing.JTextField logradouro;
  private javax.swing.JTextField nome;
}
{% endhighlight %}

##Exercício 2

Neste exercício vamos desenvolver uma aplicação Swing ou Console para implementar o seguinte requisito de sistema:
“Precisamos controlar as vendas de instrumentos musicais, desenvolva uma aplicação para cadastrar, alterar, consultar pelo código e remover os instrumentos musicais (marca, modelo e preço). Também devemos cadastrar as vendas feitas para um cliente, onde o cliente (nome, cpf e telefone) pode comprar diversos instrumentos musicais. Não temos a necessidade de controlar o estoque dos produtos, pois apenas será vendido os itens que estão nas prateleiras.”
