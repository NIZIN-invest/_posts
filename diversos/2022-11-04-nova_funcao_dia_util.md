---
title: Nova Função dia_util
tags: [funcoes, utilitários, datas, dias úteis]
categories: [ciencia de dados, utilitarios]
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
math: true
---

Aos poucos o framework para processar informações relativas a investimentos na Bolsa de Valores vai tomando forma, e surgindo funções utilitárias que serão amadurecidas conforme forem demandas e aplicadas, neste artigo apresento a função `dia_util` que será trabalhada para retornar os dias úteis de um faixa de datas.

<!--more-->

A função `dia_util` foi criada para auxiliar na obtenção de dias úteis, na versão aqui apresentada ela não identifica automáticamente os feriados, mas é possível informar uma lista obtida em outra função que será criada futuramente uma lista com as datas que são feriados.

A Função `dia_util` recebe como parâmetro obrigatório o ano que se quer obter os dias úteis, assim ela irá retornar todos os dias úteis do ano informado através do parâmetro `ano=` do típo inteiro, é possível informar o mês ou meses que se deseja obter os dias úteis, para informar apenas um mês use o parâmetro `mes=`, também do tipo inteiro, para informar uma lista de meses, faça através do parâmetro `meses=`, sendo uma lista de típos inteiros, veja que `mes=` tem maior precedência, como pode ser visto no algoritmo aqui apresentado. Além destes parâmetros também é possível informar um limite final de dias úteis através do parâmetro `limite=` que é do típo string no formato `dd/mm/aaaa`. E alista de feriados pode ser informada também no formato lista de string com a data formatada em `dd/mm/aaaa` através do parametro `feriados=`.

A função `dia_util` irá retornar uma lista de strings com as datas no formato `dd/mm/aaaa`

Abaixo segue o código completo da função na data de publicação deste artigo:

{% highlight python linenos %}
import calendar
from datetime import datetime
def dia_util(ano, mes = None, meses=range(1,13), feriados=None, limite=None):
    resutil = []
    if mes:
        meses = range(mes,mes+1)
    for month in meses:
        calutil = calendar.monthcalendar(ano, month)
        weekutil = [week[0:5] for week in calutil]
        for week in weekutil:
            for day in week:
                if day == 0:
                    continue
                daystr = "{:02d}/{:02d}/{:4d}".format(day, month, ano)
                if limite != None and datetime.strptime(limite,'%d/%m/%Y') <= datetime.strptime(daystr,'%d/%m/%Y'):
                    break
                if feriados != None and daystr in feriados:
                    continue
                resutil.append(daystr)
            else:
                continue
            break
        else:
            continue
        break
    return resutil
{% endhighlight %}
   
## Conclusão

Com certeza esta função pode ser melhorada, fica ai o desafio para trazer melhorias e apresenta-las nos comentários. Conforme surgirem melhorias atualizo este artigo ou farei um novo.