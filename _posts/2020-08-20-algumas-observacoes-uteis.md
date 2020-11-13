---
layout: 'post'
title: 'Algumas observações úteis'
style: fill
color: success
tags: [Data Science]
description: Algumas dicas e informações sobre como fazer Análise dos dados em R
---

O trabalho do cientista de dados envolve muitas técnicas e estudos. Aqui vou registrar alguns pontos que acho importantes assim como algumas ferramentas. Vou utilizar esse post somente como um guia rápido.

* Analisar o problema de negócio.
* Verificar os dados para ver quais são dados quantitativos / qualitativos.
* Utilizar modelo de Regressão ou Classificação? O que queremos prever? 
    * alguns pacotes para nos ajudar com Classificação:
        * <b>caret</b> contém a função <b>confusionMatrix</b>, que é muito util para verificar a acurácia do modelo;
        * pacote <b>class</b> contém o algoritmo <b>knn</b>;
        * pacote <b>C50</b> contem <b>C5.0</b> que é o modelo de árvore de decisão;
        * pacote <b>ROSE</b> contem <b>roc.curve</b> que plota a Caracteristica de operação do Receptor(roc)
    * alguns pacotes para nos ajudar com Regressão linear:
        * <b>caret</b> oferece a função <b>train</b>, com o parametro <b>'method'</b> podemos especificar 'lm' linear model, 'glm' regressão logistica, 'rf' random forest. 
* Importante normalizar os dados.

* Separar dados de treino e teste.
    * para testar nosso modelo devemos separar os dados:
        *  pacote <b>caTools</b>, com a função <b>sample.split</b> pode nos ajudar;

* Observar se os dados estão balanceados, em classificação é importante os dados estarem balanceados para o algorimo não gerar viés.
    * Caso os dados estejam desbalanceados podemos utilizar a técnica Synthetic Minority Over-sampling Technique:
        * o pacote <b>DMwR</b> tem a função <b>SMOTE</b> que nos ajuda a balancear os dados. 

* Muitas vezes precisamos verificar a frequencia de uma variável para entender melhor como está a distribuição
    * Para verificar a frequencia de uma variável:
        * podemos utilizar <b>prop.table</b>. 
* Avaliar o modelo final

* Podemos fazer teste de hipoteses
    * Existem alguns testes a serem aplicados:
        * <b>t.test</b>, compara a medida média de um grupo com a média de uma população, suponha que a média de altura da população seja 1,60 e a média da amostra seja 1,65. O teste avalia se a média da amostra é estatisticamente significante para rejeitar H0, neste caso nossa H0 é que a média de altura é igual 1,60. Caso o valor p < 0.05 não existe significancia para rejeitar a H0.

        * <b>shapiro.test</b>, a H0 deste teste é que a população é normal, ou seja, um valor P < 0.05 indica que a H0 foi rejeitada e nossa população não possui uma distribuição normal.