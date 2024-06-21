# TP2-IBD
Trabalho Prático 2 - Introdução a Banco de Dados

Universidade Federal de Minas Gerais

Nome: Leonardo Romano Andrade - 2023075151

## Descrição do trabalho

A ideia inicial era procurar relações entre: o número de queimadas no Brasil e o aumento de temperatura na região Sudeste, o aumento da temperatura em relação à radiação solar, e o número de queimadas em relação à umidade relativa do ar.

Para isso, foram utilizadas duas bases de dados:

- O número de queimadas por mês e por estado do Brasil de 1998 até 2017
- A temperatura da superfície medida por hora em diversas estações meteorológicas do Sudeste do Brasil
- A radiação média da última hora medida em várias estações meteorológicas do Sudeste do Brasil
- A umidade relativa do ar (fontes no final do arquivo)

Foi utilizado o PostgreSQL e a interface pgAdmin 4 para a análise.

A seguir, será explicado como foi realizado o trabalho, incluindo aquisição de dados, tratamento de exceções e limpeza, além das conclusões tiradas a partir de uma avaliação crítica dos resultados obtidos.
## 1) Preparação dos dados

Inicialmente, os dados coletados apresentavam diversas complicações, especialmente os dados de temperatura de superfície. A base encontrada tinha os seguintes problemas:

- Um grande volume de dados (arquivo de aproximadamente 2.5 GB para cada região do país)
- Muitos dados corrompidos (temperaturas de -9999.0 graus, por exemplo)
- Colunas e dados que não eram de interesse da análise ou eram redundantes (como mostrar a região SE quando a sigla do estado já era suficiente)
Índices (chaves) repetidos
   
(um exemplo de dados brutos corrompidos na imagem a seguir: 3 primeiras linhas corrompidas)![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/d237d2e4-0302-4d56-ad61-4e25bc8a83fa)

Assim, inicialmente, foi utilizado um script em Python para retirar as colunas que não eram de interesse da análise e eliminar linhas com dados absurdos. Depois disso, para tratar a repetição de chaves, foi utilizada 
uma chave maior, combinando (índice + data) como chave primária. Dessa forma, foi possível manter os princípios de unicidade da chave sem ignorar os dados com chaves repetidas.

## 2) Definição de objetivos 

O objetivo da análise é descobrir se existe uma relação direta entre o número de queimadas e outros fatores climáticos registrados na região Sudeste do Brasil ao longo dos anos. Para isso, iremos analisar:

- Se existe uma relação direta entre o número de queimadas e o aumento da temperatura
- Se um aumento de queimadas e uma diminuição da radiação solar acarretam em um aumento de temperatura
- Se um aumento do número de queimadas resulta em uma diminuição da umidade relativa do ar


## 3) Análise descritiva

Um resumo dos dados pode ser obtido aqui:![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/f84b1f1f-fa44-43a7-a5fd-e0ec66f6820c)

Após analisar a base de dados __queimadas__, temos as seguintes tabelas.

A primeira sobre a média de valores, máximos e mínimos:![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/438a937d-8bfe-4323-8f9e-87dea56f1d27)

A segunda tabela sobre a distribuição dos valores:![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/a11f5781-e0ca-435e-838e-86400dabdf73)

Após analisar a base de dados __weather_se__, temos as seguintes tabelas.

A primeira sobre a média de valores, máximos e mínimos:![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/ed95bda6-e70f-4cd1-87ce-61258615d0bf)

A segunda sobre a distribuição dos valores:

Radiação:

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/acda8f3b-72b1-4330-a67a-e747732f8b07)

Temperatura max:

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/4be0a32a-1437-456c-aa72-cde9b69d3c25)

Temperatura min:

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/83f24495-3972-48a5-9d56-0c50e9812bd2)

Umidade relativa do ar:

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/ca683047-2d57-4050-97d5-c9a96b7399eb)

## 4) Identificação de valores discrepantes

Como pode ser identificado nas tabelas do item 3, há diversos valores discrepantes nas medidas de radiação, temperatura máxima e temperatura mínima. Por exemplo, temos valores de radiação que ultrapassam os 50.000 kj/m², o que é 10 vezes mais que a média dos horários de pico de radiação solar (12:00 - 13:00). Esses valores são absurdos e indicam um equívoco na medição da radiação ou na coleta/armazenamento dos dados. Além disso, há registros de temperaturas de -51 graus Celsius, o que é claramente um erro, já que tal temperatura nunca foi registrada na história do país.

Dessa forma, temos o desafio de tratar esses dados, retirando outliers muito discrepantes e ignorando máximos e mínimos que estão muito além da realidade. Os dados absurdos foram removidos a partir da eliminação de valores que ficam além de 1.5 vezes o intervalo interquartil acima do terceiro quartil ou abaixo do primeiro quartil. A partir daqui, todas as análises e conclusões foram feitas sem contabilizar os outliers.

Além disso, é possível observar uma grande diferença entre o número de dados obtidos para cada ano. Isso ocorre devido ao problema citado no ponto 1 (preparação dos dados), onde foi identificada a corrupção de muitas linhas, tornando uma boa parte do conteúdo inutilizável.

## 5) Análise de correlação

Foi utilizado o índice de correlação de Pearson para identificar a correlação entre os dados. 
Esse índice apresenta resultados entre -1 e 1, sendo próximo de -1 uma relação inversamente proporcional, próximo de 1 sendo diretamente proporcional e próximo de 0 sendo variáveis não relacionadas.

correlação de Pearson entre número de queimadas e temperatura

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/eb0e8ce2-48a0-43a2-9779-d7fcfb07c020)

correlação de Pearson entre radiação global e temperatura (excluindo valores discrepantes)

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/7694570d-38e7-42d7-b27d-3f48ad542b8f)

correlação de Pearson entre número de queimadas e umidade relativa do ar

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/9fbd35ca-7674-4de2-bb13-fef040b71e14)

Concluímos, portanto, que, com os dados analisados, não foi possível identificar uma relação direta entre o número de queimadas e a temperatura. Porém, observamos que a radiação global tem uma influência relevante no aumento da temperatura e que o número de queimadas influencia a umidade do ar de forma inversamente proporcional. Nenhuma das relações é altamente significativa, ou seja, uma única variável não é capaz de explicar plenamente as variáveis relacionadas.

## 6) Conclusões

Analisando as hipóteses e objetivos iniciais junto com uma análise crítica dos dados e das pesquisas feitas com eles, podemos concluir que as queimadas no Brasil, pelo menos a curto prazo e com uma abordagem unidimensional, não têm uma influência direta na temperatura da região Sudeste do país. Observamos, também, que a radiação solar, conforme esperado, exerce uma influência muito mais significativa nos valores de temperatura.

No entanto, é possível afirmar que as queimadas desempenham um papel relevante na umidade relativa do ar do Sudeste, dado que a correlação entre esses dados é relativamente alta. Isso pode acarretar em mudanças climáticas substanciais que, no futuro, podem impactar as temperaturas e o clima do país.

Por fim, em relação aos dados utilizados, lamentavelmente a base de dados referente às medidas de temperatura, radiação e umidade apresentava muitos problemas, como dados corrompidos e medições inconsistentes ou extremamente discrepantes. Portanto, concluímos que seria mais prudente utilizar outros indicadores ou um conjunto de dados completamente diferente para alcançar conclusões mais precisas.

## 7) Links, referências e extras

Link dataset de queimadas no Brasil: https://www.kaggle.com/datasets/gustavomodelli/forest-fires-in-brazil?resource=download

Link dataset de dados meteorológicos por hora no Brasil: https://www.kaggle.com/datasets/PROPPG-PPG/hourly-weather-surface-brazil-southeast-region/data?select=southeast.csv











