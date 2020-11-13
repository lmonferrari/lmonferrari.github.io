---
layout: post
title: Introdução a Probabilidade - Análise Combinatória
categories: probability
style: border
color: primary
tags: [Statistics]
description: Uma pequena introdução a Análise Combinatória com exemplos em Python
---
## Permutação
##### O que é permutação?
Uma permutação é o agrupamento ordenado de n elementos distintos.

##### Quando utilizamos?
Utilizamos quando queremos saber de quantas formas distintas conseguimos organizar um número de elementos. 

##### Exemplo:
Qual o número de anagramas para a palavra 'Python'?
```
6! = 6 * 5 * 4 * 3 * 2 * 1 = 720
```
Uma implementação em Python da Permutação:
```python
from functools import reduce

n = 6
fatorial = reduce((lambda x, y: x * y), range(1,n + 1))
print(fatorial)
```

## Arranjo
##### O que é arranjo?
Um arranjo de n elementos dispostos p a p, com p menor ou igual a n, é um escolha de p entre esses n elementos na qual a ordem importa.

##### Quando utilizamos?
Devemos utilizar arranjo quando a ordem importa. 

##### Exemplo:
Temos 20 pilotos de Fórmula 1 e queremos saber quantos podios de primeiro, segundo e terceiro lugares podemos formar. Temos que levar em conta que um pódio formado por Vettel, Hamilton e Raikkonen é diferente do pódio formado por Hamilton, Raikkonen e Vettel. 

Uma implementação em Python de arranjo:
```python
from functools import reduce 

n = 20
p = 3
fatorial_n = reduce((lambda x, y: x * y), range(1,n + 1))
fatorial_n_p = reduce((lambda x, y: x * y), range(1,n + 1 - p))
print(fatorial_n / fatorial_n_p)
```

## Combinação
##### O que é combinação?
São escolhas não ordenadas de n elementos tomados p a p desses elementos.

##### Quando utilizamos?
Quando o importante é que elementos sejam diferentes. 

##### Exemplo:
Como formar uma equipe do Master Chef de 4 pessoas dentre 10 pessoas? Aqui a ordem não importa, logo uma equipe formada por Haila, Helton e Vitor é a mesma coisa que uma equipe formada por Helton, Vitor e Haila. 

Uma implementação em Python de Combinação:
```python
n = 10
p = 4
fatorial_n = reduce((lambda x, y: x * y), range(1,n + 1))
fatorial_p = reduce((lambda x, y: x * y), range(1,p + 1))
fatorial_n_p = reduce((lambda x, y: x * y), range(1,n + 1 - p))

print(fatorial_n / (fatorial_p * fatorial_n_p))
```