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

Definir um diretório, data de expiração ou dominio requer um pouco mais complicação - veja o código-fonte do set\_cookie se você quiser ir mais mais a fundo.

    set_cookie("thing", { :domain => myDomain,
                          :path => myPath,
                          :expires => Date.new } )

Estas são as coisas mais simples com cookies - Você pode também serializar um objeto de Arrays, separando com "e" comercial (&), mas quando obter de volta, terá que desserializar e dividi-lo de qualquer forma, uma grande mão de obra, codificando uma string e depois a parseando-a para o seu prazer.


status
------

Se você deseja definir seu próprio status para resposta ao invés do convencional 200 (Successo), você pode usar o helper `status`- para definir o código, e ainda assim renderizar normalmente:

    get '/' do
      status 404
      "Não funciona"
    end


Alternativamente você pode usar `throw :halt, [404, "Not found"]` para imediatamente parar futuras ações e retornar o código de status especificado e uma string para o cliente. `throw` suportam mais opções a respeito disto, consulte o capítulo adequado para mais informações.

autenticação
--------------

