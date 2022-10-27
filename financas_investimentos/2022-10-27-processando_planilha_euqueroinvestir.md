---
title: Processando a Planilha de dados fundamentalistas do Site Eu Quero Investir (EQI)
tags: [b3, investimento, ciencia de dados, acoes, ativos, aplicação, financeiro, status invest, analise fundamentalista, analise]
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

Já fizemos um tutorial onde obtemos os dados de uma planilha fornecida pela empresa EQI (http://eqi.com.br), a planilha em questão foi criada com base nos dados obtidos no StatusInvest e melhorado de forma, por exemplo, a conter detalhes como Setor e SubSetor no qual pertence o ativo. Porém, faremos aqui, com o intuito de apenas acelerar a obtenção e ampliar as possibilidades, já que nosso intuito é educacional e quanto mais formas de obtenção de dados forem apresentadas melhor, pois o aprendiz pode somar tais informações construindo um leque maior de conhecimento. 

<!--more-->
É importante destacar que a planilha da EQI vem com os dados ampliados e atualizados, pois possui fórmulas que obtêm, por exemplo, o valor de mercado do ativo em tempo real, com pequeno atraso. Já outros dados precisam ser atualizados copiando e colando do site StatusInvest. 

A falta do valor de mercado no arquivo nos dados fornecidos diretamente no site StatusInvest pode ser corrigido integrando com outras bibliotecas como Yahoo Finances, que já vem integrada no Pandas através da biblioteca DataReader.

Obter dados diretamnte no site StatusInvest é simples, basta seguir a URL abaixo com a função `request` do módulo `requests` e os dados para uma analise fundamentalistas de todas as ações serão retornados em formato Json similar a um Dicionário do Python, sendo muito simples de ser convertido para um DataFrame.

```
url_ba = 'https://statusinvest.com.br/category/advancedsearchresult?search=%7B%22Sector%22%3A%22%22%2C%22SubSector%22%3A%22%22%2C%22Segment%22%3A%22%22%2C%22my_range%22%3A%220%3B25%22%2C%22dy%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_L%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_VP%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_Ativo%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22margemBruta%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22margemEbit%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22margemLiquida%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_Ebit%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22eV_Ebit%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22dividaLiquidaEbit%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22dividaliquidaPatrimonioLiquido%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_SR%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_CapitalGiro%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22p_AtivoCirculante%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22roe%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22roic%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22roa%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22liquidezCorrente%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22pl_Ativo%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22passivo_Ativo%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22giroAtivos%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22receitas_Cagr5%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22lucros_Cagr5%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%2C%22liquidezMediaDiaria%22%3A%7B%22Item1%22%3Anull%2C%22Item2%22%3Anull%7D%7D&CategoryType=1'
import requests as req 

response = req.get(url_si)
resp_json = response.json()
```

O código acima é autoexplicativo, temos como resultado final na variável `resp_json` 
Como o conteúdo retornando que está no formato JSON práticamente como um dicionário, portanto basta passa-lo diretamente para o pandas que ele cuidará de como criar o DataFrame.

```
import pandas as pd
df = pd.DataFrame(resp_json)
```

Pronto temos o DataFrame para usarmos e fazermos nossa analise fundamentalista, porém vamos fazer ainda alguns ajustes para que fique compátivel com outros dataframes que usaremos no futuro e de outras fontes.

## Ajustando os nomes das colunas para um formato padrão

Para mantermos um padrão dos nomes das colunas faremos uma correção dos nomes retornados com o código no dicionário definido logo a seguir:

```
col_dic = { \
"companyId": "Company ID",
"companyName": "NOME DA EMPRESA",
"price": "PREÇO DO ATIVO",
"valorMercado": "VALOR DE MERCADO",
"dy": "DIVIDEND YELD",
"p_L": "P/L",
"p_VP": "P/VP",
"p_Ebita": "P/EBITDA",
"p_Ebit": "P/EBIT",
"p_SR": "PSR",
"p_Ativo": "P/ATIVO",
"p_CapitalGiro": "P/CAP. GIRO",
"p_AtivoCirculante": "P/ACL",
"pl_Ativo": "PL / ATIVO CIRC.",
"ev_ebitda": "EV/EBITDA",
"ev_ebit": "EV/EBIT",
"lpa": "LPA",
"vpa": "VPA",
"peg_Ratio": "PEG Ratio",
"dividaliquidaPatrimonioLiquido": "DIV. LIQ. / PATRI.",
"dividaliquidaEbitda": "DIV. LIQ. / EBITDA",
"dividaLiquidaEbit": "DIVIDA LIQUIDA / EBIT",
"patrimonio_ativo": "PATRIMONIO / ATIVOS",
"passivo_Ativo": "PASSIVOS / ATIVOS",
"liquidezCorrente": "LIQ. CORRENTE",
"liquidezMediaDiaria": "LIQ. MEDIA DIARIA",
"margemBruta": "MARGEM BRUTA",
"margemEbitda": "MARGEM EBITDA",
"margemEbit": "MARGEM EBIT",
"margemLiquida": "MARG. LIQUIDA",
"roe": "ROE",
"roa": "ROA",
"roic": "ROIC",
"giroAtivos": "GIRO ATIVOS",
"receitas_Cagr5": "CAGR RECEITAS 5 ANOS",
"lucros_Cagr5": "CAGR LUCRO 5 ANOS" 
}
```

Então aplicaremos no DataFrame através da função `rename` e em seguida usarei a coluna *ticker* como indice do dataframe.

```
df.rename(columns=col_dic)
df.set_index(keys="ticker", drop=True, inplace=True)
```

## Conclusão

Pronto já temos o dataframe pronto para ser usado em nosso próximo artigo, caso não tenha inda seguido os passos para obter os dados da planilha da Agencia EQI sugiro que também o faça pois deixarei o desafio para você unir os dois dataframes construindo um Dataframe mais completo.

O nosso próximo artigo ensinará como obter informações fundamentalistas de ambos os DataFrames, seja o obtido com a planilha da Agencia EQI ou diretamente no site da StatusInvest.

## Referências

* [CIEDA](http://www.cieda.com.br)
* https://statusinvest.com.br/acoes/busca-avancada
* https://github.com/lfreneda/statusinvest

