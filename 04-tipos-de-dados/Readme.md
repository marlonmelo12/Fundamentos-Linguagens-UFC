# Desafio 04: Tipos de Dados

Este diretório contém a análise comparativa entre os sistemas de tipos de dados das linguagens C, Python e JavaScript, conforme solicitado no quarto desafio da disciplina.

[cite_start]O objetivo é demonstrar na prática as diferenças entre tipagem estática e dinâmica, e forte e fraca, utilizando exemplos de código breves e comentados.

---

### Conceitos Fundamentais de Tipagem

Antes de comparar as linguagens, é importante definir dois eixos principais de classificação de tipos:

1.  **Tipagem Estática vs. Dinâmica:**
    * **Estática:** A verificação dos tipos é feita em tempo de **compilação**. O tipo de uma variável é fixo e deve ser declarado explicitamente. Tentar atribuir um valor de tipo incompatível a uma variável gera um erro de compilação. (Ex: C, Java, Rust).
    * **Dinâmica:** A verificação dos tipos é feita em tempo de **execução**. O tipo está associado ao valor, não à variável. Uma mesma variável pode armazenar valores de tipos diferentes ao longo da execução do programa. (Ex: Python, JavaScript, Ruby).

2.  **Tipagem Forte vs. Fraca:**
    * **Forte:** A linguagem impõe regras estritas sobre como os tipos podem interagir. Operações entre tipos incompatíveis são proibidas, exigindo uma conversão explícita (um *cast*). Isso torna o código mais previsível e seguro. (Ex: Python, Java).
    * **Fraca:** A linguagem tenta "ajudar" o programador realizando conversões implícitas (coerção) quando se depara com tipos diferentes em uma operação. Isso pode levar a resultados inesperados e a erros difíceis de rastrear. (Ex: JavaScript, C).

---

### Análise Comparativa das Linguagens

A seguir, analisamos cada linguagem com base nos conceitos acima.

#### 1. Linguagem C

* **Classificação:** Estática e Fraca.
* **Análise:** Em C, toda variável precisa ter seu tipo declarado antes do uso (`int idade;`), e o compilador verifica se as atribuições são compatíveis. No entanto, é considerada de tipagem fraca por permitir certas conversões implícitas (como entre `char` e `int`) e por dar ao programador o poder de contornar o sistema de tipos através de ponteiros e *casts* explícitos, o que pode comprometer a segurança.

**Exemplo de código (`exemplo.c`):**
```c
#include <stdio.h>

int main() {
    // Tipagem Estática: o tipo 'int' é definido em tempo de compilação.
    // A linha 'idade = "vinte";' causaria um erro de compilação.
    int idade = 28;

    // Tipagem Fraca: o char 'A' é implicitamente convertido para seu
    // valor na tabela ASCII (65) para que a soma possa ocorrer.
    char letra = 'A';
    int resultado = idade + letra; // Operação válida em C

    printf("Idade: %d\n", idade);
    // Saída -> Idade: 28

    printf("Resultado ('idade' + 'A'): %d\n", resultado);
    // Saída -> Resultado ('idade' + 'A'): 93 (pois 28 + 65 = 93)

    return 0;
}
```

#### 2. Linguagem Python
Classificação: Dinâmica e Forte.

Análise: Python não exige a declaração do tipo da variável; o tipo é inferido em tempo de execução a partir do valor atribuído. A tipagem é forte porque o interpretador não permite operações entre tipos incompatíveis, como somar um número e uma string, lançando um TypeError e forçando o programador a fazer conversões explícitas.

Exemplo de código (exemplo.py):
```python
# Tipagem Dinâmica: a variável 'dado' pode referenciar
# valores de tipos diferentes durante a execução.
dado = 28
print(f"Valor: {dado}, Tipo: {type(dado)}")
# Saída -> Valor: 28, Tipo: <class 'int'>

dado = "vinte e oito"
print(f"Valor: {dado}, Tipo: {type(dado)}")
# Saída -> Valor: vinte e oito, Tipo: <class 'str'>

# Tipagem Forte: tentar somar um int com uma str resulta em erro.
# Nenhuma conversão implícita é realizada.
try:
    resultado = 28 + " anos"
except TypeError as e:
    print(f"Erro: {e}")
    # Saída -> Erro: unsupported operand type(s) for +: 'int' and 'str'
```

#### 3. Linguagem JavaScript
Classificação: Dinâmica e Fraca.

Análise: Assim como Python, JavaScript possui tipagem dinâmica. Contudo, é notoriamente conhecida por sua tipagem fraca. O motor do JavaScript realiza coerções de tipo de forma agressiva para tentar fazer com que uma operação funcione, o que pode gerar resultados surpreendentes e, por vezes, indesejados.

Exemplo de código (exemplo.js):

```js
// Tipagem Dinâmica: a variável pode mudar de tipo.
let dado = 28;
console.log(`Valor: ${dado}, Tipo: ${typeof dado}`);
// Saída -> Valor: 28, Tipo: number

dado = "vinte e oito";
console.log(`Valor: ${dado}, Tipo: ${typeof dado}`);
// Saída -> Valor: vinte e oito, Tipo: string

// Tipagem Fraca: o número 28 é convertido (coagido) para uma string
// para que a concatenação com " anos" possa ocorrer.
let resultado1 = 28 + " anos";
console.log(`Resultado: "${resultado1}", Tipo: ${typeof resultado1}`);
// Saída -> Resultado: "28 anos", Tipo: string

// Outro exemplo de coerção: a string "5" é convertida para número
// para que a subtração funcione.
let resultado2 = "5" - 2;
console.log(`Resultado: ${resultado2}, Tipo: ${typeof resultado2}`);
// Saída -> Resultado: 3, Tipo: number
```

### Quadro Comparativo

<img width="666" height="184" alt="image" src="https://github.com/user-attachments/assets/be893343-4fbd-49e2-94a7-35aacd74b116" />

