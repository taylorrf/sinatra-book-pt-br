Views
=====
Todos arquivos-base serão localizados na seguinte estrutura:

	root
	  | - views/

Linguagens de Template
------------------

### Haml
	get '/' do
	  haml :index
	end

Isto irá renderizar  ./views/index.haml

### Sass
	get '/' do
	  sass :styles
	end

Isto irá renderizar ./views/styles.sass

### Erb
	get '/' do
	  erb :index
	end

Isto irá renderizar ./views/index.erb

### Builder
	get '/' do
	  builder :index
	end

Isto irá renderizar ./views/index.builder

	get '/' do
	  builder do |xml|
        xml.node do
          xml.subnode "Texto interno"
        end
      end
	end

Isto irá renderizar o xml inline, diretamente do handler.

#### Atom Feed
em breve

#### RSS Feed
Suponhamos que a url do site seja http://liftoff.msfc.nasa.gov/.


		get '/rss.xml' do
		  builder do |xml|
		    xml.instruct! :xml, :version => '1.0'
		    xml.rss :version => "2.0" do
		      xml.channel do
			xml.title "Liftoff News"
			xml.description "Liftoff to Space Exploration."
			xml.link "http://liftoff.msfc.nasa.gov/"

			@posts.each do |post|
			  xml.item do
			    xml.title post.title
			    xml.link "http://liftoff.msfc.nasa.gov/posts/#{post.id}"
			    xml.description post.body
			    xml.pubDate Time.parse(post.created_at.to_s).rfc822()
			    xml.guid "http://liftoff.msfc.nasa.gov/posts/#{post.id}"
			  end
			end
		      end
		    end
		  end
		end

Isso irá renderizar o rss inline, diretamente no handler.



Layouts
-------
Layouts são muito simples no Sinatra. Crie um arquivo em seu diretório de views nomeado como "layout.erb", "layout.haml" ou "layout.builder".  Quando a página renderizar, o layout apropriado será invocado (do mesmo tipo de arquivo) e usado.


O layout pode ter no seu contexto um ponto onde será realizada uma chamada para incluir outros conteúdos.

Um exemplo de um layout com haml seria um arquivo parecido com este:

    %html
      %head
        %title SINATRA BOOK
      %body
        #container
          = yield

evitando um layout
-----------------
Algumas vezes você deseja que o layout não seja renderizado. No seu método de renderização simplesmente passe :layout => false e você será o cara.

    get '/' do
      haml :index, :layout => false
    end

No arquivo das Views
-------------
Isto é uma fria:

	get '/' do
	  haml :index
	end

	use_in_file_templates!

	__END__

	@@ layout
	X
	= yield
	X

	@@ index
	%div.title Olá Mundo!!!!!

Experimente!

Partials
--------
em breve
