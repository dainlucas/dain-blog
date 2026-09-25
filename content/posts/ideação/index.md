---
title: Você não precisa de aplicativos
date: 2026-09-24T21:14:18-03:00
draft: false
description: ""
authors:
  - name: Lucas Ruan
    link: https://github.com/dainlucas
    image: /images/avatar.webp
tags: []
categories: []
---

É muito frequente entre pessoas do meu nicho surgirem ideias de resolução de problemas. Estamos sempre fazendo um *scan* automático no mundo em busca de coisas para melhorarmos. Boa parte dessas soluções passa pela ideia de construirmos um aplicativo cheio de funcionalidades, um sistema web capaz de suportar milhares de usuários ou qualquer coisa do tipo que faça os olhos brilharem. Porém, na maioria dos casos, a solução mais realista e justa é a menos trabalhosa e mais simples.

<!--more-->

## O "Grande" projeto

Desde maio venho trabalhando num projeto pessoal de coleta de notícias do site da faculdade e envio de notificações sobre elas para os alunos. Em longos 5 meses, eu desenvolvi essa aplicação:

No 1º mês, a ideia era o robozinho varrer todo o site e me falar quais novas páginas surgiram, para que eu conseguisse acompanhar o lançamento de editais e notícias. Isso, no primeiro mês, se provou perigoso, pois o site possuía diversas páginas fantasmas que faziam o bot realizar MUITAS requisições ao servidor em um curto espaço de tempo.

No 2º mês, eu foquei em educar o meu bot para que ele não me denunciasse para o setor de TI da faculdade. Então eu criei mecanismos para fazer com que ele fizesse as requisições de forma espaçada e tivesse um limite total delas.

No 3º mês, eu mudei totalmente de ideia. O fato de o robô percorrer todo o site procurando novas páginas era um caos total, e eu estava cansado de bagunça, queria algo mais limpo. Pensando nisso, eu mudei a ideia de o sistema ser 100% autônomo. Agora eu deveria pegar o link manualmente no site e cadastrar no monitorador. Daí o robô acompanharia apenas essas páginas.

No 4º mês, eu foquei no Telegram, em como ele enviaria as mensagens para lá e como interagiria com os usuários, como seria a usabilidade etc.

No 5º mês, era a hora do deploy. Consegui 1 ano grátis de uma VM da Azure e poderia deixar o sistema rodando 24 horas por dia por lá durante esse tempo. Porém, eu não tomei essa decisão. Pensei no futuro: o que aconteceria com meu sistema se eu tivesse muitos usuários e acabasse meu teste grátis? Eu teria a obrigação de ficar adicionando links de páginas do site no sistema e ainda por cima arcar com os custos de uma VPS posteriormente.

## 5 meses em 1 dia

Na semana de eventos da faculdade, assisti a uma palestra com um rapaz de 26 anos chamado Gabriel Fontes, que fez uma dinâmica sobre a ideação de um novo produto. Dentre diversas coisas, ele nos mostrou na prática como moldar a ideia de uma pessoa "acelerada" de modo a torná-la real e eficiente. Depois de ver essa palestra, eu fiquei maravilhado com o novo conhecimento que tinha acabado de adquirir. Logo fui atrás de onde aplicar.

O projeto dos editais caiu como uma luva: já estava há longos meses em desenvolvimento e ainda teríamos um custo altíssimo. Foi daí que resolvi aplicar os conhecimentos aprendidos. Durante as pausas entre palestras, refleti sobre como tornar minha aplicação grátis e eficiente, mantendo a funcionalidade principal.

Depois de muitos pensamentos e pesquisas, consegui traçar um plano e, no mesmo dia, coloquei em ação. Resumo: consegui planejar, desenvolver e colocar para funcionar em apenas 1 dia a funcionalidade principal do meu projeto de 5 meses.
