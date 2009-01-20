Introdução
=============

O que é Sinatra?
--------------------------
Sinatra é uma linguagem de domínio específico (DSL - Domain Specific Language) para a criação rápida de aplicações web escritas em ruby.

Ele mantém uma característica mínima definida, deixando livre o desenvolvedor para utilizar as ferramentas que melhor lhe servir em sua aplicação.

Ele não exige muito sobre sua aplicação, apenas que:

* será escrito em linguagem de programação Ruby
* terá URLs

Em Sinatra, você pode escrever pequenas _ad hoc_ aplicações ou maduras e grandes aplicações coma a mesma facilidade.
(Veja a seção "Aplicações do mundo real" depois neste livro.)

Você pode utilizar o poder de varias Rubygems e outras bibliotecas para Ruby disponíveis.

Sinatra realmente se destaca quando você utiliza-o para experimentos e aplicações mock-ups ou para criar uma rápida interface para o seu código.

Este não é um framework Model-View-Controller, mas controles especificos de URL que direciona para código Ruby relevante que retorna resultados como resposta. Isto irá habilitar você a escrever códigos limpos, com aplicações devidamente organizadas: separando as _views_ do código da aplicação, por exemplo.


Instalação
---------------------------------
A maneira mais simples de se obter o Sinatra é através da rubygems

	$ sudo gem install sinatra


### Dependências

Sinatra depende da gem _Rack_ (http://rack.rubyforge.org).

Para obter uma melhor experiência no uso você pode também instalar as gems _Haml_ (http://haml.hamptoncatlin.com) e _Builder_ (http://builder.rubyforge.org), que simplificarão os trabalhos com as views.

    $ sudo gem install builder haml


### Vivendo com as novidades

As últimas novidades do Sinatra estão no Github, disponível em **http://github.com/sinatra/sinatra/tree/master**.

Você também pode utilizar a última versão para contribuir com novas funcionalidades para o framework.

Siga os seguintes passos:

1. cd local/do/seu/projeto
2. git clone git://github.com/sinatra/sinatra.git
3. cd sinatra
4. cd seu\_projeto
5. ln -s ../sinatra

E para utilizar isto basta adicionar a seguinte linha no arquivo da sua aplicação:

	$:.unshift File.dirname(__FILE__) + '/sinatra/lib'
	require 'sinatra'


Você pode verificar qual a versão que a aplicação esta utilizando adicionando a seguinte rota:

    get '/versao' do
      "Versão utilizada: " + Sinatra::VERSION
    end

para verificar o resultado acesse `http://localhost:4567/versao`.


Aplicação "Hello Word"
-----------------------------
O Sinatra já está instalado e você esta pronto para experimentá-lo, que tal construir sua primeira aplicação?

	# hello_world.rb
	require 'rubygems'
	require 'sinatra'

	get '/' do
	  "Olá Mundo, agora é #{Time.now} no servidor!"
	end

Rode isto com o seguinte comando $ ruby myapp.rb e visualize o resultado em http://localhost:4567


Aplicações do mundo real com Sinatra
----------------------------------

### Github Services

Git hosting disponibilizado pelo Github utiliza Sinatra para post-receive hooks, chamando os serviços/URLs especificas do usuário, sempre que alguém envia algo para os repositórios:

* http://github.com/blog/53-github-services-ipo
* http://github.com/guides/post-receive-hooks
* http://github.com/pjhyett/github-services

### Git Wiki

Git Wiki é um mecanismo minimo de Wiki desenvolvido com Sinatra e Git. Veja também os diversos forks com funcionalidades adicionais.

* http://github.com/sr/git-wiki
* http://github.com/sr/git-wiki/network

### Integrity

Integrity é um serviço pequeno e limpo de integração continua utilizando Sinatra, detectando falhas em builds de seu codebase e notificando você através de difersos canais de comunicação.

* http://www.integrityapp.com/
* http://github.com/foca/integrity

### Seinfeld Calendar

Seinfeld Calendar é uma divertida aplicação para monitorar as suas contribuições em projetos open-source, mostrando seus "streaks", ou seja seus commits em seus repositórios do Github.

* http://www.calendaraboutnothing.com
* http://github.com/entp/seinfeld

Sobre este livro
---------------
Este livro assume que você já tem um conhecimento básico da linguagem Ruby e sabe como funciona o interpretador Ruby.

Para maiores informações sobre a linguagem Ruby visite os seguintes links:

* [http://www.ruby-lang.org][rubylang]
* [http://www.ruby-lang.org/en/documentation/ruby-from-other-languages/][rubylang-documentation]
* [http://www.ruby-doc.org][rubydoc]
* [http://www.ruby-doc.org/core-1.8.7/index.html][rubydoc-core]
* [http://www.ruby-doc.org/docs/ProgrammingRuby/][rubydoc-programmingruby]

[rubylang]: http://www.ruby-lang.org/
[rubylang-documentation]: http://www.ruby-lang.org/en/documentation/ruby-from-other-languages/
[rubydoc]: http://www.ruby-doc.org
[rubydoc-core]: http://www.ruby-doc.org/core-1.8.7/index.html
[rubydoc-programmingruby]: http://www.ruby-doc.org/docs/ProgrammingRuby/
