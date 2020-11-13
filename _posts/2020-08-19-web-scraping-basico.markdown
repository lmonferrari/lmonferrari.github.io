---
layout: 'post'
title: 'Web Scraping Básico com Python'
categories: 'web scraping'
tags: [Web Scraping]
style: fill
color: info
description: Web scraping é o ato de 'raspar' a página da web para conseguir algumas informações disponíveis. 
---
Nesse primeito post vou mostrar como fazer Web Scraping básico porém poderoso.
O site que faremos o scraping será o <a href='https://www.worlddata.info'>https://www.worlddata.info</a>, que contém uma série de dados sobre os países. 
O objetivo desse post é apenas para estudo! Use por sua conta e risco!

Primeiro importamos as bibliotecas necessárias:

{% highlight python %}
import requests
from bs4 import BeautifulSoup

import pandas as pd

{% endhighlight%}

* O pacote requests é utilizado para fazer requisições aos servidores que hospedam a página. 
* Do pacote bs4 vamos utilizar o BeautifulSoup que vai nos ajudar no tratamento do html. 
* Com o pandas vamos criar um dataframe para manipular de maneira mais fácil os dados.

{% highlight python %}
url = 'https://www.worlddata.info/america/brazil/populationgrowth.php'
cabecalhos = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)'}
pagina = requests.get(url, headers = cabecalhos)
html = BeautifulSoup(pagina.text, 'html.parser')
{% endhighlight%}

* <b>url</b> é o endereço da página alvo
* <b>cabecalhos</b> é para o servidor interpretar que é um browser Chrome que está fazendo a requisição GET
* <b>pagina</b> é o retorno da nossa requisição 
* <b>html</b> é o nosso arquivo formatado pelo BeaultifulSoup

Aqui fazemos a extração da tabela que contém os dados que precisamos:

{% highlight python %}
tabela = html.find('table', {'class': 'std100 hover'})
{% endhighlight %}

* A variável <b>tabela</b> está recebendo a parte do html que contém a <b>table</b> com a classe <b>std100 hover</b>

Agora vamos extrair os cabeçalhos da tabela,<b><i> lembrando que esses cabeçalhos são diferentes dos que usamos anteriormente na requisição da pagina!</i></b>

{% highlight python %}
cabecalhos = tabela.find_all('th')
{% endhighlight %}

* Aqui a variável <b>cabecalhos</b> recebe todos os elementos <b>th</b> que existiam dentro da variável <b>tabela</b>

<i>Saída</i>: 
{% highlight python %}
[<th class="left">Jahr</th>,
<th>Population<br/>Brazil</th>,
<th>Change</th>,
<th>Birthrate</th>,
<th>Deathrate</th>,
<th>Population<br/>World</th>,
<th>Change</th>]
{% endhighlight %}


Fazemos a extração apenas do texto que existe em cada elemento <b>th</b>

{% highlight python %}
cabecalhos = [item.text for item in cabecalhos]
{% endhighlight %}

<i>Saída</i>:
{% highlight python %}
['Jahr',
'PopulationBrazil',
'Change',
'Birthrate',
'Deathrate',
'PopulationWorld',
'Change']
{% endhighlight %}

Aqui pesquisamos todos os <b>tr</b> que existem dentro da variável tabela

{% highlight python %}
linhas = tabela.find_all('tr', {'class':'nowrap right'})
{% endhighlight %}

Um trecho da saída:

{% highlight python %}
<tr class="nowrap right"><td class="left">1961</td><td>74.35 M</td><td>2.97 %</td><td> </td><td> </td><td class="blank"></td><td>3,075 M</td><td>1.35 %</td></tr>,
<tr class="nowrap right"><td class="left">1962</td><td>76.57 M</td><td>2.99 %</td><td> </td><td> </td><td class="blank"></td><td>3,128 M</td><td>1.72 %</td></tr>,
{% endhighlight %}


Próximo passo é remover o <b>M</b> e <b>%</b> que existe em alguns <b>td</b>


{% highlight python %}
import re
conteudo_por_linha = []
remover_simbolos = '% M‰'
pattern = "[" + remover_simbolos + "]"

for linha in linhas:
    dado_elemento = linha.find_all('td')
    dado_elemento = [re.sub(pattern, '', item.text) for item in dado_elemento]
    conteudo_por_linha.append(dado_elemento)
{% endhighlight %}

Montando o nosso Data Frame

{% highlight python %}
import pandas as pd

df = pd.DataFrame(conteudo_por_linha)
df.drop(5, axis = 1, inplace = True)
{% endhighlight %}

Mostrando os primeiros registros do Data Frame 

{% highlight python %}
df.columns = cabecalhos
df.head()

        Jahr	PopulationBrazil	Change	Birthrate	Deathrate	PopulationWorld	Change
0	1961	           74.35	2.97			                          3,075	1.35
1	1962	           76.57	2.99			                          3,128	1.72
2	1963	           78.85	2.98			                          3,193	2.07
3	1964	           81.17	2.94			                          3,258	2.05
4	1965	           83.50	2.87			                          3,325	2.05
{% endhighlight %}

Convertendo a variável <b>PopulationBrazil</b> em float

{% highlight python %}
df['PopulationBrazil'] = df['PopulationBrazil'].astype('float')
{% endhighlight %}


Plotando um gráfico
{% highlight python %}
import matplotlib.pyplot as plt
import seaborn as sns
plt.figure(figsize=(20,10))
plt.xticks(rotation = 55)
sns.lineplot(data = df, x = 'Jahr', y = 'PopulationBrazil')
plt.xlabel('Ano')
plt.ylabel('População Brasileira')
plt.show()
{% endhighlight %}

![Gráfico](../images/webscraping/graf1.PNG)
