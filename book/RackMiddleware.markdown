Rack Middleware
===============

Sinatra pode rodar sob [Rack][rack], um padrão mínimo de interface para frameworks web escritos em Ruby. Uma das capacidades mais interessantes do Rack é a de desenvolver uma aplicação sob uma “middleware” —  componente localizado entre o servidor e sua aplicação monitorando e/ou manipulando requisições/respostas HTTP fornecendo varias funcionalidades em comuns.


Sinatra makes building Rack middleware pipelines a cinch via a top-level "use" method:

	require 'sinatra'
	require 'my_custom_middleware'

	use Rack::Lint
	use MyCustomMiddleware

	get '/hello' do
	    'Hello World'
	end


The semantics of "use" are identical to those defined for the [Rack::Builder][rack_builder]  DSL (most frequently used from rackup files). For example, the use  method accepts multiple/variable args as well as blocks:

	use Rack::Auth::Basic do |username, password|
	   username == 'admin' && password == 'secreto'
	end


Rack is distributed with a variety of standard middleware for logging, debugging, URL routing, authentication, and session handling. Sinatra uses many of of these components automatically based on configuration so you typically don’t have to use them explicitly.

[rack]: http://rack.rubyforge.org/
[rack_builder]: http://rack.rubyforge.org/doc/classes/Rack/Builder.html

