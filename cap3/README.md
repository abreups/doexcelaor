No meu post anterior forneci dois arquivos para serem usados como exemplo de arquivos csv.

Não dei detalhes de como você deveria baixar os arquivos mas é muito provável que você tenha usado seu browser internet (clicando com o botão direito do mouse, etc) e tenha salvo o arquivo em um diretório da sua escolha.

Mas e se quiséssemos ou precisássemos fazer isso de dentro do próprio script do R?

Bem vindo à função 'download.file()'.

A sintaxe da função é bem intuitiva:

url: é o parâmetro que indica onde está o arquivo.
destfile: indica onde você vai salvar o arquivo.
Se quiséssemos portanto baixar o arquivo pib1.csv do post anterior, o comando seria o seguinte:

[code language="r"]
download.file(url= &quot;https://dl.dropboxusercontent.com/u/32806032/blog/pib1.csv&quot;
,destfile=&quot;~/Documents/blog/pib1_novo.csv&quot; # ajuste para o local onde você quer salvar o arquivo
#, method=&quot;curl&quot; # retire o primeiro # se você está usando um Mac (OS X).
)
list.files(&quot;~/Documents/blog/&quot;)
[/code]
Executando o código temos:

[code language="r" gutter="false"]
&amp;amp;gt; download.file(url= &quot;https://dl.dropboxusercontent.com/u/32806032/blog/pib1.csv&quot;
+               , destfile=&quot;~/Documents/blog/pib1_novo.csv&quot; # ajuste para o local onde você quer salvar o arquivo
+               #, method=&quot;curl&quot;
+               )
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0     0    0     0
    0     0      0      0 --:--:-- --:--:-- --:--:--     0100   129  100   129    0     0     66
  0  0:00:01  0:00:01 --:--:--    66100   129  100   129    0     0     66      0  0:00:01  0:00:01 --
:--:--    66
> list.files(&amp;amp;quot;~/Documents/blog/&amp;amp;quot;)
[1] &quot;pib1_novo.csv&quot; &quot;pib1.csv&quot;      &quot;pib2.csv&quot;
[/code]
Alguns comentários:

se você foi executando passo a passo deve ter percebido que após a primeira linha o R colocou um sinal de "+" na linha seguinte e ficou esperando você continuar o comando. Isso aconteceu até você de fato terminar todo o comando. Isso foi devido à sintaxe que eu usei de quebrar o comando em várias linhas. Há algumas vantagens em se fazer desta forma:
linhas de código muito compridas ficam mais fáceis de serem lidas sem a necessidade de se rolar a tela para a direita.
você consegue colocar comentários para cada parâmetro da chamada da função no final da linha (desde que você use 1 linha para cada parâmetro).
se você tiver parênteses aninhados o R Studio indenta automaticamente os parênteses e fica mais fácil de visualizar onde cada par de parênteses começa e acaba (não consegui simular aqui no post a mesma formatação que o R Studio faz...).
e um detalhe final: se você coloca a vírgula que separa cada parâmetro no começo da nova linha fica fácil de se eliminar um parâmetro temporariamente colocando um '#' na frente da linha. Foi o que fiz com o parâmetro method="curl", que deve ser usado se você está usando download.file() em um Mac (ou Linux?).
você não precisa necessariamente colocar o nome do parâmetro quando for chamar a função. Em outras palavras, você poderia usar:
[code language="r" gutter="false"]
download.file(&quot;https://dl.dropboxusercontent.com/u/32806032/blog/pib1.csv&quot;
, &quot;~/Documents/blog/pib1_novo.csv&quot;
)
[/code]
Se você passar os parâmetros na mesma ordem que a função espera (veja o help de cada função), tudo vai dar certo. Na dúvida, coloque o nome do parâmetro...

não tirei proveito do working directory. Eu poderia ter colocado o local do arquivo de destino sem especificar o caminho completo e o arquivo seria salvo com o nome dado por mim no working directory. Por outro lado, você pode determinar um local para salvar o arquivo que não tenha nada a ver com o working directory. Ou então, você pode dar um caminho relativo ao working directory (algo como "../nomedoarquivo.csv").
Por fim, a função list.files() mostra os arquivos existentes no working directory. Nosso arquivo pib1_novo.csv está lá bem como outros arquivos que já estavam no diretório e que não estão relacionados com este post.
Por que usar download.file() e não simplesmente baixar na mão? Tudo depende da sua necessidade... Para rodar um exemplo deste blog é provável que escrever uma linha de código para baixar o arquivo seja demasiado, principalmente se você vai executar o código várias vezes para fazer algum teste (não haveria necessidade de baixar o mesmo arquivo todas as vezes). Mas se você tem que baixar uma lista de arquivos, é provável que fique muito mais fácil fazê-lo de forma mais automatizada escrevendo um script para isso. Vai do gosto e da necessidade do cidadão.

É isso.

===================


Depois que você localizou o arquivo no disco do seu computador, resta então ler o conteúdo do arquivo.

Alguns formatos de arquivo contendo dados são mais populares que outros. Dentre os mais comuns temos:

csv: comma separated values (valores separados por vírgula). Por ser um formato que usa "texto puro" é compreendido por muitos sistemas de computadores (Windows, Unix, Mac, etc).
xls e xlsx: para os amantes de planilhas Excel, muito comum hoje em dia.
É claro que a lista é muito maior que isso, mas vou ficar por aqui porque com estes formatos já dá pra fazer muita coisa com o R.

Neste post vou explorar o tema de leitura de arquivos csv (falarei de planilhas Excel em outro post futuro).

Para ler arquivos csv basta usar a função 'read.csv()'. Essa função está baseada numa função mais genérica chamada 'read.table()', mas na 'read.csv()' alguns parâmetros já são ajustados para arquivos csv, tais como:

header = TRUE : seu arquivo cvs provavelmente vai ter a primeira linha com os nomes dos campos.
sep="," : o separador dos campos em cada linha é uma vírgula;  afinal, os campos são separados por vírgula :-)
Suponha então que você tenha um arquivo bem padrãozinho e bem comportado do tipo csv, como esse: pib1.csv

Ele tem o seguinte conteúdo:

Pais,PIB,posicao
Estados Unidos,14586736,1
China,5815501,2
Japão,5458836,3
Alemanha,3391641,4
Franca,2671113,5
Brasil,2350889,6
A primeira linha contém os nomes de cada campo (é o 'header'). As outras linhas contém a informação de:

nome do país
valor do PIB em dólares americanos
a posição no ranking mundial
Veja, eu sei tudo isso porque fui eu que criei o arquivo a partir dos dados da Wikipedia. Se você receber um arquivo csv de alguém é muito desejável que você também receba alguma explicação do que é cada campo do arquivo (um outro arquivo contendo essas explicações é conhecido por 'codebook').

Baixe o arquivo pib1.csv para um diretório do seu micro e execute o seguinte código:

[code language="r"]
setwd(&quot;~/Documents/blog&quot;) # ajuste para o local onde está o arquivo
getwd()
dados &lt;- read.csv(&quot;pib1.csv&quot;)
dados
class(dados)
[/code]
Se você executar linha a linha este código você verá:

[code language="r" gutter="false"]
> setwd(&quot;~/Documents/blog&quot;)
> getwd()
[1] &quot;/Users/pauloabreu/Documents/blog&quot;
> dados &lt;- read.csv(&quot;pib1.csv&quot;)
> dados
 Pais PIB posicao
1 Estados Unidos 14586736 1
2 China 5815501 2
3 Japão 5458836 3
4 Alemanha 3391641 4
5 Franca 2671113 5
6 Brasil 2350889 6
> class(dados)
[1] &quot;data.frame&quot;
[/code]
Ou seja, a função 'read.csv()' leu o arquivo e guardou o conteúdo numa variável do tipo 'data.frame'. Desta forma, podemos agora acessar os dados de várias formas, como por exemplo:

[code language="r" gutter="false"]

> dados$Pais
[1] Estados Unidos China Japão Alemanha Franca
[6] Brasil
Levels: Alemanha Brasil China Estados Unidos Franca Japão

[/code]
que nos dá apenas os países, ou

[code language="r" gutter="false"]

> dados$posicao &gt; 3
[1] FALSE FALSE FALSE TRUE TRUE TRUE

[/code]
que nos retorna se o valor em cada 'posição' é maior que 3 (a resposta é um vetor lógico TRUE/FALSE).

Não vou me estender aqui sobre como usar e manipular data.frames (isso é assunto para um outro post).

Só que e se o arquivo csv não for tão "bem comportado"? Por exemplo, baixe esse outro arquivo csv: pib2.csv.

Vamos ler esse arquivo assim, como se diz, "curto e grosso":

[code language="r"]

rm(list=ls())
setwd(&quot;~/Documents/blog&quot;)
getwd()
dados &lt;- read.csv(&quot;pib2.csv&quot;)
dados

[/code]
Executando passo a passo temos:

[code language="r" gutter="false"]

> rm(list=ls())
> setwd(&quot;~/Documents/blog&quot;)
> getwd()
[1] &quot;/Users/pauloabreu/Documents/blog&quot;
> dados &lt;- read.csv(&quot;pib2.csv&quot;)
Erro em read.table(file = file, header = header, sep = sep, quote = quote, :
more columns than column names
> dados
Erro: objeto 'dados' não encontrado

[/code]
O que aconteceu foi o seguinte:

primeiro limpamos todas as variáveis da memória com o comando 'rm(list=ls())' para que resultados obtidos anteriormente não possam causar confusão.
ajustamos o working directory para o local onde você baixou o arquivo pib2.csv (e conferimos com getwd, que é um passo "desnecessário" mas está aqui só por questões "didáticas").
tentamos ler o arquivo mas aí temos um erro.
e a variável dados nem pode ser criada por conta do erro.
Vamos dar uma olhada no conteúdo do arquivo pib2.csv:

# Valores de PIB em dólares americanos
# posição corresponde ao ranking mundial do país
# Fonte: http://pt.wikipedia.org/wiki/Lista_de_pa%C3%ADses_por_PIB_nominal
Pais,PIB,posicao
Estados Unidos,14586736,1
China,5815501,2
Japão,5458836,3
Alemanha,3391641,4
Franca,2671113,5
Brasil,2350889,6
As 3 primeiras linhas não são dados propriamente ditos, mas explicações a respeito dos dados que virão mais adiante (taí um "mini codebook" pra você).

Apesar de útil para sabermos, por exemplo, que o valor do PIB está em dólares americanos, não precisamos disso para a captura dos dados. O que gostaríamos de fazer é pular essas três primeiras linhas!

Existe um parâmetro na função 'read.table' (que é aproveitado nas funções derivadas dela como a read.csv) chamado 'skip'.

Dê uma olhada na documentação de read.csv digitando '?read.csv' na linha de comando do R (ou R Studio).

O que skip faz é... pular uma certa quantidade de linhas no início do arquivo!

Ajustemos nosso código para:

[code language="r" firstline="4"]

dados &lt;- read.csv(&quot;pib2.csv&quot;, skip=3)

[/code]
Executando de novo passo a passo desde o começo:

[code language="r" gutter="false"]

> rm(list=ls())
> setwd(&quot;~/Documents/blog&quot;)
> getwd()
[1] &quot;/Users/pauloabreu/Documents/blog&quot;
> dados &lt;- read.csv(&quot;pib2.csv&quot;, skip=3)
> dados
Pais PIB posicao
1 Estados Unidos 14586736 1
2 China 5815501 2
3 Japão 5458836 3
4 Alemanha 3391641 4
5 Franca 2671113 5
6 Brasil 2350889 6

[/code]
Voilà!

Pode ser que seu arquivo csv tenha também algumas linhas no final que você queira (ou precise) ignorar. O parâmetro 'nrows' pode ser usado para se determinar a quantidade total de linhas a serem lidas. Não vou dar um exemplo detalhado aqui, mas fica a dica.

É isso.