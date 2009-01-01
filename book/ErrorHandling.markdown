Error Handling
==============

not\_found
---------
Lembre-se: Isto irá rodar sob o Sinatra::EventContext que significa que você poderá te-lo em todos os bons formatos que lhe são oferecidos (i.e. haml, erb, :halt, etc.)

Sempre que uma NotFound é levantadas ela é tratada pela seguinte chamada

    not_found do
      'Isto esta longe de ser encontrado'
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
Com Sinatra você pode tornar o padrão de error e not\_found mais seguro em :production. Se você deseja customizar apenas para :production com uma tela mais amigável do que em :development então faça o seguinte:

	configure :production do

	  not_found do
	    "Desculpe-nos, mas isto não esta funcionando"
	  end

	  error do
	    "Algo realmente desagradável aconteceu.  Estamos trabalhando nisto!"
	  end

	end
