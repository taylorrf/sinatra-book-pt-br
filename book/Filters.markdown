Filtros
=============

before do...
------------
Este filtro é executado em Sinatra::EventContext

  before do

.. este código será executado antes de cada evento ..

  end


Manipulação de parâmetros aninhados ao estilo Rails
------------------------------------
Se você quiser usar um formulário com parâmetros ao estilo deste (aka. Rails' nested params)

		<form>
		  <input ... name="post[title]" />
		  <input ... name="post[body]" />
		  <input ... name="post[author]" />
		</form>


Você deve converter os parâmetros para um hash. Você pode fazer isso facilmente usando o filtro "before":

		before do
		 new_params = {}
		 params.each_pair do |full_key, value|
			 this_param = new_params
			 split_keys = full_key.split(/\]\[|\]|\[/)
			 split_keys.each_index do |index|
				 break if split_keys.length == index + 1
				 this_param[split_keys[index]] ||= {}
				 this_param = this_param[split_keys[index]]
			 end
			 this_param[split_keys.last] = value
		 end
		 request.params.replace new_params
		end


Para então se tornarem nos seguintes parâmetros:

{"post"=>{ "title"=>"", "body"=>"", "author"=>"" }}
