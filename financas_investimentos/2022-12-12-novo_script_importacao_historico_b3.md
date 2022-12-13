---
title: Novo Script de Importação do Histórico da B3
tags: [b3, investimento, ciencia de dados, acoes, ativos, aplicação, financeiro, dados historicos, statusinvest, serie historica, importacao, processamento, etl, extract-transform-load]
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

Tenho trabalhado no nosso app nestes dias e não tenho trazido novos materiais para nosso site, mas este trabalho me fez perceber a necessidade de fazer melhorias no script de ETL (Extract, Transform, Load) dos dados da B3.

<!--more-->

Fiz diversas atualizações a mais visivel é a separação em arquivos facilitando o reaproveitamento do código por quem deseja usar apenas o download ou processamento, além da função "dia_util" que foi melhorada, corrigindo o erro ao lidar com feriados.

Veja abaixo o resultado:

<script src="https://gist.github.com/carlosdelfino/f17caa6c66cd2b96c5715edeb6b624a3.js"></script>