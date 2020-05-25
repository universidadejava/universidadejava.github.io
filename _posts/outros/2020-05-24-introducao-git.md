---
layout: article
title: "Aprenda como começar a controlar a versão do seu código usando o Git"
categories: outros
author: sakurai
date: 2020-05-24 23:32:00
tags: [java, git, versionamento]
published: true
excerpt: 
comments: true
image:
  teaser: 2020-05-24-teaser-introducao-git.png
ads: false
---

## Git - controle de versão

> "O Git é um sistema de controle de versão distribuído gratuito e open source desenhado para tratar de projetos pequenos à grandes  com velocidade e eficiência." [http://git-scm.com](http://git-scm.com/)


### init

Para criar um novo repositório do Git utilize o comando `git init`:

{% highlight java %}
$ git init
Initialized empty Git repository in /Users/sakurai/Documents/Git/.git/
{% endhighlight %}

Após executar o comando `git init` será criado um subdiretório oculto chamado `.git`. Agora é possível registrar as alterações realizadas no diretório.

### status

Para verificar o status do repositório, utilize o comando `git status`.

{% highlight java %}
$ git status
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git add" to track)
{% endhighlight %}

Após adicionar, modificar ou remover um arquivo no diretório utilize o comando `git status` para verificar a situação repositório.

{% highlight java %}
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   	index.html
nothing added to commit but untracked files present (use "git add" to track)
{% endhighlight %}

### add

Para adicionar um arquivo utilize o comando `git add`:

{% highlight java %}
$ git add index.html
{% endhighlight %}

Nesse primeiro momento o arquivo ainda não foi adicionado no repositório, ele foi incluído no **Staging Area**.

Para adicionar vários itens no índice utilize o comando:

{% highlight java %}
$ git add .
{% endhighlight %}

Após adicionar um arquivo no repositório utilize o comando `git status` para verificar a situação repositório.

{% highlight java %}
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#   	new file:   index.html
{% endhighlight %}

Modifique um arquivo e verifique a situação repositório.

{% highlight java %}
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#   	new file:   index.html
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#   	modified:   index.html
{% endhighlight %}

### diff

Para verificar as alterações realizadas no arquivo podemos utilizar o comando `git diff`.

{% highlight java %}
$ git diff
diff --git a/index.html b/index.html
index 814da57..35f1ee9 100644
--- a/index.html
+++ b/index.html
@@ -2,5 +2,10 @@
   <head><title>Olá mundo com Git</title></head>
   <body>
 	<h1>Olá mundo com o Git!</h1>
+	<ul>
+  	<li>Inicializar repositório</li>
+  	<li>Adicionar arquivos na Staging Area</li>
+  	<li>Comparar arquivos</li>
+	<ul>
   </body>
 </html>
\ No newline at end of file
{% endhighlight %}

### commit

Para armazenar as mudanças no repositório precisamos executar o comando `git commit`:

{% highlight java %}
$ git commit -m "Página inicial do site"
[master (root-commit) 75a6d88] Página inicial do site
 1 file changed, 1 insertion(+)
 create mode 100644 index.html
{% endhighlight %}

### log

Para visualizar o histórico dos commits podemos utilizar o comando `git log`:

{% highlight java %}
$ git log
commit 75a6d8835fab8094110c0036c38c58e7e605d53d
Author: Rafael Sakurai <rafael@sakurai.com.br>
Date:   Mon Feb 10 17:37:22 2014 -0200
    Página inicial do site
{% endhighlight %}

Uma outra forma de visualizar os commits utilize `git log --graph`:

{% highlight java %}
$git log --graph
* commit 2fbb41427e4b09e966b1351cb6fbc6a3f299c6fd
| Author: Rafael Sakurai <rafasakurai@gmail.com>
| Date:   Mon Feb 10 19:04:55 2014 -0200
|
| 	teste
| 
* commit 75a6d8835fab8094110c0036c38c58e7e605d53d
  Author: Rafael Sakurai <rafasakurai@gmail.com>
  Date:   Mon Feb 10 17:37:22 2014 -0200
 
  	Página inicial do site
{% endhighlight %}

### reset

Adicionou algum arquivo errado com o `git add`, para desfazer utilize o `git reset`:

{% highlight java %}
$ git reset index.html
Unstaged changes after reset:
M   	index.html
{% endhighlight %}

### checkout

Para desfazer as alterações feitas localmente utilize o comando `git checkout -- <file>`:

{% highlight java %}
$ git checkout -- index.html
{% endhighlight %}

### Removendo um commit

Removendo o último commit (mantêm os arquivos alterados localmente):

{% highlight java %}
git reset HEAD~1
{% endhighlight %}

Voltando para um commit em específico (mantêm os arquivos alterados localmente):

{% highlight java %}
git reset --soft commit-id
{% endhighlight %}

Voltando para um commit em específico (remove arquivos dos commits):

{% highlight java %}
git reset --hard commit-id
{% endhighlight %}

### stash

Stash é usado como uma área temporária, para colocar os arquivos do índice no `stash` utilize o comando:

{% highlight java %}
$ git stash
{% endhighlight %}

Para verificar o stash utilize o comando:

{% highlight java %}
$ git stash list
{% endhighlight %}

Para voltar os arquivos do stash para o índice utilize o comando:

{% highlight java %}
$ git stash apply
{% endhighlight %}

Para limpar o stash utilize o comando:

{% highlight java %}
$ git stash clear
{% endhighlight %}

Para voltar os arquivos do último stash para o índice e ainda remover o stash da lista utilize o comando:

{% highlight java %}
$ git stash pop
{% endhighlight %}

### branch

Para criar uma nova branch utilize o comando `git branch <nome>`:

{% highlight java %}
$ git branch dev
{% endhighlight %}

Para ver quais as branches existentes utilize o comando `git branch`:

{% highlight java %}
$ git branch
  dev
* master
{% endhighlight %}

Para trocar entre branches utilize o comando `git checkout <nome>`:

{% highlight java %}
$ git checkout dev
M   	index.html
Switched to branch 'dev'
{% endhighlight %}

Para remover uma branch, utilize o comando:

{% highlight java %}
$ git branch -D dev
Deleted branch dev (was 04b1187).
{% endhighlight %}

### merge

Para realizar merge entre as branches utilize o comando:

{% highlight java %}
$ git merge dev
Updating 2fbb414..5bd4928
Fast-forward
 index.html | 1 +
 novo.txt   | 1 +
 2 files changed, 2 insertions(+)
 create mode 100644 novo.txt
{% endhighlight %}