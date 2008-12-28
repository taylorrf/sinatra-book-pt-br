Handlers
========

Estrutura
---------
Handler é um termo genérico utilizado pelo Sinatra para "controllers". Um handler é o ponto inicial de entrada para uma nova requisição http em sua aplicação.

Para saber mais sobre rotas, leia o capitúlo Rotas (logo acima deste)


redirect
--------
O helper redirect é um atalho para o código (302) de resposta http comum.

Basicamente é muito fácil:

    redirect '/'

    redirect '/posts/1'

    redirect 'http://www.google.com'

Atualmente o redirect envia de volta para o browser um Location header, e o browser faz a seguinte requisição para o local indicado. Desde que o browser faça a requisição seguinte, você pode redirecionar para outra página, em sua aplicação, ou totalmente para um outro site.


O fluxo de  requests durante um redirect é:
Browser --> Server (redirect to '/') --> Browser (request '/') --> Server (result for '/')

Sinatra envia por padrão o código de resposta 302. Segundo o spec, 302 não pode alterar o método de requisição, mas você vê uma nota na maioria dos clientes dizendo que irá mudar isto. Aparentemente os browsers mobile que as pessoas estão usando fazem as coisas corretamente ( exceto a má interpretação do mainstream).


A solução disto no spec é enviar 2 diferentes códigos de resposta: 303 e  307. 303 reinicia o GET, 307 mantém o mesmo método.

Para forçar o sinatra a enviar um código de resposta diferente, é muito simples:

    redirect '/', 303 # força o retorno do código 303

    redirect '/', 307 # força o retorno do código 307

sessões
--------

### Padrão de Cookie baseado em Sessões

Sinatra possui um suporte básico para cookie baseado em sessão.  Para habilitar isto, no bloco de configuração, ou  no inicio da sua aplicação, você precisa habilitar esta opção.

    enable :sessions

A desvantagem para esta abordagem com sessão é que todos os dados serão armazenados no cookie. Cookies sempre terão um limite de 4 kilobytes, você nao pode armazenar muitos dados. Por outro lado cookies não são inviolaveis. O usuário pode modificar qualquer dado na sua sessão. Mas.... é muito fácil, e não tem problemas de escalonamento de memória ou de banco de dados rodando de backend junto com a sessão.


### Memória baseada em sessões

### Memcached baseado em sessões

### Arquivos baseados em sessões

### Banco de dados baseados em sessões


cookies
-------

Cookies são bastante simples de se usar no Sinatra, porém com alguns trejeitos.

Vamos primeiro olhar um simples caso de uso:

    require 'sinatra'

    get '/' do
        # Get the string representation
        cookie = request.cookies["thing"]

        # Set a default
        cookie ||= 0

        # Convert to an integer
        cookie = cookie.to_i

        # Do something with the value
        cookie += 1

        # Reset the cookie
        set_cookie("thing", cookie)

        # Render something
        "Thing is now: #{cookie}"
    end

Setting a path, expiration date, or domain gets a little more complicated - see the source code for set\_cookie if you want to dig deeper.

    set_cookie("thing", { :domain => myDomain,
                          :path => myPath,
                          :expires => Date.new } )

That's the easy stuff with cookies - It can also serialize Array objects,
separating them with ampersands (&), but when they come back, it doesn't
deserialize or split them in any way, it hands you the raw, encoded string
for your parsing pleasure.


status
------

If you want to set your own status response instead of the normal 200 (Success), you can use the `status`-helper to set the
code, and then still render normally:

    get '/' do
      status 404
      "Não funciona"
    end

Alternatively you can use `throw :halt, [404, "Not found"]` to immediately stop any further actions and return the
specified status code and string to the client. `throw` supports more options in this regard, see the appropriate section
for more info.


autenticação
--------------

