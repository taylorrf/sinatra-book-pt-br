Rack Middleware
===============

Sinatra pode rodar sob [Rack][rack], um padrão mínimo de interface para frameworks web escritos em Ruby. Uma das capacidades mais interessantes do Rack é a de se poder desenvolver uma aplicação sob um “middleware” —  componente localizado entre o servidor e sua aplicação monitorando e/ou manipulando requisições/respostas HTTP fornecendo varias funcionalidades em comuns.

Sinatra faz a utilização de um canal Rack middleware através do uso muito fácil de um método de alto nível:

	require 'sinatra'
	require 'my_custom_middleware'

	use Rack::Lint
	use MyCustomMiddleware

	get '/hello' do
	    'Hello World'
	end


A semântica de "utilização" é idêntica a definida para o [Rack:: Construtor] [rack_builder] DSL (mais frequentemente utilizado a partir do arquivo rackup). Como por exemplo, ao utilizar um método que aceita múltiplos/variáveis argumentos da mesma forma como os blocos:

	use Rack::Auth::Basic do |username, password|
	   username == 'admin' && password == 'secreto'
	end


Rack é distribuída com uma variedade de middleware padrões para login, debug, URL routing, autenticação e manipulação de sessões. Sinatra usa muito desses componentes automaticamente baseado na configuração de modo que você normalmente não tenha que usá-los explicitamente.


[rack]: http://rack.rubyforge.org/
[rack_builder]: http://rack.rubyforge.org/doc/classes/Rack/Builder.html

