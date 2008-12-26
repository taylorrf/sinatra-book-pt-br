Introdução
=============

O que é Sinatra?
--------------------------
Sinatra é uma linguagem de domínio específico (DSL) para a criação rápida de aplicações web escritas em ruby.  Ele mantém uma característica mínima definida, deixando livre o desenvolvedor para utilizar as ferramentas que melhor lhe servir em sua aplicação.


Instalação
---------------------------------
A maneira mais simples de se obter o Sinatra é através da rubygems

	$ sudo gem install sinatra

Aplicação de exemplo
-----------
O Sinatra já está instalado e você esta pronto para experimentá-lo, que tal já construir sua primeira aplicação?

	# myapp.rb
	require 'rubygems'
	require 'sinatra'

	get '/' do
	  'Olá mundo!'
	end

Rode isto com o seguinte comando $ ruby myapp.rb e visualize o resultado em http://localhost:4567

Vivendo com as novidades
--------------------------------
Looking to live life (or Sinatra I should say) on the edge, huh?  Well, it's rather simple.
The latest greatest Sinatra is on Github; so using the power of git and the power of our minds--we can do the following

1. cd local/do/seu/projeto
2. git clone git://github.com/bmizerany/sinatra.git
3. cd sinatra
4. git submodule init && git submodule update
5. cd seu\_projeto
6. ln -s ../sinatra

Para usar este poder profano, basta adicionar a seguinte linha no arquivo sinatra.rb

	$:.unshift File.dirname(__FILE__) + '/sinatra/lib'
	require 'sinatra'

Isto certamente é viver junto com as últimas novidades

Sobre este livro
---------------
Este livro assume que você já tem um conhecimento básico da linguagem Ruby e sabe como funciona o interpretador Ruby.

Para maiores informações sobre a linguagem Ruby visite o seguinte link:

- http://www.ruby-lang.org