
# Desafio 12: Programação Lógica com Sintaxe Prolog

Este diretório contém a resolução do décimo segundo desafio, focado no paradigma de Programação Lógica. [cite_start]O objetivo é modelar um pequeno problema lógico, e para isso, foi escolhido o domínio de uma árvore genealógica, utilizando uma sintaxe inspirada na linguagem Prolog.

---

### O Paradigma da Programação Lógica

Diferente do paradigma imperativo (onde damos ordens passo a passo) ou do orientado a objetos (onde modelamos entidades), a programação lógica é **declarativa**. Nós descrevemos o nosso "universo" através de:

1.  **Fatos:** Declarações que são incondicionalmente verdadeiras.
    * Exemplo: `homem(socrates).` -> "Sócrates é um homem."

2.  **Regras:** Declarações que definem como inferir novos fatos a partir de fatos existentes.
    * Exemplo: `mortal(X) :- homem(X).` -> "X é mortal SE X é um homem."

O programa, então, se torna uma "base de conhecimento". Nós fazemos **consultas (queries)** a essa base, e o interpretador Prolog usa um mecanismo de inferência para nos dar as respostas.

### Problema Escolhido: Relações de uma Árvore Genealógica

Nosso problema consiste em definir uma família fictícia através de fatos básicos (quem são os homens, as mulheres e quem são os pais de quem) e, a partir daí, criar regras para que o sistema consiga deduzir relações mais complexas como "mãe", "pai", "irmãos" e "avós".

### Implementação em Sintaxe Prolog

Abaixo está a base de conhecimento completa, que pode ser carregada em um interpretador como SWI-Prolog ou GNU Prolog.

#### 1. Base de Conhecimento (Fatos)

```prolog
% Fatos: definindo os indivíduos e seu gênero
homem(joao).
homem(jose).
homem(carlos).

mulher(ana).
mulher(maria).
mulher(clara).

% Fatos: definindo as relações de genitor (pai ou mãe)
% formato: genitor(PAI_OU_MAE, FILHO_OU_FILHA)
genitor(joao, jose).    % João é genitor de José
genitor(ana, jose).     % Ana é genitora de José

genitor(joao, clara).   % João é genitor de Clara
genitor(ana, clara).    % Ana é genitora de Clara

genitor(jose, carlos).  % José é genitor de Carlos
genitor(maria, carlos). % Maria é genitora de Carlos

genitor(jose, sofia).  % José é genitor de Sofia (Sofia não está na lista de mulher/homem)
genitor(maria, sofia). % Maria é genitora de Sofia
```
### 2. Relações Lógicas (Regras)

```prolog
% Regra para definir 'pai'
% P é pai de F SE P é genitor de F E P é homem.
pai(P, F) :- 
    genitor(P, F), 
    homem(P).

% Regra para definir 'mãe'
% M é mãe de F SE M é genitora de F E M é mulher.
mae(M, F) :-
    genitor(M, F),
    mulher(M).

% Regra para definir 'irmão'
% I1 é irmão de I2 SE I1 é homem, ambos têm o mesmo pai E a mesma mãe,
% E I1 não é a mesma pessoa que I2.
irmao(I1, I2) :-
    homem(I1),
    pai(P, I1),
    pai(P, I2),
    mae(M, I1),
    mae(M, I2),
    I1 \= I2. % Garante que a pessoa não seja irmã de si mesma

% Regra para definir 'irmã'
% I1 é irmã de I2 SE I1 é mulher e as outras condições de irmão são verdadeiras.
irma(I1, I2) :-
    mulher(I1),
    pai(P, I1),
    pai(P, I2),
    mae(M, I1),
    mae(M, I2),
    I1 \= I2.

% Regra para definir 'avô'
% A é avô de N SE A é pai de P E P é genitor de N.
avo(A, N) :-
    pai(A, P),
    genitor(P, N).

% Regra para definir 'avó'
% A é avó de N SE A é mãe de P E P é genitor de N.
avoh(A, N) :-
    mae(A, P),
    genitor(P, N).
```
### 3. Realizando Consultas (Queries)
Com a base de conhecimento carregada, podemos fazer perguntas ao sistema:

**Consulta 1:** Ana é mãe de José?
```prolog
?- mae(ana, jose).
true.
```

**Consulta 2:** Quem é o pai de Clara?
```prolog
?- pai(X, clara).
X = joao.
```

**Consulta 3:** Clara é irmã de José?
```prolog
?- irma(clara, jose).
true.
```

**Consulta 4:** Quem são os avós de Carlos?
```prolog
?- avo(X, carlos).
X = joao ;
false.

?- avoh(X, carlos).
X = ana ;
false.
```

**Consulta 5:** Carlos e Sofia são irmãos?
```prolog
?- irmao(carlos, sofia).
true.
```
