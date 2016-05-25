Tomei conhecimento da linguagem R enquanto procurava na Web uma forma de plotar um mapa do Brasil. Deparei-me com um documento chamado "R para Cientistas Sociais" que provavelmente já deve ter sido usado por muita gente dada a clareza e concisão com que explica muitas coisas sobre o R. (Esse link já aponta para a versão nova do documento que, me parece, virou até livro! Meu contato foi com uma versão do documento mais antiga, que era um PDF puro e simples, sem capa nem nada. Parabéns ao Jakson Alves de Aquino!).

O primeiro passo é conseguir um arquivo do tipo Shapefile. Sem nos alongarmos no assunto (até porque não tenho conhecimento suficiente para me aprofundar no tema), shapefile é um formato de arquivo que contém dados "vetoriais" (pontos, linhas e polígonos) que representam várias coisas geográficas, tais como fronteiras, rios, etc. Quando você baixa um mapa nesse formato ele normalmente vem com outros arquivos de suporte que contém um "mini banco de dados" com os nomes das coisas que estão representadas no shapefile. Por exemplo, para um mapa de rios os arquivos de suporte dão os nomes dos rios.

Passemos da "teoria" para a prática. Se você fizer uma pesquisa na Internet atrás de shapefiles você certamente vai encontrar muitas fontes de informação. Sugiro o seguinte site que possui mapas de todos os países do mundo (se não todos, muitos): http://gadm.org.

Siga as orientações do site e baixe o mapa do Brasil. No meu caso o link para download do mapa shapefile do Brasil foi esse: http://biogeo.ucdavis.edu/data/gadm2/shp/BRA_adm.zip. O arquivo tem 17,9 Mbytes e depois de descompactado num diretório qualquer seu conteúdo é composto de 21 arquivos sendo 20 deles arquivos com os seguintes nomes:

BRA_admx.csv
BRA_admx.dbf
BRA_admx.prj
BRA_admx.shp
BRA_admx.shx
onde o 'x' é 0, 1, 2 ou 3, e o 21 arquivo é o 'read_me.pdf'.
Veja que há o arquivo com a extensão '.shp'; esse é justamente o tal arquivo shapefile. Todos os demais arquivos são os arquivos "de suporte".

Vamos então carregar os shapefiles:

[code language="r"]
library(maptools)
setwd(&quot;~/Documents/blog/mapaBrasil&quot;) # altere para o local do seu mapa
mapa0 &lt;- readShapeSpatial(&quot;BRA_adm0.shp&quot;)
mapa1 &lt;- readShapeSpatial(&quot;BRA_adm1.shp&quot;)
mapa2 &lt;- readShapeSpatial(&quot;BRA_adm2.shp&quot;)
mapa3 &lt;- readShapeSpatial(&quot;BRA_adm3.shp&quot;)
[/code]
Para você plotar um mapa basta usar o comando 'plot()', assim:

[code language="r" gutter="false"]
plot(mapa0)
[/code]
e você terá o mapa do Brasil com apenas as fronteiras internacionais:

brasil0

se você plotar o mapa1 o resultado já é um pouco diferente:

[code language="r" gutter="false"]
plot(mapa1)
[/code]
resultado:

brasil1



Repare que o mapa1 já contém as fronteiras dos Estados.
Não vou colocar a figura aqui, mas plote também o mapa2 (fronteiras dos municípios) e o mapa3 (fronteira com distritos - menor que município!).
O mapa3 mal dá pra ver as regiões plotadas, tamanha a quantidade de detalhes!

OK, mas como é que a gente sabe o que cada mapa contém?
Pra ver isso podemos explorar a estrutura dos dados de cada shapefile com o comando 'str()' (vou usar o mapa1 que é mais interessante que o mapa0, mas você pode dar uma olhada no que acontece com os outros mapas também):

[code language="r" gutter="false"]
> str(mapa1@data)
'data.frame':	27 obs. of  9 variables:
 $ ID_0     : int  32 32 32 32 32 32 32 32 32 32 ...
 $ ISO      : Factor w/ 1 level &quot;BRA&quot;: 1 1 1 1 1 1 1 1 1 1 ...
 $ NAME_0   : Factor w/ 1 level &quot;Brazil&quot;: 1 1 1 1 1 1 1 1 1 1 ...
 $ ID_1     : int  1 2 3 4 5 6 7 8 9 10 ...
 $ NAME_1   : Factor w/ 27 levels &quot;Acre&quot;,&quot;Alagoas&quot;,..: 1 2 3 4 5 6 7 8 9 10 ...
 $ NL_NAME_1: Factor w/ 0 levels: NA NA NA NA NA NA NA NA NA NA ...
 $ VARNAME_1: Factor w/ 13 levels &quot;Amazone&quot;,&quot;Ba\xa1a&quot;,..: NA NA NA 1 2 NA NA 3 4 12 ...
 $ TYPE_1   : Factor w/ 2 levels &quot;Distrito Federal&quot;,..: 2 2 2 2 2 2 1 2 2 2 ...
 $ ENGTYPE_1: Factor w/ 2 levels &quot;Federal District&quot;,..: 2 2 2 2 2 2 1 2 2 2 ...
 - attr(*, &quot;data_types&quot;)= chr  &quot;N&quot; &quot;C&quot; &quot;C&quot; &quot;N&quot; ...
>
[/code]
A coluna mais interessante é a NAME_1, que parece conter os nomes dos Estados.
Vamos dar uma olhada no conteúdo dela com:

[code language="r" gutter="false"]
> mapa1@data$NAME_1
 [1] Acre                Alagoas             Amap\xe1            Amazonas
 [5] Bahia               Cear\xe1            Distrito Federal    Esp\xedrito Santo
 [9] Goi\xe1s            Maranh\xe3o         Mato Grosso do Sul  Mato Grosso
[13] Minas Gerais        Par\xe1             Para\xedba          Paran\xe1
[17] Pernambuco          Piau\xed            Rio de Janeiro      Rio Grande do Norte
[21] Rio Grande do Sul   Rond\xf4nia         Roraima             Santa Catarina
[25] S\xe3o Paulo        Sergipe             Tocantins
27 Levels: Acre Alagoas Amap\xe1 Amazonas Bahia Cear\xe1 Distrito Federal ... Tocantins
>
[/code]
Se os nomes dos Estados que contém acentos (como Amapá) não aparecem corretamente, não se desespere, é apenas uma questão de "encoding". No nosso caso você pode resolver isso usando o comando 'iconv()' (veja o help do iconv para detalhes):

[code language="r" gutter="false"]
> iconv(x, from=&quot;latin1&quot;, to=&quot;UTF8&quot;)
 [1] &quot;Acre&quot;                &quot;Alagoas&quot;             &quot;Amapá&quot;               &quot;Amazonas&quot;
 [5] &quot;Bahia&quot;               &quot;Ceará&quot;               &quot;Distrito Federal&quot;    &quot;Espírito Santo&quot;
 [9] &quot;Goiás&quot;               &quot;Maranhão&quot;            &quot;Mato Grosso do Sul&quot;  &quot;Mato Grosso&quot;
[13] &quot;Minas Gerais&quot;        &quot;Pará&quot;                &quot;Paraíba&quot;             &quot;Paraná&quot;
[17] &quot;Pernambuco&quot;          &quot;Piauí&quot;               &quot;Rio de Janeiro&quot;      &quot;Rio Grande do Norte&quot;
[21] &quot;Rio Grande do Sul&quot;   &quot;Rondônia&quot;            &quot;Roraima&quot;             &quot;Santa Catarina&quot;
[25] &quot;São Paulo&quot;           &quot;Sergipe&quot;             &quot;Tocantins&quot;
>
[/code]
Para não termos que ficar convertendo toda hora os nomes dos Estados (e facilitar outras tarefas) vamos converter e sobrescrever os dados do mapa1:

[code language="r" gutter="false"]
> mapa1@data$NAME_1 &lt;- iconv(mapa1@data$NAME_1, from=&quot;latin1&quot;, to=&quot;UTF8&quot;)
> mapa1@data$NAME_1
 [1] &quot;Acre&quot;                &quot;Alagoas&quot;             &quot;Amapá&quot;               &quot;Amazonas&quot;
 [5] &quot;Bahia&quot;               &quot;Ceará&quot;               &quot;Distrito Federal&quot;    &quot;Espírito Santo&quot;
 [9] &quot;Goiás&quot;               &quot;Maranhão&quot;            &quot;Mato Grosso do Sul&quot;  &quot;Mato Grosso&quot;
[13] &quot;Minas Gerais&quot;        &quot;Pará&quot;                &quot;Paraíba&quot;             &quot;Paraná&quot;
[17] &quot;Pernambuco&quot;          &quot;Piauí&quot;               &quot;Rio de Janeiro&quot;      &quot;Rio Grande do Norte&quot;
[21] &quot;Rio Grande do Sul&quot;   &quot;Rondônia&quot;            &quot;Roraima&quot;             &quot;Santa Catarina&quot;
[25] &quot;São Paulo&quot;           &quot;Sergipe&quot;             &quot;Tocantins&quot;
>
[/code]
Agora vamos a um "pulo do gato": e se quisermos plotar somente um Estado, por exemplo, o Estado de São Paulo?
Primeiro temos que fazer um "filtro" (ou subconjunto, ou subset) só para o Estado de São Paulo (leia o artigo no post anterior).

[code language="r" gutter="false"]
sp &lt;- mapa1@data$NAME_1 == &quot;São Paulo&quot;
[/code]
depois fazemos o subset do mapa só onde ele for TRUE:

[code language="r" gutter="false"]
mapa_sp &lt;- mapa1[sp,]
[/code]
e plotamos com:

[code language="r" gutter="false"]
plot(mapa_sp)
[/code]
Você pode fazer tudo isso numa linha só, assim:

[code language="r" gutter="false"]
plot( mapa1[ mapa1@data$NAME_1 == &quot;São Paulo&quot;  ,] )
[/code]
Resultado:
saopaulo

Pra finalizar, vamos plotar o mapa do Brasil, o Estado de São Paulo em vermelho e o Estado de Minas Gerais em Azul. Veja como fazemos isso:

[code language="r" gutter="false"]
plot(mapa1)
plot( mapa1[ mapa1@data$NAME_1 == &quot;São Paulo&quot;  ,] , add=TRUE, col=&quot;red&quot;)
plot( mapa1[ mapa1@data$NAME_1 == &quot;Minas Gerais&quot;  ,] , add=TRUE, col=&quot;blue&quot;)
[/code]
Resultado:

brasil_sp_minas_color

A "mágica" foi feita graças aos parâmetros 'add' (para adicionar o plot ao plot anterior, ao invés de apagar o anterior e fazer um novo) e 'col' (que indica a cor a ser usada no preenchimento).
Como sempre os helps das funções 'plot()' e 'par()' (para ver os parâmetros que se pode usar) são muito úteis.

É isso.
