# Ordenar Cromossomos

O exercício anterior era composto de baixar os cromossomos do site da UCSC (link: https://hgdownload.soe.ucsc.edu/goldenPath/hg38/chromosomes/)

Para isso, geramos uma sequência de números de 1 a 22 utilizando o comando seq e depois combinamos com o comando awk para gerar um arquivo com as linhas de comando de download usando o wget dos cromossomos do hg38 na USCS.
O comando utilizado foi:

    seq 1 22 | awk '{print("wget -c https://hgdownload.soe.ucsc.edu/goldenPath/hg38/chromosomes/chr"$1".fa.gz")}' > pegar-fa.sh

Depois disso, bastou executar o arquivo pegar-fa.sh

    sh pegar-fa.sh

O próxima passo é gerar um arquivo com todos os cromossomos. E aí que está o desafio!

Para juntar todos os cromossomos, podemos utilizar o comando **cat** (ou **zcat** para arquivos compactados). Porém, os arquivos ficam listados em ordem alfabética, como é possível observar na figura abaixo:

![Captura de tela de 2023-09-05 10-56-29](https://github.com/angelicanakagawa/ordenar-cromossomo/assets/60954085/c342a762-a1a8-489f-81a9-210d53898944)

Então, simplesmente, não podemos utilizar o caracter coringa * no comando zcat, ou seja:

    zcat chr*.fa.gz > hg38.fa

Pois, neste caso, o cromossomo 10 viria antes do cromossomo 1.

A solução mais simples é digitar todos os cromossomos:

    zcat chr1.fa.gz chr2.fa.gz chr3.fa.gz chr4.fa.gz chr5.fa.gz chr6.fa.gz chr7.fa.gz chr8.fa.gz chr9.fa.gz chr10.fa.gz chr11.fa.gz chr12.fa.gz chr13.fa.gz chr14.fa.gz chr15.fa.gz chr16.fa.gz chr17.fa.gz chr18.fa.gz chr19.fa.gz chr20.fa.gz chr21.fa.gz chr22.fa.gz > hg38.fa

Mas podemos otimizar o comando acima, para não digitar o nome de todos os arquivos de cromossomo. Uma solução é utilizar o comando abaixo:

    zcat $(ls chr*.fa.gz|sort -V) > hg38.fa

onde nós substituimos os nomes dos arquivos de cromossomos pela saída do comando ls chr*.fa.gz|sort -V, desse modo o resultado será os nomes dos arquivos de cromossomos ordenados de 1 a 22.
