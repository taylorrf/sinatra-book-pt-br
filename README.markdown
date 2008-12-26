Sinatra Book
============

Documentação do "livro" para o Web Framework Sinatra.

Sobre
-----
Isto provavelmente irá servir mais como um "livro" no estilo receitas de bolo, mais linear, juntamente com um tutorial para você começar.

Junte-se a nós no IRC (irc.freenode.org #sinatra) se você quiser ajudar com alguma coisa.

Layout dos arquivos:

* _book_   - Text of the book.  In maruku's markdown format.
* _images_ - Images, Diagrams, Funny Pictures
* _source_ - Any source examples to be included into the book

Learn more about Sinatra at [http://github.com/bmizerany/sinatra](http://github.com/bmizerany/sinatra).


Como gerar o livro
---------------------

Você irá precisar da seguinte gem para gerar o livro:

    sudo gem install thor maruku

In the root directory, launch the following Thor task:

    thor book:build

How to contribute
-----------------

Fork this repository, be sure to read the [styleguide](http://github.com/cschneid/sinatra-book/wikis/how-to-contribute) and post pull requests.
