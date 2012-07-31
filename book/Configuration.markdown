Configuração
=============

Use as opções "set" do Sinatra
------------------------------

Blocos de configuração não são executados no contexto dos eventos e também não possuem acesso as variáveis de instância. Para guardar uma informação que será acessada em suas rotas, utilize `set`.

    configure :development do
      set :dbname, 'devdb'
    end

    configure :production do
      set :dbname, 'productiondb'
    end

Você pode consultar o valor definido anteriormente através do símbolo `dbname`, do objeto `options`.

    get '/qualbanco' do
      'Você está utilizando o banco de dados: #{options.dbname}'
    end

Arquivo de configuração externa através de bloco de configuração
----------------------------------------------------------------
Em breve


Módulo da aplicação / Área de configuração
------------------------------------------
Em breve
