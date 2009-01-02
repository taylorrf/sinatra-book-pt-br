Models
======

Datamapper
----------
em breve


Sequel
------
em breve


ActiveRecord
------------
Primeiro você deverá incluir a gem ActiveRecord na sua aplicação, em seguida definir os dados de conexão ao banco de dados:

		require 'rubygems'
		require 'sinatra'
		require 'activerecord'

		ActiveRecord::Base.establish_connection(
		  :adapter => 'sqlite3',
		  :dbfile =>  'sinatra_application.sqlite3.db'
		)

Agora você pode criar e usar modelos com o ActiveRecord assim como no Rails (o exemplo pressupõe que você já tem uma tabela ‘posts’ no seu banco de dados)

		class Post < ActiveRecord::Base
		end

		get '/' do
		  @posts = Post.all()
		  erb :index
		end

Para renderizar isso em ./views/index.html:

		<% for post in @posts %>
		  <h1><% post.title %></h1>
		<% end %>
