Deployment
==========

Lighttpd Proxied to Thin        {#deployment_lighttpd}
------------------------

Iremos cobrir aqui como realizar o deploy no Sinatra com suporte a proxy reverso e load balancing utilizando Lighttpd e Thin.

1. Instalando Lighttpd e Thin

        # Verifique se você já tem lighttpd, isto pode ser verificado através do gerenciado de pacotes da sua distribuição Linux

        # Para o Thin:
        gem install thin

2. Crie seu arquivo de rackup - é necessario que tenha a linha "require 'app'", equivalente a sua aplicação Sinatra.

        require 'app'

        set :env,       :production
        set :port,      4567
        disable :run, :reload

        run Sinatra.application

3. Configure o config.yml - mude o /local/da/minha/app para o diretório correto da sua aplicação.

        ---
            environment: production
            chdir: /local/da/minha/app
            address: 127.0.0.1
            user: root
            group: root
            port: 4567
            pid: /local/da/minha/app/thin.pid
            rackup: /local/da/minha/app/config.ru
            log: /local/da/minha/app/thin.log
            max_conns: 1024
            timeout: 30
            max_persistent_conns: 512
            daemonize: true

4. Configure o lighttpd.conf - troque "mydomain" pelo endereço correto. Também marque corretamente a porta primária conforme configurado no  config.yml.

         $HTTP["host"] =~ "(www\.)?mydomain\.com"  {
                 proxy.balance = "fair"
                 proxy.server =  ("/" =>
                                         (
                                                 ( "host" => "127.0.0.1", "port" => 4567 ),
                                                 ( "host" => "127.0.0.1", "port" => 4568 )
                                         )
                                 )
         }

5. Inicie o thin e a sua aplicação. Eu criei um script que já faz isso
   chamado de "rake start".

         thin -s 2 -C config.yml -R config.ru start

Está feito! Vá até o seu domínio "mydomain.com/" e veja o resultado! Tudo pode ser configurado agora, verifique também a configuração do seu domínio no seu arquivo lighttpd.conf.

*Variações* - nginx via proxy - O mesmo acesso do proxy pode ser aplicado ao web server nginx

	upstream www_mydomain_com {
		server 127.0.0.1:5000;
		server 127.0.0.1:5001;
	}

	server {
		listen		www.mydomain.com:80
		server_name	www.mydomain.com live;
		access_log /path/to/logfile.log

		location / {
			proxy_pass http://www_mydomain_com;
		}

	}

*Variações* - Mais instâncias Thin - Para adicionar mais instâncias thin, mude o paramêtro `-s 2` no comando de inicialização do thin para quantos servidores você deseja.
E não se esqueça dos proxies lighttpd, adicionando uma nova linha para cada um deles. Após, reinicie o lighttpd e tudo irá subir conforme o esperado.



Passenger (mod rails)           {#deployment_passenger}
------------------------
Odeia fazer deployment com FastCGI? Você não está sozinho.  Pois advinhe só, Passenger tem suporte a Rack;
e este livro irá lhe dizer como fazer tudo isso.

Você pode encontrar a documentação adicional sobre Passenger no seu repositório no Github.

1. Configurando através da interface de conta na Dreamhost

        Domains -> Manage Domains -> Edit (web hosting column)
        Habilite 'Ruby on Rails Passenger (mod_rails)'
        Adicione o diretório "public" no campo para diretórios web. Caso você esteja usando 'rails.com', isto irá mudar para 'rails.com/public'
        Salve suas alterações

2. Criando a estrutura de diretórios

        domain.com/
        domain.com/tmp
        domain.com/public
        # local para uma versão do Sinatra - não necessário caso estiver usando gems
        domain.com/sinatra

3.  Criando o arquivo de "Rackup" (rack configuration file) `config.ru`

        # Este arquivo irá ficar em domain.com/config.ru
        require 'sinatra/lib/sinatra.rb'   # "require 'sinatra'" caso tenha sido instalado via gems
        require 'rubygems'

        require 'test.rb' # assuma que o arquivo da sua aplicação Sinatra seja 'test.rb'

        set :env,  :production
        disable :run

        run Sinatra.application


4. Uma aplicação Sinatra bastante simples

        # este é o arquivo test.rb citado acima
        get '/' do
                "Trabalhando na dreamhost"
        end

        get '/foo/:bar' do
                "Você requisitou por foo/#{params[:bar]}"
        end



E isso é tudo que necessitamos por enquanto! Uma vez tudo configurado, acesse seu domínio e você já poderá ver a página "Trabalhando na dreamhost". Para reiniciar a aplicação após algumas mudanças, você precisará executar `touch tmp/restart.txt`.

Observe que na versão atual do passenger (2.0.3) existe um bug onde o Sinatra não encontra o diretório das views. Neste caso, adicione a opção `:views => '/diretório/das/views/'` no seu arquivo Rackup do Sinatra.

Nota complementar: algumas documentações terão um formato diferente de passar as opções no arquivo Rackup do Sinatra, como por exemplo:

		Sinatra::Application.default_options.merge!(
		  :run => false,
		  :env => :production,
		  :raise_errors => true
		)



FastCGI                         {#deployment_fastcgi}
-------

O método padrão para fazer deployment é usando Thin ou Mongrel, e ter um proxy reverso (lighttpd, nginx ou mesmo Apache) apontando para o seus servidores.


Mas nem sempre isto é possível.  Em hosts baratos e compartilhados (como Dreamhost) você não pode ter rodando Thin ou Mongrel, ou configurar proxy reverso (pelo menos no plano padrão compartilhado).

Felizmente, Rack suporta varias conexões, incluindo CGI e FastCGI.  Infelizmente para nós, FastCGI não trabalha muito bem com a versão atual do Sinatra.

Para enviar um simples 'hello world' com uma aplicação Sinatra rodando na Dreamhost
é necessário baixar todo o código atual do Sinatra, e hackear ele um pouco.  Não se preocupe, isto somente requer comentar algumas linhas e ajustar algumas outras.

Passos para um deploy com FastCGI:

* .htaccess
* dispatch.fcgi
* Tweaked sinatra.rb


1. .htaccess
        RewriteEngine on

        AddHandler fastcgi-script .fcgi
        Options +FollowSymLinks +ExecCGI

        RewriteRule ^(.*)$ dispatch.fcgi [QSA,L]

2. dispatch.fcgi
        #!/usr/bin/ruby

        require 'sinatra/lib/sinatra.rb'
        require 'rubygems'

        fastcgi_log = File.open("fastcgi.log", "a")
        STDOUT.reopen fastcgi_log
        STDERR.reopen fastcgi_log
        STDOUT.sync = true

        set :logging, false
        set :server, "FastCGI"

        module Rack
          class Request
            def path_info
              @env["REDIRECT_URL"].to_s
            end
            def path_info=(s)
              @env["REDIRECT_URL"] = s.to_s
            end
          end
        end

        load 'test.rb'

3. sinatra.rb - Modificar esta função com uma nova versão aqui (descomentando as linhas `puts`)

        def run
          begin
            #puts "== Sinatra começou os trabalhos na porta #{port} para #{env} apoiado pelo #{server.name}"
            require 'pp'
            server.run(application) do |server|
              trap(:INT) do
                server.stop
                #puts "\n== Sinatra já terminou sua configuração (e a multidão aplaude)"
              end
            end
          rescue Errno::EADDRINUSE => e
            #puts "== Algo já está sendo executando na porta #{port}!"
          end
        end

Heroku
------

[Heroku] já tem um suporte básico para aplicações em Sinatra. Esta é provavelmente a opção mais fácil de deployment desde que configurada corretamente, deploying no heroku torna-se basicamente uma questão de um simples git push.

Passos para deploy no Heroku:

* crie o config/rackup.ru
* git push

1. um exemplo de arquivo rackup:

        require File.dirname(__FILE__) + "/../my_sinatra_app"

        set     :app_file, File.expand_path(File.dirname(__FILE__) + '/../my_sinatra_app.rb')
        set     :public,   File.expand_path(File.dirname(__FILE__) + '/../public')
        set     :views,    File.expand_path(File.dirname(__FILE__) + '/../views')
        set     :env,      :production
        disable :run,      :reload

        run Sinatra.application

2. git push

        $ git remote add heroku git@heroku.com:my-sinatra-app.git
        $ git push heroku master

[Heroku]: http://www.heroku.com

Fuzed e Amazon
----------------
// TODO: Conversar com o Blake sobre isso

Poolparty e Amazon EC2
------------------------


// TODO: Quais outras estratégias de deployment são usadas lá?

