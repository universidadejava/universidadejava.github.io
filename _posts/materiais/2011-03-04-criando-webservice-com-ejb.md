---
layout: article
title: "JAX-WS - Criando Web Service com EJB 3.0"
categories: materiais
author: sakurai
date: 2011-03-04 09:01:00
tags: [javaee, jaxws, webservice, ejb]
published: true
excerpt: Introdução a Web Services com JAX-WS.
comments: true
image:
  teaser: 2011-03-04-teaser-criando-webservice-com-ejb.png
ads: true
---

Para criar um serviço web baseado no EJB 3.0, podemos utilizar a Java API for XML-based Web Services (JAX-WS), através dessa API podemos adicionar anotações ao EJB para que ele possa ser utilizado como um serviço web.

Nesse vídeo demonstro como criar um Web Service usando o JAX-WS:

<iframe width="420" height="315" src="https://www.youtube.com/embed/p2G08MDxCUs" frameborder="0" allowfullscreen></iframe>

Algumas das anotações que utilizamos para definir um web service:

**javax.jws.WebService**
Define uma classe como um WS (Está classe precisa ser um Stateless Session Bean).

**javax.jws.WebMethod**
Define que um método será disponibilizado via WS.

**javax.jws.WebParam**
Define os parâmetros que serão recebidos pelo WS.

**javax.jws.WebResult**
Define o nome da tag XML com o conteúdo do retorno, por padrão a tag de retorno do método chama <return>.

Exemplo de Web Service:

{% highlight java %}
@WebService()
@Stateless()
public class ExemploWS {
  @WebMethod(operationName = "olaMundo")
  public String olaMundo() {
    return "Ola Mundo!!!";
  }
}
{% endhighlight %}

Exemplo visual de um web service no NetBeans:

<figure>
    <a href="/images/2011-03-04-criando-webservice-com-ejb-01.png"><img src="/images/2011-03-04-criando-webservice-com-ejb-01.png" alt="Exemplo visual de Web Services no NetBeans."></a>
</figure>

No NetBeans após criar um Web Service e publica-lo no Glassfish, podemos testa-lo clicando com o botão direito do mouse no **nome do Web Service** (dentro da guia Serviços Web) e selecionando a opção **Testar serviço Web**:

<figure>
    <a href="/images/2011-03-04-criando-webservice-com-ejb-02.png"><img src="/images/2011-03-04-criando-webservice-com-ejb-02.png" alt="Testar Web Service."></a>
</figure>

Após clicar no item Testar serviço web será aberto no navegador uma página para testar os métodos do web service, se clicarmos em WSDL File (arquivo WSDL) podemos ver o XML do WSDL gerado e se clicarmos na operação olaMundo veremos a execução de seu método:

<figure>
    <a href="/images/2011-03-04-criando-webservice-com-ejb-03.png"><img src="/images/2011-03-04-criando-webservice-com-ejb-03.png" alt="Testar Web Service."></a>
</figure>

Quando clicamos no método olaMundo podemos ver a requisição e a resposta no formato SOAP.

<figure>
    <a href="/images/2011-03-04-criando-webservice-com-ejb-04.png"><img src="/images/2011-03-04-criando-webservice-com-ejb-04.png" alt="Testar Web Service."></a>
</figure>

Outro exemplo de Web Service:

Neste exemplo temos um Web Service que faz uma conexão com o banco de dados para consultar as vendes de livros realizados em um período recebido via parâmetro e retorna um simples texto com as informações das vendas.

{% highlight java %}
@WebService()
@Stateless()
public class VendasWS {
  @PersistenceContext(unitName = "LivrariaPU")
  private EntityManager em;

  @WebMethod(operationName = "consultarVendasPorPeriodo")
  public String consultarVendasPorPeriodo(@WebParam(name = "inicioPeriodo") String inicioPeriodo, @WebParam(name = "fimPeriodo") String fimPeriodo) {
    String retorno = "";
    try {
      DateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
      VendaDAO dao = new VendaDAO(em);
      List<Venda> vendas = dao.procurarPorPeriodo(dateFormat.parse(inicioPeriodo), dateFormat.parse(fimPeriodo));
      for(Venda venda : vendas) {
        retorno += venda.getCliente().getNome() + " - " + dateFormat.format(venda.getDataVenda());
        retorno += "\n-----Livros------";
        for(Livro livro : venda.getLivros()) {
          retorno += "\nTitulo: " + livro.getTitulo() + " - R$" + livro.getPreco();
        }
        retorno += "\nTotal: R$" + venda.getValorTotal() + "\n--------------\n\n";
      }
    } catch (ParseException ex) {
      ex.printStackTrace(); //Erro ao converter as datas.
    }
    return retorno;
  }
}
{% endhighlight %}
