# Desafio 07: Implementação de Subprogramas e a Pilha de Chamadas

Este diretório contém a resolução do sétimo desafio, que foca na implementação de subprogramas. [cite_start]O objetivo é explicar o funcionamento da **pilha de chamadas (call stack)** através de um exemplo de função recursiva, demonstrando visualmente como o estado de cada chamada é gerenciado.

Para este exemplo, foi escolhida a função `fatorial(n)` em Python.

---

### O que é a Pilha de Chamadas (Call Stack)?

A pilha de chamadas é uma estrutura de dados do tipo LIFO (Last-In, First-Out) que as linguagens de programação usam para controlar a execução de subprogramas. Toda vez que uma função é chamada, um "bloco" de memória, conhecido como **registro de ativação** ou **stack frame**, é empilhado.

Este registro contém informações essenciais para a execução da função, como:
* Os parâmetros recebidos.
* As variáveis locais.
* O endereço de retorno (para onde o controle deve voltar quando a função terminar).

Quando a função termina, seu registro de ativação é desempilhado, e a execução retorna ao ponto onde a chamada foi feita.

### Exemplo Recursivo: Função Fatorial

A função fatorial é um exemplo clássico de recursão. Matematicamente, $n! = n * (n-1)!$, com o caso base $0! = 1$.

**Código de Exemplo (`fatorial.py`):**
```python
def fatorial(n):
    print(f"-> Chamando fatorial({n})")
    # Caso base: impede a recursão infinita
    if n == 0:
        print(f"<- Caso base atingido. Retornando 1.")
        return 1
    # Passo recursivo: a função chama a si mesma
    else:
        resultado_parcial = fatorial(n - 1)
        resultado_final = n * resultado_parcial
        print(f"<- Calculando {n} * fatorial({n-1}) = {resultado_final}. Retornando {resultado_final}.")
        return resultado_final

# Chamada inicial da função
resultado = fatorial(5)
print(f"\nO fatorial de 5 é: {resultado}")
```
### Exemplo 
<img width="635" height="483" alt="image" src="https://github.com/user-attachments/assets/12719c09-62d2-4477-936c-c360029993eb" />
