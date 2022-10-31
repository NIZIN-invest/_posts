---
title: Como identificar o primeiro e último dia do mês
tags: [calendario, calculo, diversos]
categories: [ciencia de dados, diversos]
layout: article
share: true
toc: true
comments: true
feature:
 category: true
 index: true
ads: 
 show: true
tagcloud: true
image:
  feature: financas_investimentos/b3-header.png
  teaser: financas_investimentos/b3-logo.png
  credit: Diversas Origens
---

Abaixo apresento um algoritimo simples que identifica o primeiro dia útil de cada mẽs e o último dia útil de cada mẽs sem levar em conta feriados.

<!--more-->

O algoritimo é um exemplo de como obter tais informações e não visa exaurir as possibilidades de como conseguir tais valores.

{% highlight python linenos%}
import calendar
  
year = 1997
  
resmax = []
for month in range(1, 13):
  resmax.append(str(
    max(max(week[0:5] for week in calendar.monthcalendar(year, month)))) + "-" + str(month)+ "-" + str(year))

resmin = []
for month in range(1, 13):
    calmin = calendar.monthcalendar(year, month)
    for i, w in enumerate(calmin):
        for j, d in enumerate(w):
            if 0 == d:
            calmin[i][j] = 32
    resmin.append(str(
        min([week[0:5] for week in calmin][0])) + '-' + str(month) + '-' + str(year))

print(" Alista não leva em conta feriados!")
print("Lista de primeiros dias úteis: " + str(resmin))
print("Lista de últimos dias úteis: " + str(resmax))
{% endhighlight %}