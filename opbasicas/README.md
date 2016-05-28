# Operações Básicas
Agora que você já tem o R e o R Studio instalados no seu computador
vamos falar um pouco de alguns conceitos básicos que você tem que 
entender para começar a usar este ambiente.

## A Console

Tudo o que você digitar na console será executado assim que 
você bater na tecla Enter. Por exemplo, digite na console:

````r
> 1+2
````

tecle Enter e o resultado aparecerá:

````r
[1] 3
````

Seu console deve estar provavelmente assim:

![Console](console1.png)


não se preocupe com esse 1 entre colchetes; ele é apenas a forma
do R indicar a saída do resultado. O que nos interessa é o 3, que 
é o resultado da soma.

Você pode fazer um monte de operações matemáticas diretamente na
linha de comando. Experimente tirar a raiz quadrade de 2 e multiplicar
pelo cubo de 3:

````r
> sqrt(2) * 8**3
[1] 724.0773
````

todas as regras que você aprendeu na escola sobre a precedência de 
algumas operações sobre outras vale aqui também. No caso, a potenciação
é calculada antes da multiplicação. Mas você pode colocar parênteses
para tornar a precedência explícita. Por exemplo:

````r
> 3 + 2 * 4 + 1
[1] 12
> (3 + 2) * (4 + 1)
[1] 25
````

No primeiro caso a multiplicação tem precedência, então nos restou a soma
`3 + 8 + 1`. No segundo caso, os parênteses forçaram que as somas fossem
calculadas antes e ficamos com a operação `5 * 5`.

A propósito, os espaços em branco entre os números e os sinais não afetam
o reultado. Você poderia ter digitado `3+2*4    +    1` que o resultado seria
o mesmo.

Aqui vai a primeira dica de utilização da Console: pressione a seta para cima
no seu teclado e a Console mostrará o último comando executado. Se você teclar
Enter o comando será executado. Se, ao invés de teclar Enter, você pressionar
novamente a tecla para cima, a Console mostrará o penúltimo comando executado.
Vá pressionando várias vezes a tecla para cima (e tente a seta para baixo
também) para navegar pelo histórico de comandos que você executou.

Isso é muito conveniente quando você digita um comando bem comprido e ao
teclar o Enter a Cosnole te dá um erro. Por exemplo, digite o comando a
seguir exatamente como mostrado:

````r 
> qrt(2)
Erro: não foi possível encontrar a função "qrt"
````

O R não conhece essa tal função `qrt`. Opa, era `sqrt`! A função
que calcula a raiz quadrada de seu argumento (no caso, o número 2).
Seta para cima, seta para a esquerda 6 vezes (até o início do comando),
digite o `s` que faltava, e Enter.

````r
> sqrt(2)
[1] 1.414214
````

OK, OK. O comando não era tão comprido assim e você poderia ter digitado 
tudo de novo, mas você entendeu a idéia, certo?

Você pode usar a seta para cima para consultar o histórico de comandos, mas
há uma outra forma. O R Studio possui no canto superior esquerdo
uma área que contém todo o histórico de comandos executados, conforme
ilustrado na figura a seguir:

![Histórico](history.png)

Coloque o mouse sobre cada um dos ícones e veja o texto de ajuda que
aparece. Ele é bem autoexplicativo e você pode brincar à vontade para
ver o que acontece.

Mas vamos em frente.


## Variáveis

No Excel (ou qualquer outro software de planilha eletrônica) a 
forma usual de se armazenar um valor para poder utilizá-lo mais
tarde é colocar o valor numa célula. Essa operação é quase que
intuitiva para quem usa planilhas eletrônicas. Você guarda um certo
valor na célula A1, um outro valor na célula B1 e na célula C1 você
coloca uma fórmula que, por exemplo, soma os valores.

No R você guarda valores em variáveis. Uma variável é um "nome", uma 
palavra que tem que começar com uma letra (não pode começar com um
número). `banana` é uma palavra que começa com uma letra (`b`), portanto
pode ser o nome de uma variável. `3tigres` não pode ser nome de 
variável, pois começa com um número (`3`).

Você guarda um valor na variável `banana` através das seguintes sintaxes:

- sintaxe 1: 

```r
> banana = 1
```

- sintaxe 2:

```r
> banana <- 1
```

Para descobrir qual valor está guardado na variável `banana`, basta
digitar o nome da variável na console e bater Enter:

````r
> banana
[1] 1
````

A sintaxe 2 é a mais utilizada quando você pesquisa sobre código
em R na Internet, então é a que utilizaremos nesse texto.

Vamos guardar um outro valor em outra variável: `laranja`:

````r
> laranja <- 2
````

E novamente, para vermos o valor que está guardado na variável 
`laranja`, escrevemos seu nome na console e Enter

````r
> laranja
[1] 2
````

Agora podemos fazer como fazíamos na planilha: podemos fazer uma operação 
utilizando as variáveis `banana` e `laranja`. Veja os seguintes
exemplos:

````r
> banana + laranja
[1] 3
> banana / laranja
[1] 0.5
> banana**laranja
[1] 1
> laranja ** banana
[1] 2
> sin(laranja) / cos(banana)
[1] 1.682942
> banana == laranja
[1] FALSE

````

Temos uma soma, uma divisão, duas potenciações e por fim uma comparação que
diz que o valor guardado em `banana` não é igual (ou
seja, é `FALSO`) ao valor guardado em `laranja`.

Note que numa planilha, os valores das células são posicionais, ou seja,
a célula A1 (exemplo do primeiro parágrafo) guarda um certo valor e
a célula B1 guarda outro valor. No R, as variáveis não tem esse
conceito de "posição" (como as células da planilha). As variáveis
estão "na memória do 
computador", e ponto. Tudo o que estamos fazendo (cálculos, criando
e usando variáveis) não depende de nada "visual"; você não está vendo
os valores numa "tabela", você não pode pintar a variável `banana`
de amarelo e a `laranja` de alaranjado, como tenho certeza de que
você já deve ter feito com céluas de uma planilha!

Há um ponto sutil (ou não) aqui de diferença entre usar uma planilha
e o R. Na planilha é provável que, à medida que você vai fazendo
os cálculos, você já vai pensando em onde colocar as coisas no grid
da planilha. Já vai pintando as células, formatando o fonte das
letras, colocando bordas nas células, enquanto vai fazendo sua 
análise. No R não. Primeiro você faz a análise. Os resultados vão aparecendo
na console, são um tanto quanto "feinhos", não tem formatação. É isso
mesmo; o formato, o posicionamento do gráfico, a tabela bonita você 
só vai posicionar (num relatório) lá na
frente, no final. Não é que "tenha que ser assim". Mas vai por mim,
é mais fácil "perder tempo" com essas coisas de formatação
e cores mais pra frente, quando sua análise já estiver mais 
madura e os resultados "prontos".


## Tipos de dados

Até agora usamos valores numéricos armazenados em variáveis.
Também podemos guardar texto em variáveis. Por exemplo, podemos criar
uma variável chamada `nome` e uma outra chamada `idade` e aí fazemos
(atenção para as aspas):

````r
nome <- "Maria"
idade <- "20"
````

Então poderíamos gerar uma frase com o nome e a idade da pessoa assim:

````r
> nome <- "Maria"
> idade <- "20"
> cat(nome, "tem", idade, "anos.")
Maria tem 20 anos.
````

`cat()` é uma função do R que concatena, ou seja, cola um depois do outro,
vários argumentos (que são os valores dentro dos parênteses, separados
por vírgula).

Bem, e se tentarmos dividir o valor da variável `idade` pelo valor
da variável `laranja`? A matemática seria 20 dividido por 2 que resultaria
em 10. Vamos tentar?

````r
> idade / laranja
Error in idade/laranja : argumento não-numérico para operador binário
````

Caramba! O que aconteceu? Bem, tentamos dividir um número por um texto.
O valor de `laranja` é numérico, já o valor de `idade` é texto. Como saber?

Quando definimos o valor de `laranja` não colocamos aspas em torno do valor,
mas quando definimos o valor de `idade` colocamos aspas para definir o valor
de 20. As aspas dizem ao R que o que você está fornecendo são 
"caracteres de texto".

Lembra no Excel quando você quer digitar um número numa célula mas
não quer que o Excel trate aquilo como valor numérico, mas como
texto? No Excel começamos a digitação com um apóstrofe (ou aspas
simples) antes de digitar o número. Aqui a idéia é parecida (colocamos
o número entre aspas duplas).

Para vermos que tipo de informação uma variável guarda usamos a 
função `class()`
do R, assim:

````r
> class(laranja)
[1] "numeric"
> class(idade)
[1] "character"
````

Existem outros tipos de dados além de "numérico" e "caracter"; por exemplo,
temos o tipo "lógico". Esse tipo de dado tem apenas 2 valores: verdadeiro ou
falso (`TRUE` e `FALSE`, respectivamente em Inglês).

Quando fizemos a comparação entre os valores armazenados em `banana`i
 e `laranja`, obtivemos
a resposta `FALSE`. Se usarmos a função `class()` no resultado daquela
operação, veja o que acontece:

````r
> class(banana == laranja)
[1] "logical"
````

Ou seja, o resultado da comparação `banana == laranja` é `FALSE`, que
é um valor do tipo "lógico". `FALSE` não é texto, como se escrevêssemos
as letras F, A, L, S, E.

Já dá pra fazer muita coisa com esses três tipos de dados, então vamos
passar para um tipo de variável extremamente importante: data frame.

## Data frames

Data frame é um tipo de estrutura de dados que, para todos os efeitos
práticos, funciona como uma tabela. Sabe quando você carrega um arquivo
no Excel e olha para a planilha com as linhas e as colunas cheias de
valores? Então, no R, aquilo seria representado por um data frame.

Deixe-me ressaltar mais uma vez a diferença entre "olhar para a tabela"
(que é o que estamos acostumados a fazer na nossa interação com o Excel)
e ter uma tabela armazenada na memória do computador, sem necessariamente
tê-la numa representação visual (que é o jeito de trabalhar no R).

Vamos criar uma tabela hipotética com pessoas, suas idades, alturas e
pesos. Seria algo assim:

> Nome		Idade	Altura	Peso
> José		40		1.85	80
> Maria		38		1.70	65
> Pedro		20		1.60	80
> Flávia	12		1.50	50
> Darci		 2		0.70	20


