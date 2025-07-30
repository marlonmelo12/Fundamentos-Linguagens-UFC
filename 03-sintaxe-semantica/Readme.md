# Desafio 03: Descrições Sintáticas e Semânticas

Este diretório contém a especificação de uma linguagem de programação fictícia, criada para cumprir o terceiro desafio da disciplina. O objetivo é demonstrar a compreensão sobre **análise léxica** (a identificação de tokens) e **descrição sintática** (a criação de uma gramática que define as regras da linguagem).

---

### 1. A Linguagem Fictícia: "melo"

**Conceito:** A linguagem "melo" é uma linguagem de script minimalista projetada para controlar o estado de uma lâmpada inteligente. Ela permite ligar, desligar, definir a intensidade do brilho e agendar uma ação para um horário específico.

// Ligar a lâmpada com brilho máximo
LIGAR;
DEFINIR BRILHO = 100;

// Desligar a lâmpada em um horário agendado
AGENDAR "22:30" {
DESLIGAR;
}

---

### 2. Análise Léxica

A análise léxica é a primeira fase da interpretação de um programa. Ela consiste em ler o código-fonte como uma sequência de caracteres e convertê-lo em uma sequência de unidades léxicas, chamadas **tokens**.

#### Tokens da Linguagem "melo"

Abaixo estão todos os tokens que o analisador léxico da "melo" reconheceria:

| Token             | Lexema (Exemplo)        | Descrição                                         |
| ----------------- | ----------------------- | ------------------------------------------------- |
| `PALAVRA_CHAVE`   | `LIGAR`, `DESLIGAR`     | Comandos de ação direta.                          |
| `DEFINIR_CMD`     | `DEFINIR`               | Inicia um comando de definição de propriedade.    |
| `AGENDAR_CMD`     | `AGENDAR`               | Inicia um bloco de agendamento.                   |
| `PROPRIEDADE`     | `BRILHO`                | Nome de uma propriedade da lâmpada.               |
| `ATRIBUICAO`      | `=`                     | Símbolo de atribuição.                            |
| `NUMERO_INTEIRO`  | `100`, `50`, `0`        | Um valor numérico inteiro.                        |
| `TEXTO`           | `"22:30"`               | Uma cadeia de caracteres entre aspas.             |
| `ABRE_BLOCO`      | `{`                     | Inicia um bloco de código.                        |
| `FECHA_BLOCO`     | `}`                     | Finaliza um bloco de código.                      |
| `FIM_COMANDO`     | `;`                     | Símbolo que termina uma instrução.                |

#### Exemplo de Tokenização

Analisando o seguinte código:

DEFINIR BRILHO = 75;

O analisador léxico produziria a seguinte sequência de tokens:

`[DEFINIR_CMD, PROPRIEDADE("BRILHO"), ATRIBUICAO, NUMERO_INTEIRO(75), FIM_COMANDO]`

---

### 3. Descrição Sintática (Gramática)

A sintaxe define como os tokens podem ser combinados para formar estruturas válidas na linguagem. A gramática abaixo está escrita em um formato similar ao **BNF (Backus-Naur Form)**, que é uma notação padrão para descrever gramáticas.

#### Regras da Gramática "melo"

```bnf
<programa> ::= <lista_de_comandos>

<lista_de_comandos> ::= <comando> | <comando> <lista_de_comandos>

<comando> ::= <comando_simples> | <comando_definir> | <comando_agendar>

<comando_simples> ::= (LIGAR | DESLIGAR) FIM_COMANDO

<comando_definir> ::= DEFINIR_CMD PROPRIEDADE ATRIBUICAO NUMERO_INTEIRO FIM_COMANDO

<comando_agendar> ::= AGENDAR_CMD TEXTO ABRE_BLOCO <lista_de_comandos> FECHA_BLOCO
