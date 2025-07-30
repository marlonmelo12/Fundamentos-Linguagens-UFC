# Desafio 06: Subprogramas e Passagem de Parâmetros

Este diretório contém a resolução do sexto desafio, focado em subprogramas. O objetivo é implementar e comparar os mecanismos de passagem de parâmetros **por valor** e **por referência**. Para isso, foram utilizadas as linguagens C e Python, conforme a sugestão do guia do trabalho, por demonstrarem esses conceitos de forma clara e contrastante.

---

### Conceitos: Valor vs. Referência

A forma como os dados são passados para uma função (subprograma) impacta diretamente se a função pode ou não alterar a variável original que foi passada como argumento.

1.  **Passagem por Valor (Pass-by-Value):**
    A função recebe uma **cópia** do valor do argumento. Qualquer modificação feita no parâmetro dentro da função afeta apenas a cópia local, não alterando a variável original fora da função. É o método mais seguro para evitar efeitos colaterais indesejados.

2.  **Passagem por Referência (Pass-by-Reference):**
    A função recebe o **endereço de memória** da variável original. Dessa forma, o parâmetro dentro da função se torna um "apelido" para a variável original. Qualquer modificação no parâmetro altera diretamente o valor da variável original.

---

### Implementação em C

A linguagem C nos permite demonstrar os dois mecanismos de forma explícita. Por padrão, C passa argumentos por valor. Para simular a passagem por referência, utilizamos ponteiros.

**Código de Exemplo (`parametros.c`):**

```c
#include <stdio.h>

// 1. PASSAGEM POR VALOR
// A função recebe uma CÓPIA de 'numero'.
// A alteração em 'numero' aqui dentro não afeta a variável 'original_valor'.
void adiciona_dez_por_valor(int numero) {
    numero = numero + 10;
    printf("Dentro da função (por valor): %d\n", numero);
}

// 2. PASSAGEM POR REFERÊNCIA (simulada com ponteiros)
// A função recebe o ENDEREÇO de 'numero' (*ponteiro_numero).
// Ao alterar o conteúdo do endereço, a variável 'original_referencia' é modificada.
void adiciona_dez_por_referencia(int *ponteiro_numero) {
    *ponteiro_numero = *ponteiro_numero + 10;
    printf("Dentro da função (por referência): %d\n", *ponteiro_numero);
}

int main() {
    int original_valor = 20;
    int original_referencia = 20;

    printf("--- Testando Passagem por Valor ---\n");
    printf("Valor original ANTES da função: %d\n", original_valor);
    adiciona_dez_por_valor(original_valor);
    printf("Valor original DEPOIS da função: %d\n\n", original_valor);

    printf("--- Testando Passagem por Referência ---\n");
    printf("Valor original ANTES da função: %d\n", original_referencia);
    // Passamos o endereço de memória da variável usando o operador '&'
    adiciona_dez_por_referencia(&original_referencia);
    printf("Valor original DEPOIS da função: %d\n", original_referencia);

    return 0;
}
```
### Implementação em Python

A passagem de parâmetros em Python é mais sutil e frequentemente descrita como "passagem por atribuição" ou "passagem por referência de objeto".

```python
# 1. COMPORTAMENTO COM TIPOS IMUTÁVEIS (int, str, tupla)
# A variável 'numero' dentro da função é uma nova referência.
# A alteração não afeta a variável original.
def tenta_modificar_numero(numero):
    numero = numero + 10
    print(f"Dentro da função (número): {numero}")

# 2. COMPORTAMENTO COM TIPOS MUTÁVEIS (list, dict)
# A função recebe a referência para o MESMO objeto lista.
# Modificar o CONTEÚDO do objeto afeta a variável original.
def modifica_lista(minha_lista):
    minha_lista.append(4) # Esta modificação será visível fora
    print(f"Dentro da função (lista): {minha_lista}")


# --- Testes ---
print("--- Testando com Tipo Imutável (int) ---")
numero_original = 20
print(f"Valor original ANTES da função: {numero_original}")
tenta_modificar_numero(numero_original)
print(f"Valor original DEPOIS da função: {numero_original}\n")


print("--- Testando com Tipo Mutável (list) ---")
lista_original = [1, 2, 3]
print(f"Valor original ANTES da função: {lista_original}")
modifica_lista(lista_original)
print(f"Valor original DEPOIS da função: {lista_original}")
```
