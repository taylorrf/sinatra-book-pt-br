Rotas
======

métodos http
------------
As rotas do Sinatra foram concebidas para responder a métodos de requisições HTTP.

* get
* post
* put
* delete



básico
-----
Simples

	get '/hi' do
	  ...
	end

Com parâmetros

	get '/:name' do
	  # matches /sinatra and the like and sets params[:name]
	end

opções
-------

splats
------
	get '/say/*/to/*' do
	  # matches /say/hello/to/world
	  params["splat"] # => ["hello", "world"]
	end

	get '/download/*.*' do
	  # matches /download/path/to/file.xml
	  params["splat"] # => ["path/to/file", "xml"]
	end


user agent
----------
	get '/foo', :agent => /Songbird (\d\.\d)[\d\/]*?/ do
	  "You're using Songbird version #{params[:agent][0]}"
	end

	get '/foo' do
	  # matches non-songbird browsers
	end

outros métodos
-------------
Os outros métodos são requisitados exatamente da mesma forma como na rota "get". Você simplesmente usa o "post", "put" ou "delete" que já estarão definidos como uma rota, além do "get" é claro. Para acessar os parametros POSTados, use params\[:xxx\] onde xxx é nome do elemento postado pelo seu formulário.

    post '/foo' do
      "você requisitou por foo, com o parametro bar via post igual a #{params[:bar]}"
    end


os métodos PUT e DELETE
--------------------------

Quando os browsers não suportavam nativamente os métodos PUT e DELETE, alguns hacks ou workaround eram adotados pela comunidade web. Bastava adicionar um elemento hidden com o nome "metodo" e o valor igual ao método HTTP que você desejava usar. O formulário continuará sendo enviado como um POST, mas o Sinatra irá interpretá-lo com o método desejado.

Quando você quiser usar PUT ou DELETE em um formulário com um cliente que não tem suporte (da mesma forma como Curl ou ActiveResorce), simplesmente vá em frente e use-os normalmente, ignorando aquele método citado acima. Aquilo é apenas para "hacks" em apoio aos browsers.


como as rotas são requisitadas
------------------------
Each time you add a new route to your application, it gets compiled down into a
regular expression that will match it.  That is stored in an array along with
the handler block attached to that route.

When a new request comes in, each regex is run in turn, until one matches.  Then
the the handler (the code block) attached to that route gets executed.


