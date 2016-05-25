Quem já usou (ou ainda usa) o Microsoft Excel para trabalhar com tabelas (aka planilhas) provavelmente já está bem acostumado ao uso dos filtros do Excel. Realmente é muito fácil de se utilizar: selecione uma coluna ou várias colunas, clique no botão de "filtros" na barra de ferramentas e pronto; você já pode começar a fazer subconjuntos dos dados. Por exemplo, você clica naquela flechinha ao lado do nome da coluna seleciona apenas alguns casos; depois vai para outra coluna e seleciona um subconjunto dos dados já parcialmente filtrados. Realmente muito simples e conveniente. Mas esse não é um post sobre Excel :-) Como fazer isso no R?

Vamos começar com um conjunto de dados, como por exemplo esse: https://dl.dropboxusercontent.com/u/32806032/blog/pib3.csv

[code language="r"]
link &lt;- &quot;https://dl.dropboxusercontent.com/u/32806032/blog/pib3.csv&quot;
setwd(&quot;~/Documents/blog&quot;) # ajuste conforme seu caso
destino &lt;- &quot;pib3.csv&quot;
download.file(url = link
             , destfile = destino
             #, method = &quot;curl&quot; # apenas se você está usando um Mac (OS X).
             )
[/code]
Depois carregamos o arquivo baixado numa variável:

[code language="r" firstline="8"]
dados &lt;- read.csv(&quot;pib3.csv&quot;, skip=3) # as 3 primeiras linhas não nos interessam
                                      # abra o arquivo num editor simples e veja
[/code]
E se dermos uma olhada na variável 'dados' teremos:

[code language="r" gutter="false"]
> dados
  Continente           Pais      PIB posicao
1   Américas Estados Unidos 14586736       1
2       Ásia          China  5815501       2
3       Ásia          Japão  5458836       3
4     Europa       Alemanha  3391641       4
5     Europa         Franca  2671113       5
6   Américas         Brasil  2350889       6
>
[/code]
Muito bem. E se você quiser ver apenas os países da Ásia?
Bem, no Excel você marcaria a coluna de 'Continente' (ou todas as colunas), clicaria na tal seta (que abre uma "drop down list") e escolheria apenas "Ásia". Esta operação toda de "point and click" no Excel está criando uma condição lógica mais ou menos assim:

[code language="r" gutter="false"]
&quot;Continente&quot; tem que ser igual a &quot;Ásia&quot;
[/code]
E essa condição vai ser 'verdadeira' ou 'falsa' para cada linha de 'dados'.
Podemos então criar uma condição lógica no R assim:

[code language="r" firstline="10"]
cond &lt;- dados$Continente == &quot;Ásia&quot;
[/code]
A sintaxe dados$Continente indica que estamos nos referindo à coluna Continente da variável dados. A variável dados é um data frame, que você pode imaginar como sendo uma tabela, ou seja, está organizada em linhas e colunas, exatamente como uma planilha Excel. Para acessar qualquer coluna da "tabela" dados basta colocar o caracter $ e depois o nome da coluna. Por exemplo, para acessar a coluna Pais escreveríamos dados$Pais. E assim por diante.

A sintaxe == (isso mesmo, são dois caracteres '=') indicam que queremos fazer uma comparação lógica (verdadeiro ou falso) entre o que está à esquerda de == com o que está à direita de ==. No caso da tabela dados, isso vai ser feito linha a linha. Ou seja: na linha 1 da coluna Continente temos "Ásia"? Não? Então a resposta é FALSE (falso). E na linha 2? É? Então a resposta é TRUE. E na linha 3? Também é? Então a resposta para a linha 3 é outro TRUE. Etc, etc, etc, até a última linha.
O resultado para todas as linhas de dados é portanto: FALSE, TRUE, TRUE, FALSE, FALSE, FALSE.
Vamos ver? Executando e olhando o valor de cond temos:

[code language="r" gutter="false"]
> cond &lt;- dados$Continente == &quot;Ásia&quot;
> cond
[1] FALSE  TRUE  TRUE FALSE FALSE FALSE
>
[/code]
Bom, e daí? Não queremos ver um monte de FALSEs e TRUEs; queremos ver as linhas filtradas.
Para isso utilizamos a variável cond para selecionar quais as linhas de dados que queremos ver. A idéia é a seguinte: se quisermos ver a linha 1 e coluna 2 de dados escrevemos dados[1,2]. Se quisermos ver a linha 3 e todas as colunas, escrevemos dados[3,TRUE].
O que estamos fazendo é usar a sintaxe de [linha,coluna] e sempre que passarmos um valor que não seja FALSE como linha ou como coluna o R vai mostrar o conteúdo.
A variável cond já tem a informação de quais linhas queremos ver (TRUE) e quais não nos interessam (FALSE). Que tal então se escrevermos dados[cond,TRUE]? O R vai pegar cada valor de cond e considerar que queremos (ser for TRUE) ou não queremos (se for FALSE) ver aquela linha. E como temos TRUE para a coluna, o R vai mostrar todas as colunas.
Tenta aí:

[code language="r" gutter="false"]
> dados[cond,TRUE]
  Continente  Pais     PIB posicao
2       Ásia China 5815501       2
3       Ásia Japão 5458836       3
>
[/code]
Nota: você não precisa colocar TRUE na coluna. Basta deixar vazio e o R vai subentender que você quer ver todas as colunas:

[code language="r" gutter="false"]
> dados[cond,]
  Continente  Pais     PIB posicao
2       Ásia China 5815501       2
3       Ásia Japão 5458836       3
>
[/code]
A mesma sintaxe vale para a linha. se você quisesse ver todas as linhas mas só a coluna 3 você poderia escrever:

[code language="r" gutter="false"]
> dados[,3]
[1] 14586736  5815501  5458836  3391641  2671113  2350889
>
[/code]
Uma outra forma seria:

[code language="r" gutter="false"]
> dados[,&quot;PIB&quot;]
[1] 14586736  5815501  5458836  3391641  2671113  2350889
>
[/code]

onde você explicitamente dá o nome da coluna.

Finalmente podemos guardar a tabelinha só da Ásia em uma variável, assim:

[code language="r" firstline="11"]
asia &lt;- dados[cond,]
[/code]
E pra conferir executamos e espiamos o valor de asia:

[code language="r" gutter="false"]
> asia &lt;- dados[cond,]
> asia
  Continente  Pais     PIB posicao
2       Ásia China 5815501       2
3       Ásia Japão 5458836       3
>
[/code]
Você pode seguir fazendo mais subconjuntos (mais "filtros"). Por exemplo, quais países da Ásia tem PIB maior que 5,5 bilhões de dólares, ou qualquer coisa do gênero. Mas vou deixar os detalhes para outro post.

O código completo desse post é esse:

[code language="r"]
link &lt;- &quot;https://dl.dropboxusercontent.com/u/32806032/blog/pib3.csv&quot;
setwd(&quot;~/Documents/blog&quot;) # ajuste conforme seu caso
destino &lt;- &quot;pib3.csv&quot;
download.file(url = link
             , destfile = destino
             #, method = &quot;curl&quot; # apenas se você está usando um Mac (OS X).
             )
dados &lt;- read.csv(&quot;pib3.csv&quot;, skip=3) # as 3 primeiras linhas não nos interessam
                                      # abra o arquivo num editor simples e veja
cond &lt;- dados$Continente == &quot;Ásia&quot;
[/code]
É isso.
