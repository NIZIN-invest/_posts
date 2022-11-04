---
title: Como identificar dias úteis
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

Abaixo apresento um algoritimo simples que identifica o primeiro dia útil de cada mẽs, o último dia útil de cada mẽs e finalmente todos os dias úteis, sem levar em conta feriados.

<!--more-->

O algoritimo é um exemplo e não visa exaurir as possibilidades.

## Obtendo o último dia útil do mês

{% highlight python linenos %}
import calendar
  
year = 1997
  
resmax = []
for month in range(1, 13):
  resmax.append(
    "{:02d}/{:02d}/{:4d}".format(
    max(max(week[0:5] for week in calendar.monthcalendar(year, month))), month, year))

print(" A lista não leva em conta feriados!")
print("Lista de últimos dias úteis: " + str(resmax))
{% endhighlight %}

## Obtendo o primeiro dia útil do mês

{% highlight python linenos %}
import calendar
  
year = 1997
  
resmin = []
for month in range(1, 13):
  calmin = calendar.monthcalendar(year, month)
  for i, w in enumerate(calmin):
    for j, d in enumerate(w):
      if 0 == d:
        calmin[i][j] = 32
  day = min([week[0:5] for week in calmin][0])
  if day == 32:
     day = min([week[0:5] for week in calmin][1])
  resmin.append(
    "{:02d}/{:02d}/{:4d}".format(day, month, year))

print(" A lista não leva em conta feriados!")
print("Lista de primeiros dias úteis: " + str(resmin))
{% endhighlight %}

## Obtendo os dias úteis do mês

E finalmente apresento um algoritimo que seleciona os dias úteis de todos os mesês:

{% highlight python linenos %}
import calendar
  
year = 1997
  
resutil = []
for month in range(1, 13):
  calutil = calendar.monthcalendar(year, month)
  weekutil = [week[0:5] for week in calutil]
  for week in weekutil:
    for day in week:
      if day != 0:
        resutil.append(
        "{:02d}/{:02d}/{:4d}".format(day, month, year))

print(" A lista não leva em conta feriados!")
print("Lista de dos dias úteis: " + str(resutil))
{% endhighlight %}

## Conclusão

Este algoritmo é um exemplo de como se obter o todos os dias úteis, o primeiro e último do mês em python, e pode ser melhorado conforme as aplicações desejadas, deixe no comentários sua sugestão de melhora, ou outra abordagem que julgue interessante.

Caso o algoritmo tenha sito útil em seu projeto compartilho com a gente como ele foi usado.
