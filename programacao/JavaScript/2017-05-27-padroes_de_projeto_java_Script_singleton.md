---
title: "Singleton - Padrões de Projeto em JavaScript"
tags: [JavaScript, Programar, Programação, ECMA, ECMAScript, WEB, WWW, Node.js, JQuery, módulos, CommonJS, ECMAScript 6, ES6, Padrões de Projeto, Criaçonal, Singleton, Prototype, Classes]
categories: [programacao, javascript, padroes_de_projeto]
layout: article
share: true
toc: true
comments: true
feature:
 category: true
 index: true
image:
 feature: linuxmacwindows/allOS-logos-900x210.png
 teaser: linuxmacwindows/macwindowslinux-500x210.png
ads:
 show: true
tagcloud: true
coinbase:
 show: true
---
Este post estreia uma série de posts sobre padrões de projeto na linguagem JavaScript, estes posts são baseados diretamente no livro "Mastering JavaScript Design PAtterns", escrito por Simon Timms e publicado pela Packt Publishing.

<!--more-->

## Consolidando alguns conceitos
Bem, para começarmos a falar de Padrões de projeto com a linguagem JavaScript, alguns conceitos ligados a Programação Orientada a Objetos precisam estar bem compreendidos. Portanto, vou dar inicio a este post explicando bem o conceito de prototype e como usa-lo para se ter sucesso em especial com o *"Padrão de Projetos"* **Singleton**.

Além disso, é importante salientarmos que iremos nos basear no ECMAScript 6 (ES6) para expressarmos nossos códigos no JavaScript, portanto é fundamental que se compreenda bem os novos conceitos, em espcial quando ao uso do `var` e `let` para decarlação de variáveis.

Apesar de nossos projetos estarem sendo migrados para TypeScript não iremos por hora abordar tal linguagem.

### Declarando Variáveis.

Bem, vamos começar esclarecendo a diferença entre 'let' e 'var', usaremos os conceitos conforme apresentado no livro "Learning ECMAscript 6" compilado por "Narayan Prusty" e publicado por "Packt publishing".

Até então o JavaScript não tinha nenhum mecanismos para definir o escopo de uma variável, sendo portanto as variáveis visiveis em toda a aplicação, sendo portanto todas globais, não importando onde elas eram declaradas.

Com o advento da especificação ES6, surgiu uma nova palavra chave para declarar variáveis, `let`, com esta nova palavra chave foi mantida a compatibilidade da linguagem com os códigos antigos que já tinha seus algoritmos baseados na declaração de variáveis com a palavra chave `var`.

Quando se usa `var` o JavaScript declara uma variável e anexa ela ao contexto global, sendo assim sua visiblidade é para todo o código, não importanto quando interno seja sua delcaração em algum bloco de código dentro de seu algoritmo.

Isso interfere bastante na funcionalidade em padrões de projeto como o Singleton, já que a variável que armazena a instância única da classe criada pode sofrer intervenções de outras partes do código, causando um mal funcionamento ou mesmo a quebra de seu código.

portanto, toda variável declarada por meio da palavra chave `let` é visivel em seu contexto e blocos inferiores.

### Prototype

Como muitos sabem, mas poucos usam, o javaScript e uma lingaugem orientada a objetos.

Possui 5 tipos primitivos:
* Undefined
* Null
* Boolean
* String
* Number

Dos 5 apenas 3 são objetos propriamente, Boolean, String e Number, estes são fácilmente encapsulados e desencapsulados de seu formato primitivo e diferem apenas pelas iniciais maísculas e mínusculas.


Por exemplo quando conecatenando String, comparando logicamente valores Booleanos, efetuando operações matematicas.

A criação de objetos em JavaScript é muito simples, você pode faze-la de duas formas, ou através do operador `new` ou usando um par de chaves `{}`, usando a notação JSON.

veja que vc não está declarando uma classe, mas estanciando um objeto que é declado em linha de código.

Assim, este objeto cada vez que é declarado, mesmo suas funções são novas entidades de código, tendo seu espaço de memória reservado, porém ainda se encontra o problema das declarações de váriaveis com a apalvra reservada `var`, pois estas variáveis estão anexadas ao contexto global.

Já as campos do objeto, estão anexadas a ele diretamente e não há o conceito de campo estática, já que este formato não existe a classe. Mas veremos a frente como isso foi contornado na Linguagem.

Internamente as funções e blocos de código, se pode definri variáveis usando a valavra reservada `let`que irá contextualizar a visiblidade da variável conforme ela é criada.

### `this`, seu uso.

A delcaração `this` é realmente um enigma a ser compreendido no JavaScript, e pode vir a causar bastante confusão para o iniciante, tivemos muitos transtorno no aprendizado da lingaugen, aplicando o conhecimento que foi adquirdo no Java e posteriormetne no C/C++.

No JavaScript podemos criar as funções (mensagens do objeto), veja em JavaScript não temos metodos, apenas funções, em qualquer parte do códigoe  em seguida atribuila a umo campo do objeto e assim usa-la como um membro do ojbeto, o que nos dá muita dinamicidade para estender objetos, mas isso é bastante perigoso, veremos isso nesta série.

Quando usamos `this` ele faz referencia ao objeto onde a função está sendo executada, e não criada, por exemplo:

Eu tenho a função x() criada na console de seu naevagdor, conforme abaixo:

``` JavaScript
> var x = function(){
   console.log(this);
}
```

Esta função quando executada na console, irá ter `this` fazendo referência ao objeto  `window` que é o contexto global e quem possuie o controle, ou seja é o dnbo da função quando sendo executada.

Agora, criando um objeto `o`:

``` JavaScript
var o = ();
```

e posteriormente, adicionar o  conteúdo de `x` que é uma função:

``` JavaScript
o.x = x;
```

Ao executar a função `o.x`, `this` já será uma referência ao objeto `o` e não mais o objeto `window`, assim será `this` igual a `o` e não mais a `window`;

Para minizar o desconforto de uso do `this`, caso venha ter, é preciso práticar seu uso exasltivamente, criando senários onde você transfere a propriedade( o dono, o senhorio) de funções entre objetos.

Uma forma de perceber isso é havendo um atributo `nome`, referênciada por `this.nome` em uma função quando definida em um objeto qualquer, ao dar esta função ao senhorio de outro objeto, ou seja permitir que outro objeto seja o prorprietário dela, este novo objeto pode não ter o atributo, e como tal situação não irá gerar um erro na maioria dos usos, retornando apenas como `undefined`, o erro pode ser detectado tarde de mais.

### protipando

Um último conceito que precisamos dominar antes de continuar com padrões de projeto em JavaScript é a prototipação de objetos, que é feita através do uso do campo `prototype` presente na definição de objetos, ou seja na classe do objeto.

Quando definimos um objeto, usando a notação de classe, por exemplo:

``` JavaScript
var MinhaClasse = function(){
   // aqui vc define propriedades, atributos
   // e funções, mensagens.
}
```

A Classe acimna é a definição de um objeto que é do tipo MinhaClasse, ele não tem nenhuma propreidade/atributo e nenhuma função/mensagem.

Para usar a classe acima, precisamos instancia-la em um objeto, da seguinte forma:

``` JavaScript
var objetoMinhaClasse =  new MinhaClasse();
```

Mas não temos nenhum atributo ou mensagem para esta classe, então nosso objeto não faz nada.

Podemos ampliar (não vou usar o temo extender aqui, em outro post veremos a diferença entre ampliar uma classe, e extender a classe), adiiconando novas propreidades/atributos e novas funções a então criada classe.

Assim:

``` JavaScript
MinhaClasse.prototype.nome = "";
MinhaClasse.prototype.mostreNome = function(){
   this.nome;
}
```
Fazendo testes com o objeto criado deste classe, você verá que ele automáticamente possui os novos atributos adicionados, e mensagnes. em cada instãncia da classe o objeto possui seu proprio this, mas compartilha a implementação, não ocupando memória extra quanto a implemntação das mensagens/funçoes.

o campo/atributo `prototype`é um array que contem as propriedades adicioando a classe.

### Outra forma de criar objetos

No ECMAScript 5 foi adicionado uma nova sintaxe para se criar objetos, ela usa a seguinte função presente na classe Objeto:

```JavaScript
Object.create(prototype[, propriedades do objeto]);
```

as propriedades do objeto, é um objeto JSON, qu descreve cada campo do objeto, definindo se ele pode ser:

* _writable_: ou seja o campo é alterável;
* _configurable_: se ele pode ser removido e reconfigurado futuramente;
* _enumerable_: se será listado, quando acessando a lista de propriedades(campos) do objeto;
* _value_: valor padrão do objeto, usado quando craido.

Esta é uma forma muito eficaz de extender uma classe já existente, o prototipo informado, e instancia-la consecutivamente.

### Noviddades no ECMAscript 6

No ECMAScript 6, ou simplesmente ES6, que também é conhecido como *Harmony*, foram definidos novos instrumentos para trabalhar com Classes e Módulos, deixando a linguagem mais familiar para quem vem de linguagens cxomo Java e C++, e até mesmo PHp. E claro dando mais poder para manipular objetos e classes.

por hora veremos simples a sintaxe usada para criar uma classe que está eposta abaixo;

``` JavaScript
class ECG extend BioSignal{
   constructor(name,aliegience){
      super(name);
      ...
   }

   consolidar(){
      ...
      super.consolidar();
      ...
   }
}
```

Vamos deixar para um post futuro o uso de módulos e e xportação, importação de funções e Classes.

## O Padrão de projeto
E porque estamos vendo estes conceitos de forma a consolida-los? Nosso objetivo aqui é estudar padrões de projeto com a Lingaugem JavaScript e suas variações como o TypeScript, mas aqui nesta série de posts veremos apenas a lingaugem JavaScript em especial com foco no ECMAscript 6.


O Padrão de projeto é um conjunto de práticas de condificação onde blocos de códigos tem funcionalidadees bastante especificas e atendem a necessidades arquiteturais de códigos para qualquer sistema.

Os padrões de projeto são blocos de código reaproveitáveis em objetos de forma a não otimizar sua aplicação, mas como também dar maior legibilidade e compreenção do código, estrutuando e auxiliando na modularização do código.

Os padrões de projeto inicialmente foram catalogados per um grupo de Engenheiros de Softwares conhecidos como Gang of Four,