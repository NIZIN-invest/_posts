---
title: Obtendo os dados históricos da B3
tags: [b3, investimento, ciencia de dados, acoes, ativos, aplicação, financeiro, dados historicos, gspread, pandas, nunpy, mathplotLib, openpyxl, requests, pyfolio, pytz, zipfile, analise fundamentalista, analise, tutorial]
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
---
Nesse próximo passo desta serie de tutoriais é a obtenção de dados históricos de negociação de ações na B3, Os arquivos são disponbilizados compactados e em formato posicional sendo bem simples a extração dos dados.

## Onde obter dados de cotação diária

Para obter dados de cotação seja diária, mensal e anual visite o link
[https://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/historico/mercado-a-vista/cotacoes-historicas/](https://www.b3.com.br/pt_br/market-data-e-indices/servicos-de-dados/market-data/historico/mercado-a-vista/cotacoes-historicas/).

Abaixo apresento uma forma simples de obtenção automática dos dados, procure fazer todo o tutorial, depois busque fazer melhorias no código e compartilhe nos comentários suas sugestões de melhoras. As acotações serão gravadas na subpasta `cotacoes`.

## Obtendo o arquivo de cotações

Como este tutorial é base para tutoriais mais avançados, irei criar uma função que em outros tutoriais será expandida para atender nossa necessidades de cada fase desta série.

Para lermos o conteúdo do arquivo Zipado iremos usar o módulo ZipFile.

Iremos trabalhar apenas com os arquivos anuais, futuramente em outros tutoriais adicionaremos recursos para baixar o mensal e anual, conforme a demanda.

{% highlight python linenos%}
import os
from pathlib import Path
import requests as req
def get_cotacoes(ano, mes=None, dia=None, overwrite=True):
    
    if dia and mes:
        zip_file_name = "COTAHIST_D{:02d}{:02d}{}.ZIP".format(dia,mes,ano);
    elif mes:
        zip_file_name = "COTAHIST_M{:02d}{}.ZIP".format(mes,dia);
    else:
        zip_file_name = "COTAHIST_A{}.ZIP".format(ano);
    #
    dest_path_file = Path("cotacoes/" + zip_file_name)
    if dest_path_file.is_file() and not overwrite:
        print("Arquivo {} já existe, não será baixado!".format(zip_file_name))
        return
    #
    print("Obtendo histórico {}".format(zip_file_name))
    url = "https://bvmf.bmfbovespa.com.br/InstDados/SerHist/"+zip_file_name
    headers = { 'accept': '*/*',
      'accept-language': 'en-US,en;q=0.9,pt-BR;q=0.8,pt;q=0.7,es-MX;q=0.6,es;q=0.5',
      'content-type': 'application/x-www-form-urlencoded; charset=UTF-8',
      'x-requested-with': 'XMLHttpRequest',
      'sec-ch-ua': '" Not;A Brand";v="99", "Google Chrome";v="97", "Chromium";v="97"',
      'sec-ch-ua-mobile': '?0',
      'sec-ch-ua-platform': '"macOS"',
      'sec-fetch-dest': 'empty',
      'sec-fetch-mode': 'cors',
      'sec-fetch-site': 'same-origin',
      'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko)'}
    get_response = req.get(url,stream=True, headers = headers)
    print('#', end='')
    #
    file_name  = "./cotacoes/" + zip_file_name
    os.makedirs("./cotacoes", exist_ok=True)
    #
    with open(file_name, 'wb') as f:
        print('#', end='')
        
        for chunk in get_response.iter_content(chunk_size=1024):
            if chunk: # filter out keep-alive new chunks
                print('.', end='')
                f.write(chunk)
        
        f.close()
        print('#')
{% endhighlight %}


## Ajustes nas datas

Precisamos lembrar de setar o fusohorário que vamos trabalhar já que iremos lidar com datas, neste caso é '-3', iremos usar o módulo PyTZ para nos ajudar, instale com o comando 'pip install pytz',  então importe do módulo "datetime' o objeto 'datetime' e do módulo "pytz" o objeto 'timezone', e finalmente informe seu time zone, como estou no Ceará, usarei o TimeZone America/Fortaleza.

{% highlight python linenos%}
from datetime import datetime
from pytz import timezone

tz = timezone('America/Fortaleza')
data_e_hora_atuais = datetime.now()
data_e_hora_atuais_tz = data_e_hora_atuais.astimezone(tz)
{% endhighlight %}

## Lendo o arquivo e convertendo para um DataFrame

Iremos agora ler o arquivo zipado que foi baixado e converte-lo em um DataFrame (uma tabela de dados do Pandas, se não está familiarizado, veja os outros dois tutoriais que criamos desta série).

Iremos usar duas funções, a primeira lê o arquivo, a segunda processa cada linha lida.

### Como é composto e estruturado o arquivo de cotações da B3

O Arquivo de cotrações está compactádo no formato ZIP, o arquivo de dados possui o mesmo nome do arquivo compactado, porém com a extensão ".TXT", não será necessário descompactar o arquivo, iremos trabalhar com ele diretamente dentro do arquivo ZIP.

O formato do arquivo é descrito pelo documento [SeriesHistorcas_Layout.pdf que pode ser obtido clicando aqui](https://www.b3.com.br/data/files/33/67/B9/50/D84057102C784E47AC094EA8/SeriesHistoricas_Layout.pdf)

O Arquivo em sí é composto por 3 tipos de linha: header, registro, trailer, a primeira é um simple cabeçalho que confirma a data referente as cotações e sua origem "BOVESPA", o `trailer` é o último registro, fornece a data que o arquivo foi gerado e o tamanho em número de registros, e as demais linhas são os registros de cada cotação com diversos dados como listado a seguir:

TIPREG
: Típo de registro, tem o valor fixo em 01

Data da transação
: formato AAAAMMDD

CODBDI
: Código BDI, formato XX, Utilizado para classificar os papeis no Boletim Diário de Informação, 

CODNEG
: Código de negociação do papel, formato X(12)

TPMERC
: Tipo de mercado, formato NNN, 

NONRES
: Nome resumido da empresa emissora do papel, formato X(12)

ESPECI
: Especificação do papel, formato X(10)

PRAZOT
: Prazo em dias do mercado a termo, formato X(03)

MODREF
:  Moeda de Referência, tipicamente "R$", formato X(4)

PREABE
: Preço de abertura do papelmercado no pregão, formato (11)V99

PREMAX
: Preço máximo do papelmercado no pregão, formato (11)V99

PREMIN
: Preço mínimo do papelmercado no pregão, formato (11)V99

PREMED
: Preço médio do papelmercado no pregão, formato (11)V99

PREULT
: Preço do último negócio do papelmercado no pregão, formato (11)V99

PREOFC
: Preço da melhor oferta de compra do papelmercado, formato (11)V99

PREOFV
: Preço da melhor oferta de venda do papelmercado, formato (11)V99

TOTNEG
: Número de negócios efetuados com o papelmercado no pregão, formato (11)V99

QUATOT
: Quantidade total de títulos negociados neste papelmercado, formato (11)V99

VOLTOT
: Volume total de títulos negociados neste papelmercado, formato (11)V99

PREEXE
: Preço de exercício para o mercado de opções ou valor do contrato para o mercado de termo secundário, formato (11)V99

INDOPC
: Indicador de correção de preços de exercícios ou valores de contrato para os mercados opções ou termo secundário, formato N(01)

DATVEN
: Data do vencimento para os mercados de opções ou termo secundário, formato "AAAAMMDD"

FATCOT
: Fator de cotação do papel, 1 para cotação unitária, 1000 para lotes de mil ações, formato N(07)

PTOEXE
: Preço de exercício em pontos para opções referenciadas em dólar ou valor de contrato em pontos para termo secundário, para os referenciados em Dólar, cada ponto equivale ao valor, na moeda corrente de um centêsimo da taxa média do Dólar comercial 

CODISI
: Código do papel no sistema isin ou código inerno do papel, formato X(12)

DISMES
: Número de distribuição do papel, número de sequência do papel correspondente ao estado de direito vigente, formato N(03)

### Lendo o Arquivo

Para ler o arquivo precisamos importar o módulo ZipFile. E então faremos o processamento do arquivo de dados diretamente dentro do arquivo compactado, que é um formato posicional como explicado anteriormente.

{% highlight python linenos%}
from zipfile import ZipFile
import pandas as pd

def processa_cotacoes(ano, mes=None, dia=None, overwrite=True):
    if dia and mes:
        zip_file_name = "COTAHIST_D{}{}{}.ZIP".format(ano,mes,dia);
    elif mes:
        zip_file_name = "COTAHIST_M{}{}.ZIP".format(mes,dia);
    else:
        zip_file_name = "COTAHIST_A{}.ZIP".format(ano);
    zip_file_name = "cotacoes/" + zip_file_name
    print('#', end='')
    #
    cotacoes_df = pd.DataFrame()
    #
    with ZipFile(arq_zip_cotacoes, 'r') as zip:
        arq_cotacoes = "COTAHIST_A{}.TXT".format(ano)
        print('#', end='')
        with zip.open(arq_cotacoes) as cotacoes:
            print('#', end='')
            #
            cotacoes_df = pd.DataFrame()
            count = 0
            for linha in cotacoes:
                count = count + 1
                dic = processa_linha_cotacoes(linha.decode('utf8'),count)
                if dic:
                    cotacoes_df = cotacoes_df.append(dic, ignore_index=True)
                #                
                print('.', end='')
                if not count % 40:
                    print(" " + str(count)) 
        zip.close()
    #
    cotacoes_df['CODBDI'].astype("category")
    cotacoes_df['TPMERC'].astype("category")
    cotacoes_df['ESPECI'].astype('category')
    cotacoes_df['INDOPC'].astype('category')
    #
    print('#')
    #    
    return cotacoes_df
{% endhighlight %}

Bem agora precisamos criar a função `processa_linha_cotacoes(linha).

### Processando linha por linha

Agora vamos criar a função que processa a linha e obtém os dados necessários para construirmos nosso dataframe de cotações.

Essa função é bem extensa em número de linhas de comando, pois precisamos processar cada campo do registro informada, o código é auto explicativo quanto a cada posição dos registros obtidos e já foram descritos acima.

{% highlight python linenos%}
def processa_linha_cotacoes(reg,count):
    dic = {}
    if reg[0:2] == '01':  # Registro de dados
        dic['DATAPREG'] = datetime.strptime(reg[2:10], '%Y%m%d').astimezone(tz)
        dic['CODBDI'] = reg[10:12].strip()
        dic['CODNEG'] = reg[12:24].strip()
        dic['TPMERC'] = int(reg[24:27])
        dic['NOMRES'] = reg[27:39].strip()
        dic['ESPECI'] = reg[39:49].strip()
        dic['PRAZOT'] = int(reg[49:52]) if reg[49:52].strip() else 0
        dic['MODREF'] = reg[52:56].strip()
        dic['PREABE'] = float(reg[56:69])/100
        dic['PREMAX'] = float(reg[69:82])/100
        dic['PREMIN'] = float(reg[82:95])/100
        dic['PREMED'] = float(reg[95:108])/100
        dic['PREULT'] = float(reg[108:121])/100
        dic['PREOFC'] = float(reg[121:134])/100
        dic['PREOFV'] = float(reg[134:147])/100
        dic['TOTNEG'] = int(reg[147:152])
        dic['QUATOT'] = int(reg[152:170])
        dic['VALTOT'] = float(reg[170:188])/100
        dic['PREEXE'] = float(reg[188:201])/100
        dic['INDOPC'] = int(reg[201:202])
        dic['DATVEN'] = datetime.strptime(reg[202:210], '%Y%m%d').astimezone(tz)
        dic['FATCOT'] = int(reg[210:217])
        dic['PTOEXE'] = float(reg[217:230])/1000000
        dic['CODISI'] = reg[230:242].strip()
        dic['DISMES'] = int(reg[242:245])
    #   
    elif reg[0:2] == '00': # Registro de metadados
        print("Arquivo criado em {}".format(datetime.strptime(reg[23:30], '%Y%m%d')))
        return
    elif reg[0:2] == '99': # Registro de metadados
        size = int(reg[31:42])
        if count != size:
            raise Exception("Arquivo Invalid, número de linhas diferente: foram processadas {}, mas era esperado {}".format(count, size))
        return    
    return dic
#
{% endhighlight %}

## Usando as funções acima

O uso das funções ficou bem simplicado, basta chamar as funções na seguinte ordem. :

{% highlight python %}
import warnings
warnings.filterwarnings("ignore")

ano = 2022
get_cotacoes(ano=ano)
processa_cotacoes(ano=ano)
{% endhighlight %}

Veja que eu parametrizo o python para não emitir warnings, já que estou usando o método `Pandas.append` que será descontinuado em versões futuras, o como você melhoria este código?

## Conclusão

Fizemos neste tutorial dois passos muito importantes para manutenção de sua base de dados para analise fundamentalista e técnica, você aprendeu aqui a baixar um arquivo diretamente no site da B3 e um segundo passo como processar o arquivo para ter um dataframe que será usado nos próximos tutoriais.

## Referências

* https://stackoverflow.com/questions/22676/how-to-download-a-file-over-http
* [CIEDA](http://cieda.com.br)