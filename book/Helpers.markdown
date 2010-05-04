Helpers
=============

O básico
----------

Não é aconselhável que você crie helpers na raiz da sua aplicação. Eles distorcem	o namespace global e não têm um fácil acesso aos requests, reponse, session ou cookies.

Em vez disso, use o prático método Sinatra::EventContenxt para instalar helpers para o uso dentro de eventos e templates.

Exemplo:

	helpers do
	  def bar(name)
	    "#{name}bar"
	  end
	end


	get '/:name' do
	  bar(params[:name])
	end

implementando partials no estilo rails
------------------------------------

Usar partials em suas views é uma ótima maneira de mantê-las limpas. Para que Sinatra possa se aproximar do design de um framework, você terá que implementar um handler de partial nele próprio.


Eis aqui uma versão realmente básica:

    # Usage: partial :foo
    helpers do
      def partial(page, options={})
          haml page, options.merge!(:layout => false)
      end
    end


Uma versão mais avançada desse handler passando variáveis locais e interando um hash seria algo parecido com isto:


    # Render the page once:
    # Usage: partial :foo
    #
    # foo will be rendered once for each element in the array, passing in a local variable named "foo"
    # Usage: partial :foo, :collection => @my_foos

    helpers do
      def partial(template, *args)
        options = args.extract_options!
        options.merge!(:layout => false)
        if collection = options.delete(:collection) then
          collection.inject([]) do |buffer, member|
            buffer << haml(template, options.merge(
                                      :layout => false,
                                      :locals => {template.to_sym => member}
                                    )
                         )
          end.join("\n")
        else
          haml(template, options)
        end
      end
    end
