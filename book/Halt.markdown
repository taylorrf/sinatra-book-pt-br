Halt!
=============

Para interromper imediatamente um request durante ou antes com um filtro ou na utilização de um evento:

	throw :halt


Para definir o corpo do resultado com um método helper:

	throw :halt, :helper_method


Define o corpo do resultado com um método helper enviando depois com os parâmetros do escopo local

	throw :halt, [:helper_method, foo, bar]


Para definir o corpo do resultado com uma string:

	throw :halt, 'Isto será a mensagem!'


Para definir um status e depois o corpo da mensagem:

	throw :halt, [401, 'vá embora!']


Define o status e então chama o método helper com os parâmetros do escopo local

	throw :halt, [401, [:helper_method, foo, bar]]


Roda uma proc na instância de Sinatra:EventContext e define o corpo do resultado

	throw :halt, lambda { puts 'Na proc!'; 'Eu escrevi ao $stdout!' }



Crie seu próprio "toresult"

	class MyResultObject
		def to_result(event_context, *args)
			event_context.body = 'Isto será a mensagem!'
		end
	end

	get '/' do
		throw :halt, MyResultObject.new
	end
