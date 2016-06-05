

CAPÍTULO DE SCRIPTS

CARREGANDO UM ARQUIVO DINAMICAMENTE DEFINIDO PELO USUARIO.



### Lendo arquivos csv



### Passando um nome de arquivo de forma dinâmica.

COLOCAR ESSA PARTE NUM CAPÍTULO CHAMADO "SCRIPTS"

Durante a execução do código você quer poder navegar até o diretório onde está o arquivo e selecioná-lo.

Para isso usa-se a função `file.choose()`.

Código:

````r
arquivo <- file.choose()
````

Rode esse script e você verá uma caixa de diálogo padrão do seu sistema operacional que te permite escolher o arquivo.

caixaDialogo



Suponha então que escolhamos o arquivo 'Kalimba.mp3'.

Ao fazermos isso e clicarmos em 'Open' a váriável 'arquivo' conterá o valor do caminho completo até o arquivo incluindo o nome do arquivo:

````r
> arquivo
[1] "C:\\Users\\Public\\Music\\Sample Music\\Kalimba.mp3"
````

Note que as duas barras invertidas são a forma do R representar a mudança de diretório se seu sistema operacional é Windows.

Suponha agora que queiramos separar o nome do arquivo do nome do caminho de diretórios.

Código:

````r
pedacos <- strsplit(arquivo, "\\\\")
x <- length(pedacos[[1]])
nome_arquivo <- pedacos[[1]][x]
y <- x - 1
caminho <- paste(pedacos[[1]][1:y], sep="", collapse="/")
````

Se você executar passo a passo esse script e for verificando o valor de cada variável teremos:

````r
> pedacos <- strsplit(arquivo, "\\\\")
> pedacos
[[1]]
[1] "C:" "Users" "Public" "Music" "Sample Music" "Kalimba.mp3" 

> x <- length(pedacos[[1]])
> x
[1] 6
> nome_arquivo <- pedacos[[1]][x]
> nome_arquivo
[1] "Kalimba.mp3"
> y <- x - 1
> y
[1] 5
> caminho <- paste(pedacos[[1]][1:y], sep="", collapse="/")
> caminho
[1] "C:/Users/Public/Music/Sample Music"
````

A função `strsplit` quebrou o valor da variável `pedacos` em uma lista de nomes onde cada nome estava separado por uma barra invertida dupla `\`. No código temos 4 barras invertidas porque cada barra tem que ser scaped com uma outra barra (barra invertida é um caracter especial e temos que colocar uma outra barra invertida na frente pra dizer que o próximo caracter deve ser tratado literalmente e não como um "comando").

Veja que `pedacos` é uma lista. O tamanho da lista é 6 e queremos o último elemento para capturar o nome do arquivo. É o que `pedacos[[1]][x]` faz.

Todos os outros elementos formam o caminho do arquivo então pegamos todos eles (`pedacos[[1]][1:y]`) os colamos (`paste`) de volta usando o caracter `/` como "cola". A variável `caminho` fica então com o resultado. (Experimente fazer `sep="x"`, ou qualquer outra coisa, pra ver o que acontece).

Note que usei `/` porque ela é entendida como separador de diretórios pelo R mesmo no Windows. E também pra mostrar que dá pra usar outra coisa que não o separador inicial `\`.

Ter o caminho como variável é útil porque o que você pode fazer agora é ajustar o working directory pro mesmo lugar de onde você leu o arquivo. Assim seu script fica um pouco mais flexível e você não precisa se preocupar em ficar ajustando manualmente o working directory dependendo de onde está o arquivo sendo lido.

Ou seja:

````r
> setwd(caminho)
> getwd()
[1] "C:/Users/Public/Music/Sample Music"
````




=========

### Baixando arquivos por código

Na sessão anterior forneci dois arquivos para serem usados como exemplo de arquivos csv.

Não dei detalhes de como você deveria baixar os arquivos mas é muito provável que você tenha usado seu browser internet (clicando com o botão direito do mouse, etc) e tenha salvo o arquivo em um diretório da sua escolha.

Mas e se quiséssemos ou precisássemos fazer isso de dentro do próprio script do R?

Bem vindo à função 'download.file()'.

A sintaxe da função é bem intuitiva:

url: é o parâmetro que indica onde está o arquivo.
destfile: indica onde você vai salvar o arquivo.
Se quiséssemos portanto baixar o arquivo pib1.csv do post anterior, o comando seria o seguinte:

````r
download.file(url="https://dl.dropboxusercontent.com/u/32806032/blog/pib1.csv"
,destfile="~/Documents/blog/pib1_novo.csv" # ajuste para o local onde você quer salvar o arquivo
#, method="curl" # retire o primeiro # desta linha se você está usando um Mac (OS X).
)
list.files("~/Documents/blog/")
````

Executando o código temos:

````r
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
````

Alguns comentários:

se você foi executando passo a passo deve ter percebido que após a primeira linha o R colocou um sinal de "+" na linha seguinte e ficou esperando você continuar o comando. Isso aconteceu até você de fato terminar todo o comando. Isso foi devido à sintaxe que eu usei de quebrar o comando em várias linhas. Há algumas vantagens em se fazer desta forma:
linhas de código muito compridas ficam mais fáceis de serem lidas sem a necessidade de se rolar a tela para a direita.
você consegue colocar comentários para cada parâmetro da chamada da função no final da linha (desde que você use 1 linha para cada parâmetro).
se você tiver parênteses aninhados o R Studio indenta automaticamente os parênteses e fica mais fácil de visualizar onde cada par de parênteses começa e acaba (não consegui simular aqui no post a mesma formatação que o R Studio faz...).
e um detalhe final: se você coloca a vírgula que separa cada parâmetro no começo da nova linha fica fácil de se eliminar um parâmetro temporariamente colocando um '#' na frente da linha. Foi o que fiz com o parâmetro method="curl", que deve ser usado se você está usando download.file() em um Mac (ou Linux?).
você não precisa necessariamente colocar o nome do parâmetro quando for chamar a função. Em outras palavras, você poderia usar:
````r
download.file("https://dl.dropboxusercontent.com/u/32806032/blog/pib1.csv"
, "~/Documents/blog/pib1_novo.csv"
)
````

Se você passar os parâmetros na mesma ordem que a função espera (veja o help de cada função), tudo vai dar certo. Na dúvida, coloque o nome do parâmetro...

não tirei proveito do working directory. Eu poderia ter colocado o local do arquivo de destino sem especificar o caminho completo e o arquivo seria salvo com o nome dado por mim no working directory. Por outro lado, você pode determinar um local para salvar o arquivo que não tenha nada a ver com o working directory. Ou então, você pode dar um caminho relativo ao working directory (algo como "../nomedoarquivo.csv").
Por fim, a função list.files() mostra os arquivos existentes no working directory. Nosso arquivo pib1_novo.csv está lá bem como outros arquivos que já estavam no diretório e que não estão relacionados com este post.
Por que usar download.file() e não simplesmente baixar na mão? Tudo depende da sua necessidade... Para rodar um exemplo deste blog é provável que escrever uma linha de código para baixar o arquivo seja demasiado, principalmente se você vai executar o código várias vezes para fazer algum teste (não haveria necessidade de baixar o mesmo arquivo todas as vezes). Mas se você tem que baixar uma lista de arquivos, é provável que fique muito mais fácil fazê-lo de forma mais automatizada escrevendo um script para isso. Vai do gosto e da necessidade do cidadão.

É isso.

===================

