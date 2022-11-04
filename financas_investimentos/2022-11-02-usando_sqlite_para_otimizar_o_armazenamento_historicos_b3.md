---
title: Usando SQLite para otimizar o armazenamento de históricos da B3
tags: [b3, investimento, ciencia de dados, acoes, ativos, aplicação, financeiro, dados historicos, gspread, pandas, nunpy, mathplotLib, openpyxl, requests, pyfolio, pytz, zipfile, analise fundamentalista, analise, tutorial, gspread, pandas, nunpy, mathplotLib, openpyxl, requests, pyfolio, pytz, zipfile, b3, sqlite, banco de dados]
categories: [ciencia de dados, investimentos]
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

Visto que os históricos anuais da B3 são muito grandes para serem manipulados diretamente em memória, principalmente quando precisamos lidar com mais de um arquivo, apresentarei uma solução para uso do SQLite como meio de armazenamento e obtenão estruturada dos registros. O pandas oferece integração com SQLite, sendo assim, apresentaremos o uso de ambos.

<!--more-->

## Quais módulos do Python usaremos 

Usaremos mais dois módulos em python, o primeiro SQLite3 e o segundo precisará ser instalado com o comando `pip sqlite_utils` este auxilia o uso de `dictionary` de forma transparente com o SQLite3.

## Processando os arquivos

Veremos aqui apenas como processar o arquivo, para aprender como obte-lo diretamente no site da B3 veja o tutorial ["Obtendo os dados históricos da b3"]({% post_url /financas_investimentos/2022-10-28-dados_historicos_b3 %}), é fundamental que execute os algoritmos fornecidos no artigo citado.

## Conectando ao banco de dados SQLite

Conectar ao banco de dados SQLlite usando `sqlite_utils` é muito simples vejo o código abaixo:

{% highlight python linenos %}
import os
from pathlib import Path
from sqlite_utils import Database, suggest_column_types

os.makedirs("./cotacoes/database", exist_ok=True)

db = Database("cotacoes/database/historico_b3.db")
{% endhighlight %}

Veja que criamos um novo subdiretório para armazenar nosso banco de dados. além disso, como pode ver o módulo `sqlite_utils" tem uma classe utilitária, "Database" que cria o banco de dados.

## Processando e inserido os dados 

Agora se vocẽ ainda não leu e pois em prática o código do artigo ["Obtendo os Dados Históricos da B3]({% post_url /financas_investimentos/2022-10-28-dados_historicos_b3 %}), faça primeiro pois vamos precisar das funções lá criadas para obter os arquivos, a função de processamento será ajustada e renomeada para `processa_cotacoes_db`, as demais funções continuam com o mesmo algoritmo.

{% highlight python linenos%}
from zipfile import ZipFile
import pandas as pd

def processa_cotacoes_db(ano, mes=None, dia=None, overwrite=True):
    
    if dia and mes:
        file_name = "COTAHIST_D{{:0>4}}{{:0>2}}{{:0>2}}".format(ano,mes,dia)
    elif mes:
        file_name = "COTAHIST_M{{:0>4}}{{:0>2}}".format(ano,mes)
    else:
        file_name = "COTAHIST_A{{:0>4}}".format(ano)
    #
    zip_file_name = "cotacoes/" + file_name + ".ZIP"
    print('#', end='')
    #
    os.makedirs("./cotacoes/database", exist_ok=True)
    db = Database("cotacoes/database/historico_b3.db")
    #
    with ZipFile(zip_file_name, 'r') as zip:
        arq_cotacoes = file_name + ".TXT"
        print('#', end='')
        with zip.open(arq_cotacoes) as cotacoes:
            print('#', end='')
            #
            count = 0
            for linha in cotacoes:
                count = count + 1
                dic = processa_linha_cotacoes(linha.decode('utf8'),count)
                if dic:
                    db['cotacoes'].insert(dic)
                #                
                print('.', end='')
                if not count % 40:
                    print(" " + str(count)) 
        zip.close()
    #
    print('#')
    #    
    return cotacoes_df
{% endhighlight %}

Como pode ver pouco código foi alterado, apenas não criamos mais o Dataframe e inserimos diretamente no banco de dados os novos registros.

## Conclusão

O código acima é didático e deve ser otimizado para uso na prática com históricos anuais. Colocamos no [GIST (clique aqui)](https://gist.github.com/carlosdelfino/f17caa6c66cd2b96c5715edeb6b624a3) este mesmo código um pouco mais otimizado, vejao que pode melhorar com o conhecimento que você já tem.

## Referência

* [Gerenciando banco de dados SQLite3 com Python](http://pythonclub.com.br/gerenciando-banco-dados-sqlite3-python-parte1.html#inserindo-um-registro-com-parametros-de-entrada-definido-pelo-usuario)
* [SQLite Utils](https://sqlite-utils.datasette.io/en/3.14/installation.html)