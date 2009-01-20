Configuração
=============

Use as opções "set" do Sinatra
-----------------------------------
Blocos de configuração não são executados no contexto dos eventos e não possuem acesso as variaveis de instância. Para guardar um pedaço de informação que você queira acessar nas suas rotas, utilize `set`.

    configure :development do
      set :dbname, 'devdb'
    end

    configure :production do
      set :dbname, 'productiondb'
    end

...

    get '/qualbanco' do
      'Você esta utilizando o banco de dados ' + options.dbname
    end

Arquivo de configuração externa através do bloco de configuração
----------------------------------------------------------------
em breve


Módulo da aplicação / Área de configuração
------------------------------------------
em breve


