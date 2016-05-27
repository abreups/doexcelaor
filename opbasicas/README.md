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


tipos de variáveis


data frames


