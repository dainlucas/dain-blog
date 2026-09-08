---
title: "rv-tracelab: aprendendo RISC-V pelo caminho mais longo"
slug: "rv-tracelab-primeiros-passos"
date: 2026-09-08T10:00:00-03:00
draft: false
description: "O plano inicial de um buffer circular de rastros e decodificador RV32I escrito em C para estudar sistemas por dentro."
translationKey: "rv-tracelab-first-steps"
authors:
  - name: "Lucas Ruan"
    link: "https://github.com/dainlucas"
    image: "/images/avatar.webp"
tags:
  - "C"
  - "RISC-V"
  - "arquitetura de computadores"
  - "baixo nível"
categories:
  - "Sistemas"
images:
  - "cover.webp"
toc: true
---

![Representação de um buffer circular conectado a um decodificador de instruções](cover.webp)

O `rv-tracelab` começou com uma pergunta simples: quanto eu consigo aprender sobre C e arquitetura de computadores construindo uma ferramenta pequena, em vez de apenas ler sobre os conceitos?

Minha resposta provisória é um buffer circular que guarda rastros de execução e um decodificador para um subconjunto de RV32I. O projeto ainda está no começo. Neste texto, registro o desenho antes da implementação — inclusive as decisões que provavelmente vou precisar revisar.

<!--more-->

## O recorte

Cada entrada do rastro terá duas palavras de 32 bits:

```c {filename="tracebox.h"}
typedef struct {
    uint32_t pc;
    uint32_t instruction;
} TraceEntry;
```

O `pc` diz onde a instrução estava; `instruction` guarda a palavra crua que será decodificada depois. A primeira versão do estado do buffer deve ser próxima disto:

```c {filename="tracebox.h"}
typedef struct {
    TraceEntry *entries;
    size_t capacity;
    size_t count;
    size_t head;
} TraceBox;
```

Não há muita coisa aqui, e esse é o ponto. Quero conseguir explicar o papel de cada campo e manter algumas invariantes visíveis:

- `count` nunca passa de `capacity`;
- depois que o buffer enche, uma nova escrita substitui a entrada mais antiga;
- a leitura apresenta as entradas em ordem cronológica, mesmo depois de uma ou várias voltas;
- capacidade zero, ponteiros nulos e falhas de alocação são erros tratados, não comportamento indefinido acidental.

## Por que um buffer circular

Um rastreador não precisa guardar a execução inteira. Para diagnosticar o que acabou de acontecer, as últimas `N` entradas costumam ser mais úteis do que um histórico sem limite.

O buffer circular força uma separação importante entre **ordem física** e **ordem lógica**. Os elementos deixam de estar ordenados do índice zero em diante assim que ocorre a primeira volta. A interface de leitura precisa esconder esse detalhe sem apagar a realidade da estrutura.

É um problema pequeno que toca em alocação, aritmética modular, limites e desenho de API — exatamente o tipo de exercício que procuro.

## O decodificador mínimo

O segundo pedaço do projeto transforma palavras de instrução em campos que possam ser entendidos e exibidos. O primeiro subconjunto planejado é:

| Instrução | Formato | O que quero praticar |
| --- | --- | --- |
| `ADD`, `SUB` | R | `opcode`, `funct3`, `funct7` e registradores |
| `ADDI` | I | imediato com extensão de sinal |
| `LW` | I | endereço base mais deslocamento |
| `SW` | S | imediato dividido em dois campos |
| `BEQ` | B | imediato remontado e alinhado para desvios |

Uma decisão já está tomada: decodificar e formatar serão tarefas separadas. Primeiro a palavra vira uma representação estruturada; depois outra função decide como mostrá-la. Isso permite testar máscaras e deslocamentos sem depender de uma string pronta, além de evitar que apresentação e regra de negócio cresçam misturadas.

Instruções válidas, mas ainda não suportadas, também precisam sobreviver ao caminho inteiro. A ferramenta deve preservar a palavra original e informar que não sabe interpretá-la, sem travar.

## Como pretendo avançar

O backlog está organizado em fatias que possam ser explicadas e testadas isoladamente:

1. definir `TraceEntry` e `TraceBox` antes da lógica;
2. implementar inicialização, liberação e inserção;
3. provar a ordem lógica nos casos vazio, parcial, cheio e sobrescrito;
4. decodificar uma instrução por vez;
5. integrar armazenamento, decodificação e formatação;
6. fechar com testes de regressão, AddressSanitizer e UndefinedBehaviorSanitizer.

A demonstração final deverá ultrapassar propositalmente a capacidade do buffer e imprimir apenas as entradas mais recentes, na ordem em que aconteceram. O `Makefile` terá pelo menos `make`, `make test` e `make clean`, compilando com `-Wall`, `-Wextra` e `-Wpedantic`.

## O critério que importa

O objetivo não é terminar com uma biblioteca impressionante. É chegar a uma versão `v0.1.0` cuja memória, bits e casos de borda eu consiga explicar sem atalhos.

O código e o andamento estão no [repositório do rv-tracelab](https://github.com/dainlucas/rv-tracelab). Conforme as hipóteses deste texto encontrarem a implementação, volto aqui para registrar o que mudou.

