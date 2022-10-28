---
title: Criando credêncial para Google Cloud
tags: [google cloud, cloud, credencial, google api, google drive, google sheet, google drive api, google sheet api, google api]
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
Como criar uma credêncial de acesso ao Google Cloud para uso em serviços típo Bot.

<!--more-->

## Crie sua credêncial de acesso ao Google API para um robô

Usaremos nossos códigos como se fosse um robô de processamento, assim ficando bem mais simples o acesso, para isso é preciso ativar serviços do Google API e criar uma credêncial que deverá ser gravada na pasta de trabalho.

### Ativando os Serviços da API do Google

O primeiro passo é ativar a API do Google Drive e do Google Sheets em sua conta no Google, portanto você já deve ter um e-mail no GMail, para ativar as APIs siga cada um dos links e clique em ativar, são dois links distintos apesar de bastante similares:

* [https://console.cloud.google.com/marketplace/product/google/drive.googleapis.com](https://console.cloud.google.com/marketplace/product/google/drive.googleapis.com)
* [https://console.cloud.google.com/marketplace/product/google/sheets.googleapis.com](https://console.cloud.google.com/marketplace/product/google/sheets.googleapis.com)

Irei mostrar abaixo as telas apenas da ativação da API do Google Sheet, pois o processo é identico para ambos.

![Google Sheet API - Ativar]({{site.url}}/assets/images/financas_investimentos/telas/02-01-google-sheet-api.png)

![Google Sheet API - Ativado]({{site.url}}/assets/images/financas_investimentos/telas/02-02-google-sheet-api.png)

### Criando a Credêncial de acesso

Agora vamos criar as credênciais propriamente, para isso entre no link:

* [https://console.cloud.google.com/apis/credentials](https://console.cloud.google.com/apis/credentials)

* A credêncial a ser criada é para Bot (Robô), no menu está como "Conta de Serviço", para cria-la clique no botão "Criar Credênciais" em seguida escolha "Conta de Serviços".

![Criar Credêncial]({{site.url}}/assets/images/financas_investimentos/telas/03-01-credencial.png)

* Digite o nome que deseja dar a credêncial que está criando, pode ser o nome de seu projeto que irá usa-la.
![Nome da Credêncial]({{site.url}}/assets/images/financas_investimentos/telas/03-02-credencial.png)

* Em seguida defina o papel como sendo "proprietário", clique em continuar

![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-03-credencial.png)
![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-04-credencial.png)

* Nesta tela clique em continuar sem alterar nada.

![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-04-credencial.png)

* Agora role a tela até a base e clique no lápis correspondente na "Conta de Serviço" criada. 

![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-06-credencial.png)

* Em seguida vá na aba chaves e clique no botão "Adicionar Chave" e selecione "Criar Nova Chave".

![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-07-credencial.png)

* Na tela apresentada selecione a opção "JSon" e clique em "Criar". 

![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-08-credencial.png)

* Neste momento um arquivo será criado e baixado, clique em fechar, e mova o arquivo para a pasta que usará para digitar os comandos deste tutorial. 

![Definir o Papel Principal]({{site.url}}/assets/images/financas_investimentos/telas/03-09-credencial.png)

* Renomeie o arquivo para "service-account.json"
 


Para uso em outros cenários, reserve o e-mail encontrado na propriedade `client_email` do arquivo json que foi criado. Renomeie o arquivo para service_account.json e mova-o para o diretório que irá trabalhar.


## Referências

* [CIEDA](http://cieda.com.br)
* https://medium.com/@vince.shields913/reading-google-sheets-into-a-pandas-dataframe-with-gspread-and-oauth2-375b932be7bf
