# TP2-IBD
Trabalho Prático 2 - Introdução a Banco de Dados

Universidade Federal de Minas Gerais

Nome: Leonardo Romano Andrade - 2023075151

## Descrição do trabalho

A ideia inicial era de procurar relações entre o número de queimadas no Brasil e o aumento de temperatura na região Sudeste do Brasil.
Para isso, foram utilizadas duas bases de dados:
 - O número de queimadas por mês e por estado do Brasil de 1998 até 2017
 - A temperatura da superfície medida por hora de diversas estações meteorológicas do Brasil
   (fontes no final do arquivo)

Foi utilizado o PostgreSQL e a interface pgAdmin 4 para a análise do trabalho.

A seguir, será explicado como foi feito o trabalho, aquisição de dados, tratamento de exceções e limpeza, além das conclusões tiradas a partir de uma avaliação crítica dos resultados obtidos.

## 1) Preparação dos dados

Inicialmente, os dados coletados possuíam diversas complicações, em especial os dados de temperatura de superfície.
A base encontrada, tinha os seguintes problemas:
 - Um grande volume de dados (arquivo de aproximadamente 2.5 GB para cada região do país)
 - Muitos dados corrompidos (temperaturas de -9999.0 graus, por exemplo)
 - Colunas e dados que não eram de interesse da análise ou redundantes (exemplo, mostrando a região SE quando a sigla do estado já era suficiente)
 - Índices (chaves) repetidas
   
(um exemplo de dados brutos corrompidos na imagem a seguir: 3 primeiras linhas corrompidas)![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/d237d2e4-0302-4d56-ad61-4e25bc8a83fa)

Assim, inicialmente, foi utilizado um script em python para retirar as colunas que não eram de interesse da análise e retirar linhas com dados absurdos.
Depois disso, para tratar a repetição de chaves, foi utilizada uma chave maior, usando *(índice + data)* como chave primária. Assim, foi possível manter os
princípios de unicidade da chave sem ignorar os dados com chaves repetidas.

## 2) Definição de objetivos 

O objetivo da análise é de descobrir se existe uma relação direta entre os números de queimadas e outros fatores do clima registrados na região Sudeste do Brasil ao longo dos anos.
Para isso, iremos analisar: 
 - Se existe uma relação direta entre o número de queimadas e aumento da temperatura
 - Se um aumento de queimadas e uma diminuição da radiação solar acarreta em um aumento de temperatura
 - Se um aumento do número de queimadas acarreta em uma diminuição da umidade relativa do ar


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

Como pode ser identificado na tabela do item 3, existem muitos valores que 

## 5) Análise de correlação

Foi utilizado o índice de correlação de Pearson para identificar a correlação entre os dados. 
Esse índice apresenta resultados entre -1 e 1, sendo próximo de -1 uma relação inversamente proporcional, próximo de 1 sendo diretamente proporcional e próximo de 0 sendo variáveis não relacionadas.

correlação de Pearson entre número de queimadas e temperatura

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/eb0e8ce2-48a0-43a2-9779-d7fcfb07c020)

correlação de Pearson entre radiação global e temperatura (excluindo valores discrepantes)

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/7694570d-38e7-42d7-b27d-3f48ad542b8f)

correlação de Pearson entre número de queimadas e umidade relativa do ar

![image](https://github.com/LeoRoms/TP2-IBD/assets/145928486/9fbd35ca-7674-4de2-bb13-fef040b71e14)

Concluímos, portanto, que, com os dados analisados, não foi possível identificar uma relação direta entre o número de queimadas e a temperatura. Porém, vemos que
a radiação global tem uma influência relevante no aumento da temperatura e o número de queimadas influencia a umidade do ar de forma inversamente proporcional.
Nenhuma das relações é de cunho altamente relacionado, ou seja, uma variável apenas não é capaz de explicar ambas variáveis relacionadas.

## 6) Conclusões

## 7) Links, referências e extras











