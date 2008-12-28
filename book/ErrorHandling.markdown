Error Handling
==============

not\_found
---------
Remember: These are run inside the Sinatra::EventContext which means you get all the goodies is has to offer (i.e. haml, erb, :halt, etc.)

Whenever NotFound is raised this will be called

    not_found do
      'This is nowhere to be found'
    end

erros
-----
Por default os erros são capturados por Sinatra::ServerError

O Sinatra irá enviar seu erro através do ‘sinatra.error’ no request.env

	error do
	  'Desculpe mas houve um erro desagradável - ' + request.env['sinatra.error'].name
	end

Customizando o error mapping:

	error MyCustomError do
	  'O que acabou de acontecer foi...' + request.env['sinatra.error'].message
	end

se isto acontecer:

	get '/' do
	  raise MyCustomError, 'algo terrível'
	end

você irá obter isto:

	O que acabou de acontecer foi... algo terrível

Informações Adicionais
----------------------
Because Sinatra give you a default not\_found and error do :production that are secure. If you want to customize only for :production but want to keep the friendly helper screens for :development then do this:

	configure :production do

	  not_found do
	    "Desculpe-nos, mas isto não esta funcionando"
	  end

	  error do
	    "Algo realmente desagradável aconteceu.  Estamos trabalhando nisto!"
	  end

	end
