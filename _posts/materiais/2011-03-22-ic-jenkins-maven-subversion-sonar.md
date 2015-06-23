---
layout: article
title: "Integração Contínua com Jenkins"
categories: materiais
author: sakurai
date: 2011-03-22 16:00:00
tags: [jenkins, maven, subversion, integração]
published: true
excerpt: Exemplo de uso de repositório de fontes com Subversion, integração continua com Jenkins CI e analise de código com o Sonar.
comments: true
image:
  teaser: 2011-03-22-teaser-ic-jenkins-maven-subversion-sonar.png
ads: true
---

Neste exemplo vamos criar uma aplicação Java bem simples com apenas uma classe e usando o JUnit para testar esta classe, iremos versionar o projeto no Subversion para controlar as alterações, depois vamos configurar o Jenkins CI para ele gerar o build da aplicação a partir do repositório do Subversion, feito isso adicionamos o plugin do Sonar que irá verificar a qualidade do código-fonte.

## Integração Contínua

"Integração Contínua é uma pratica de desenvolvimento de software onde os membros de um time integram seu trabalho frequentemente, geralmente cada pessoa integra pelo menos diariamente – podendo haver múltiplas integrações por dia. Cada integração é verificada por um build automatizado (incluindo testes) para detectar erros de integração o mais rápido possível. Muitos times acham que essa abordagem leva a uma significante redução nos problemas de integração e permite que um time desenvolva software coeso mais rapidamente." [Martin Fowler]

### Ciclo do desenvolvimento com Integração Continua [FOWLER, M.]

* Fazer uma cópia local do repositório (checkout).
* Desenvolver sua tarefa com testes unitários.
* Montar uma build do projeto e passar por todos os testes.
* Atualizar a cópia local com a versão do repositório.
* Montar uma build do projeto e passar por todos os testes (se algo de errado nos testes é preciso corrigir até que a versão esteja sincronizada com a versão principal).
* Guardar suas alterações no repositório (commit).
* Montar um build a partir de uma maquina de integração (garantindo que todas as novas alterações foram submetidas corretamente) de preferencia de forma automática com algum programa de integração continua.

## Criando um repositório no Subversion

Download do Subversion em [http://subversion.apache.org](http://subversion.apache.org)

O Subversion é um sistema de controle de versão centralizado, usando como base um repositório onde todos os códigos-fontes (na verdade você pode guardar tudo que for importante) são versionados gerando históricos de alterações,

Crie um novo repositório:

{% highlight xml %}
 $ svnadmin create /caminho/do/repositório
{% endhighlight %}

Exemplo:

{% highlight xml %}
 $ svnadmin create /Users/sakurai/Desktop/IC/Repositorio
{% endhighlight %}

## Criando um projeto Java no NetBeans com Maven

Download do NetBeans em [http://netbeans.org/](http://netbeans.org/)

> Neste exemplo estou usando o NetBeans, mas se preferir pode fazer o projeto em outra IDE ou manualmente.

Exemplo de Projeto Java:

Crie uma classe que retorna o ano do veículo, dado o chassi e a posição que informa o ano do veículo.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-01.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-01.png" alt="Exemplo caractere com valor do ano do chassi."></a>
</figure>

### Criando o projeto Java com Maven

No NetBeans crie um novo **Projeto Maven - Aplicativo Java**.

#### pom.xml

Alterei o pom.xml para informar a versão 4.8.2 do JUnit que estou usando e informar onde fica no build as classes .java e .class da aplicação:

{% highlight xml %}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>tdd</groupId>
  <artifactId>Chassi</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>Chassi</name>
  <url>http://www.universidadejava.com.br</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.8.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <sourceDirectory>
      ${project.basedir}/src/main/java
    </sourceDirectory>
    <testSourceDirectory>
      ${project.basedir}/src/test/java
    </testSourceDirectory>
    <resources>
      <resource>
        <directory>
          ${project.basedir}/src/main/java
        </directory>
      </resource>
    </resources>
    <testResources>
      <testResource>
        <directory>
          ${project.basedir}/src/test/java
        </directory>
      </testResource>
    </testResources>
  </build>
</project>
{% endhighlight %}

Crie uma classe de teste:

> O ideal é desenvolver seguindo as práticas do TDD.

{% highlight java %}
package br.universidadejava.ic.util;

import static org.junit.Assert.*;
import org.junit.Test;

public class ValidarChassiTest {
    ValidarChassi util = new ValidarChassi();

    @Test
    public void validarAnoANoChassi() {
        assertEquals(2010, util.validar("9BP17164GA000001", 10));
    }

    @Test
    public void validarAno9NoChassi() {
        assertEquals(2009, util.validar("9BP17164G9000002", 10));
    }

    @Test
    public void validarAnoBNoChassi() {
        assertEquals(2011, util.validar("9BP17164GB000003", 10));
    }

    @Test
    public void validarAnoAMinusculoNoChassi() {
        assertEquals(2010, util.validar("9bp17164ga000001", 10));
    }

    @Test(expected=IllegalArgumentException.class)
    public void validarAnoIncorreto() {
        assertEquals(2020, util.validar("9bp17164gz000004", 10));
    }
}
{% endhighlight %}

Crie a classe com a implementação:

{% highlight java %}
package br.universidadejava.ic.util;

public class ValidarChassi {
    public int validar(String chassi, int posicao) {
        if(chassi.charAt(posicao - 1) == '9') {
            return 2009;
        } else if(chassi.toUpperCase().charAt(posicao - 1) == 'A') {
          return 2010;
        } else if(chassi.toUpperCase().charAt(posicao - 1) == 'B') {
          return 2011;
        }

        throw new IllegalArgumentException("Valores incorretos!!!");
    }
}
{% endhighlight %}

> Se você acha que este código está bem escrito, vai gostar de ver a opinião do Sonar.

### Importe o projeto Java do Netbeans para o repositório do SubVersion

Com o botão direito em cima do projeto selecione **Versionamento -> Importar no repositório Subversion…**

Na tela do Repositório do Subversion informe a URL do repositório.

Exemplo de URL:

{% highlight xml %}
file:///Users/sakurai/Desktop/IC/Repositorio
{% endhighlight %}

Clique em **Avançar**.

Especifique uma mensagem.

Clique em **Finalizar**.

### Iniciar o Jenkins CI

Download do Jenkins CI em [http://jenkins-ci.org](http://jenkins-ci.org)

O Jenkins CI é uma aplicação de Integração Continua, nela podemos

#### Iniciar o Jenkins:

{% highlight java %}
java -jar jenkins.war
{% endhighlight %}

Para acessar o Jenkins CI abra a URL: [http://localhost:8080/](http://localhost:8080/)

#### Configurando o Jenkins CI

No menu Gerenciar Jenkins entre em Configurar sistema:

Informe o local de instalação do JDK.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-02.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-02.png" alt="Instalação do JDK."></a>
</figure>

Informe a versão do Maven.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-03.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-03.png" alt="Versão do Maven."></a>
</figure>

Informe o local do repositório do Subversion.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-04.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-04.png" alt="Repositório do Subversion."></a>
</figure>

Criar uma tarefa no Jenkins CI:

* Informe o nome da tarefa, selecione Construir com Maven 2/3 e clique em OK
* Informar o caminho do repositório.
* Informar o local do pom.xml dentro do projeto.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-05.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-05.png" alt="Disparadores de Construção."></a>
</figure>

### Instalando o Sonar

Download do Sonar [http://www.sonarsource.org](http://www.sonarsource.org)
Descompactar o arquivo sonar-x.x.zip.
Escolhar dentro do diretório bin a versão de SO.

Iniciar o Sonar:

{% highlight java %}
$ ./sonar.sh start
{% endhighlight %}

Para acessar o Sonar abra a URL: [http://localhost:9000](http://localhost:9000)

### Adicionar o plugin do Sonar no Jenkins CI

No menu Gerenciar Jenkins entre em Gerenciar Plugins selecione na categoria Disponíveis o plugin do Sonar e clique no botão instalar.

No menu Gerenciar Jenkins:

Configurar sistema informe que irá usar o Sonar.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-06.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-06.png" alt="Adicionar Sonar no projeto."></a>
</figure>

Na tarefa entre no menu Configurar informe que está usando o Sonar:

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-07.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-07.png" alt="Configurar ações para chamar o Sonar."></a>
</figure>

Entre na tarefa e clique em Construir Agora

Durante a construção do Projeto é possível acompanhar o andamento através da Saída do Console.

Após terminar a construção, podemos visualizar:

* Se aconteceu algum erro ao montar o build.
* Verificar as mudanças no código.
* Se executou todos os testes.

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-08.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-08.png" alt="Todos os testes."></a>
</figure>

* Podemos acessar o Sonar e visualizar a qualidade do código.

Note que temos apenas 13 linhas de código e o Sonar já registrou 4 violações de categoria Minor (menor).

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-09.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-09.png" alt="Dashboard do Sonar."></a>
</figure>

Se clicar em cima do item Minor o Sonar vai detalhar as violações e as classes que elas ocorrem:

<figure>
    <a href="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-10.png"><img src="/images/2011-03-22-ic-jenkins-maven-subversion-sonar-10.png" alt="Detalhe de violações no Sonar."></a>
</figure>

## Referências

* [FOWLER, M.] **Continuous Integration** - Disponível em: http://martinfowler.com/articles/continuousIntegration.html
* [FOWLER, M. 04] **Refatoração: Aperfeiçoando o Projeto de Código Existente.** Porto Alegre: Bookman, 2004. 366 p.
* [FREEMAN, S.; PRYCE, N.] **Growing Object-Oriented Software, Guided By Tests.** Boston: Person, 2010. 358 p.
* [JENKINS CI] **Jenkins Continuous Integration** - Disponível em: http://jenkins-ci.org/
* [SONAR] **Sonar** - Disponível em: http://www.sonarsource.org/
* [SUBVERSION] **Subversion** - Disponível em: http://subversion.apache.org/
