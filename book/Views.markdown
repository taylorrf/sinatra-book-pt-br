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
          xml.subnode "Inner text"
        end
      end
	end

Isto irá renderizar o xml inline, diretamente do handler.

#### Atom Feed

#### RSS Feed

Layouts
-------
Layouts são muito simples no Sinatra. Crie um arquivo em seu diretório de views nomeado como "layout.erb", "layout.haml" ou "layout.builder".  Quando a página renderizar, o layout apropriado será invocado (do mesmo tipo de arquivo) e usado.


O layout pode ter no seu contexto um ponto onde é realizada uma chamada para incluir outros conteúdos.

An example haml layout file could look something like this:

    %html
      %head
        %title SINATRA BOOK
      %body
        #container
          = yield

evitando um layout
-----------------
Algumas vezes você deseja que o layout não seja renderizado. No seu método de renderização simplesmente passe :layout => false, e você será o cara.

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
	%div.title Hello world!!!!!

Experimente!

Partials
--------
