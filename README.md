# TP2-IBD
Trabalho Prático 2 - IBD - Correlação queimadas no Brasil e temperatura média na região Sudeste

Nome: Leonardo Romano Andrade - 2023075151

Universidade Federal de Minas Gerais

## Descrição do trabalho

A ideia inicial era de procurar relações entre o número de queimadas no Brasil e o aumento de temperatura na região Sudeste do Brasil.
Para isso, foram utilizadas duas bases de dados:
 - O número de queimadas por mês e por estado do Brasil de 1998 até 2017
 - A temperatura da superfície medida por hora de diversas estações meteorológicas do Brasil
   (fontes no final do arquivo)

A seguir, será explicado como foi feito o trabalho, aquisição de dados, tratamento de exceções e limpeza, além das conclusões tiradas a partir de uma análise crítica dos dados.

## Preparação dos dados

Inicialmente, os dados coletados possuíam diversas complicações, em especial os dados de temperatura de superfície.
A base encontrada, tinha os seguintes problemas:
 - Um grande volume de dados (arquivo de aproximadamente 2.5 GB para cada região do país
 - Muitos dados corrompidos (temperaturas de -9999.0 graus)
 - Colunas e dados que não eram de interesse da análise ou redundantes (exemplo, mostrando a região SE quando a sigla do estado já era suficiente)
 - Índices (chaves) repetidas
(um exemplo de dados brutos absurdo na imagem a seguir)![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/0124aaae-7a1e-4f85-a558-38c01184b9ef)

