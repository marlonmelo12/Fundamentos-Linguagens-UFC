# Desafio 10: Gerenciamento de Memória

Este diretório contém a resolução do décimo desafio, que consiste em uma pesquisa e comparação entre os modelos de gerenciamento de memória de duas linguagens de programação distintas. 

Seguindo a sugestão do guia do trabalho, a análise foca no contraste entre o **gerenciamento manual de memória em C** e o **gerenciamento automático via Garbage Collection (GC) em Java**. 

---

### Visão Geral

O gerenciamento de memória é o processo de alocar blocos de memória para programas quando eles precisam e liberá-los quando não são mais necessários, para que possam ser reutilizados. A forma como uma linguagem lida com esse processo é uma de suas características mais fundamentais, impactando diretamente a segurança, a performance e a complexidade do desenvolvimento.

A memória de um programa é tipicamente dividida em duas áreas principais:
* **Stack (Pilha):** Memória de alocação estática, gerenciada automaticamente pelo compilador. Armazena variáveis locais e chamadas de função. É rápida, mas limitada em tamanho.
* **Heap (Monte):** Memória de alocação dinâmica. É uma grande área de memória usada para armazenar objetos e dados cujo tamanho não é conhecido em tempo de compilação. É aqui que as diferenças entre C e Java se tornam mais evidentes.

### 1. Gerenciamento de Memória em C (Manual)

Em C, o programador tem controle total e responsabilidade total sobre a memória alocada no Heap.

* **Alocação:** A memória é alocada dinamicamente usando funções da biblioteca padrão, como:
    * `malloc(tamanho)`: Aloca um bloco de memória do tamanho especificado em bytes.
    * `calloc(n, tamanho)`: Aloca memória para um array de `n` elementos e a inicializa com zeros.
    * `realloc(ponteiro, novo_tamanho)`: Redimensiona um bloco de memória já alocado.

* **Liberação:** Uma vez que a memória não é mais necessária, o programador **deve** liberá-la explicitamente usando a função `free(ponteiro)`.

* **Riscos:** Este controle total vem com grandes riscos. Erros de gerenciamento de memória são comuns e perigosos:
    * **Memory Leak (Vazamento de Memória):** Ocorre quando o programador esquece de chamar `free()` para uma memória alocada que não será mais usada. O programa "perde" essa memória, que não pode ser reutilizada, podendo levar ao esgotamento de recursos.
    * **Dangling Pointer (Ponteiro Solto):** Ocorre quando `free()` é chamado, mas o ponteiro para aquela região de memória continua sendo usado. Tentar acessar essa memória pode causar falhas ou comportamentos imprevisíveis.

**Exemplo de Código em C:**
```c
#include <stdio.h>
#include <stdlib.h>

void exemplo_c() {
    // Aloca memória para um array de 5 inteiros no Heap
    int *array = (int*) malloc(5 * sizeof(int));
    
    // Verifica se a alocação foi bem-sucedida
    if (array == NULL) {
        printf("Falha na alocação de memória!\n");
        return;
    }
    
    printf("Memória alocada com sucesso.\n");
    
    // ... código que utiliza o array ...
    
    // Libera a memória explicitamente. Se esta linha for esquecida, ocorre um memory leak.
    free(array);
    printf("Memória liberada com sucesso.\n");
}
```

### 2. Gerenciamento de Memória em Java (Automático)
Java adota uma abordagem de "não se preocupe". O programador não gerencia a liberação de memória diretamente; essa tarefa é delegada a um processo automático chamado Garbage Collector (GC).

* **Alocação:** A memória no Heap é alocada usando a palavra-chave new.

   * `ArrayList<String> lista = new ArrayList<>();`

   * **Liberação:** O programador não chama nenhuma função para liberar memória. A Máquina Virtual Java (JVM) possui o Garbage Collector, um processo que roda em segundo plano e:
      - Identifica quais objetos no Heap não estão mais em uso (são "inalcançáveis"). Um objeto se torna inalcançável quando nenhuma variável ativa no programa possui uma referência a ele.
      - Automaticamente libera a memória ocupada por esses objetos.

  * **Riscos:** Vantagens e Desvantagens:

Vantagem: Elimina quase completamente os erros de memory leak e dangling pointers, tornando o desenvolvimento mais rápido e seguro.

Desvantagem: O GC pode introduzir pequenas pausas na execução do programa (eventos de "stop-the-world") enquanto realiza a limpeza, o que pode ser um problema para sistemas de tempo real.

Exemplo de Código em Java:
```java
import java.util.ArrayList;

public class GerenciamentoMemoriaJava {

    /**
     * Este método cria um objeto. Ao final de sua execução,
     * a referência para o objeto é perdida, tornando-o elegível
     * para o Garbage Collector.
     */
    public static void criarObjeto() {
        // Aloca memória para um objeto ArrayList no Heap usando 'new'
        ArrayList<String> lista = new ArrayList<>();
        System.out.println("  -> Objeto 'lista' criado no Heap.");
        lista.add("Item 1");
        lista.add("Item 2");
        System.out.println("  -> Objeto 'lista' utilizado.");
    } 
    // Ao final do método criarObjeto(), a referência 'lista' deixa de existir.
    // O objeto ArrayList no Heap se torna "inalcançável".

    public static void main(String[] args) {
        System.out.println("Iniciando a execução do programa...");
        
        criarObjeto();
        
        System.out.println("Método criarObjeto() finalizou. A memória do objeto 'lista' agora é elegível para coleta.");

        // Em um programa real, o programador NÃO chama o GC.
        // A linha abaixo é apenas uma SUGESTÃO para a JVM rodar o GC, sem garantias.
        // É usada aqui apenas para fins didáticos.
        System.gc();

        System.out.println("O programa foi finalizado.");
    }
}
```
### Quadro Comparativo
| Característica | Gerenciamento em C (Manual) | Gerenciamento em Java (Automático) |
| :--- | :--- | :--- |
| **Mecanismo Principal** | Controle explícito pelo programador. | Garbage Collector (GC) da JVM. |
| **Alocação no Heap** | Funções como `malloc()`, `calloc()`. | Palavra-chave `new`. |
| **Liberação no Heap** | Manual e obrigatória, usando a função `free()`. | Automática. O GC limpa objetos inalcançáveis. |
| **Responsabilidade** | Totalmente do programador. | Da Máquina Virtual Java (JVM). |
| **Riscos Comuns** | Memory Leaks, Dangling Pointers, Double Free. | Praticamente eliminados. O principal "risco" são pausas do GC. |
| **Complexidade** | Alta. Requer disciplina e atenção constantes. | Baixa. O programador foca mais na lógica de negócio. |
| **Performance** | Potencialmente mais rápido, pois não há o overhead do GC. | Pode sofrer com o overhead e pausas do GC, mas os algoritmos modernos são muito eficientes. |
