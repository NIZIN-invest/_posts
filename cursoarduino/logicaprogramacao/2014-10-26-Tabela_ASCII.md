---
title: Tabela ASCII
excerpt: A tabela ASCII
tags: [ASCII, Tabela ASCII, Arduino, Linguagem, C/C++, C, C++, Programação]
categories: [HelloWorldArduino]
layout: article
share: true
toc: true
comments: true
image:
  teaser:
  feature:
  credit:
  creditlink:
imagebase: "/images/encoder/"
---
O nome ASCII é a breviação para American Standard Code for Information Interchange, foi criada com base
no idioma inglês e foi codificada sobre 7 bits, sendo 128 códigos existêntes, inicialmente.

A tabela possui em seu formato original, caractéres conficados como números de 0 a 9, letras de a até z
minúsculas e maiúsculas, simbolos de pontuação básicos comumente usados, códigos de controle, usados até 
hoje em impressores matriciais, criados na época dos teletipos, além de espaço.

Como dito a tabela foi criada para uso com teletipos e seu primeiro uso data de 6 de outubro de 1960.
Sendo o primeira versão do padrão lançado em 1963, a maior revisão foi em 1967, e a mais atual revisão
em 1986, atualmente utilizamos uma versão de 256 bytes, acrescida de caracteres utilizados em outros 
idiomas em especial de origem latina.

A tabela ASCII atualmente somente peder em uso para a tabela de caracteres UTF-8 conhecido como Conjunto
de caracter UTF-8 (ou CharSet UTF-8).

Abaixo apresentamos a tabela básica, que de maior interesse para uso com o Arduino, e a seguir a extensão da tabela. 

 
Inicialmente apresento a tabela indexa com números hexadecimais, e na segunda tabela com números décimais.

|                                            Números Hexadécimais                                             |
| :---------------------------------------------------------------------------------------------------------: |
|      | 0x0 | 0x1 | 0x2 | 0x3 | 0x4 | 0x5 | 0x6 | 0x7 | 0x8 | 0x9 | 0xA | 0xB | 0xC | 0xD | 0xE | 0xF |      | 
| ---- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | -----|
| 0x00 | NUL | SOH | STX | ETX | EOT | ENQ | ACK | BEL | BS  | TAB | LF  | VT  | FF  | CR  | SO  | SI  | 0x0F | 
| 0x10 | DLE | DC1 | DC2 | DC3 | DC4 | NAK | SYN | ETB | CAN | EM  | SUB | ESC | FS  | GS  | RS  | US  | 0x1F |
| 0x20 |     | !   | "   | #   | $   | %   | &   | '   | (   | )   | *   | +   | ,   | -   | .   | /   | 0x2F |
| 0x30 | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | :   | ;   | <   | =   | >   | ?   | 0x3F |
| 0x40 | @   | A   | B   | C   | D   | E   | F   | G   | H   | I   | J   | K   | L   | M   | N   | O   | 0x4F |
| 0x50 | P   | Q   | R   | S   | T   | U   | V   | W   | X   | Y   | Z   | [   | \   | ]   | ^   | _   | 0x5F |
| 0x60 | `   | a   | b   | c   | d   | e   | f   | g   | h   | i   | j   | k   | l   | m   | n   | o   | 0x6F |
| 0x70 | p   | q   | r   | s   | t   | u   | v   | w   | x   | y   | z   | {   | \|  | }   | ~   |     | 0x7F |


|                         Números Décimais                              |
| :-------------------------------------------------------------------: |
|     | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   |     |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 000 | NUL | SOH | STX | ETX | EOT | ENQ | ACK | BEL | BS  | TAB | 009 |
| 010 | LF  | VT  | FF  | CR  | SO  | SI  | DLE | DC1 | DC2 | DC3 | 019 |
| 020 | DC4 | NAK | SYN | ETB | CAN | EM  | SUB | ESC | FS  | GS  | 029 |
| 030 | RS  | US  |     | !   | "   | #   | $   | %   | &   | '   | 039 |
| 040 | (   | )   | *   | +   | ,   | -   | .   | /   | 0   | 1   | 049 |
| 050 | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | :   | ;   | 059 |
| 060 | <   | =   | >   | ?   | @   | A   | B   | C   | D   | E   | 069 |
| 070 | F   | G   | H   | I   | J   | K   | L   | M   | N   | O   | 079 |
| 080 | P   | Q   | R   | S   | T   | U   | V   | W   | X   | Y   | 089 |
| 090 | Z   | [   | \   | ]   | ^   | _   | `   | a   | b   | c   | 099 |
| 100 | d   | e   | f   | g   | h   | i   | j   | k   | l   | m   | 109 |
| 110 | n   | o   | p   | q   | r   | s   | t   | u   | v   | w   | 119 |
| 12  | x   | y   | z   | {   | \|  | }   | ~   |     |     |     | 129 |
