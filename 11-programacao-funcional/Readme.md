# Desafio 11: Programação Funcional

Este diretório contém a resolução do décimo primeiro desafio, focado em Programação Funcional. [cite_start]O objetivo é implementar uma solução para um problema que utilize os conceitos de **recursão** e **funções de alta ordem (Higher-Order Functions)**.

A solução foi desenvolvida em Python, uma das linguagens sugeridas, aproveitando suas funcionalidades para o paradigma funcional.

---

### Conceitos de Programação Funcional Aplicados

A programação funcional trata a computação como uma avaliação de funções matemáticas e evita estados e dados mutáveis. Os conceitos-chave aplicados neste desafio são:

1.  **Funções de Alta Ordem (Higher-Order Functions):** São funções que podem receber outras funções como argumentos ou retornar funções como resultado. `map()`, `filter()` e `reduce()` (do módulo `functools`) são exemplos clássicos.

2.  **Recursão:** É a técnica de uma função chamar a si mesma para resolver um problema, quebrando-o em subproblemas menores até atingir um caso base. É a forma de iteração preferida no paradigma funcional puro, em vez de loops como `for` ou `while`.

3.  **Imutabilidade:** Os dados não são alterados. Em vez de modificar uma lista existente, por exemplo, criamos uma nova lista com os dados transformados.

### Problema Escolhido: Pipeline de Processamento de Notas de Alunos

O nosso problema fictício é processar uma lista de dados de alunos. Queremos calcular a média das notas finais apenas dos alunos aprovados de um determinado curso (ex: "Ciência da Computação"), após conceder um ponto extra a todos eles.

O pipeline será:
1.  **Filtrar:** Selecionar apenas os alunos do curso desejado.
2.  **Mapear:** Aplicar o ponto extra na nota de cada aluno filtrado.
3.  **Filtrar novamente:** Selecionar apenas os alunos que foram aprovados (nota >= 7).
4.  **Reduzir (com recursão):** Calcular a soma total das notas dos alunos aprovados e, em seguida, a média.

### Código da Solução Funcional (`alunos.py`)

```python
from functools import reduce

# Lista de dados inicial (imutável)
alunos = [
    {'nome': 'Ana', 'curso': 'Engenharia', 'nota': 8.5},
    {'nome': 'Bruno', 'curso': 'Ciência da Computação', 'nota': 6.0},
    {'nome': 'Carla', 'curso': 'Ciência da Computação', 'nota': 9.5},
    {'nome': 'Daniel', 'curso': 'Arquitetura', 'nota': 7.0},
    {'nome': 'Eduarda', 'curso': 'Ciência da Computação', 'nota': 5.5},
    {'nome': 'Felipe', 'curso': 'Ciência da Computação', 'nota': 8.0},
]

# --- FUNÇÕES DE ALTA ORDEM E LAMBDAS ---

# 1. FILTRAR por curso
alunos_cc = list(filter(lambda aluno: aluno['curso'] == 'Ciência da Computação', alunos))
# print("Alunos de CC:", alunos_cc)

# 2. MAPEAR para adicionar um ponto extra
# A função map cria uma nova lista, preservando a imutabilidade da original.
def adicionar_ponto_extra(aluno):
    novo_aluno = aluno.copy() # Cria uma cópia para não modificar o original
    novo_aluno['nota'] += 1.0
    return novo_aluno

alunos_com_ponto_extra = list(map(adicionar_ponto_extra, alunos_cc))
# print("Alunos com ponto extra:", alunos_com_ponto_extra)


# 3. FILTRAR novamente por alunos aprovados (nota >= 7)
alunos_aprovados = list(filter(lambda aluno: aluno['nota'] >= 7.0, alunos_com_ponto_extra))
# print("Alunos aprovados:", alunos_aprovados)


# --- RECURSÃO PARA AGREGAR OS DADOS ---

def somar_notas_recursivo(lista_de_alunos):
    """
    Função recursiva para somar as notas de uma lista de alunos.
    """
    # Caso base: se a lista estiver vazia, a soma é 0.
    if not lista_de_alunos:
        return 0
    # Passo recursivo: soma a nota do primeiro aluno com a soma do resto da lista.
    else:
        primeiro_aluno = lista_de_alunos[0]
        resto_da_lista = lista_de_alunos[1:]
        return primeiro_aluno['nota'] + somar_notas_recursivo(resto_da_lista)


# --- CÁLCULO FINAL ---
if alunos_aprovados:
    soma_total_notas = somar_notas_recursivo(alunos_aprovados)
    media_final = soma_total_notas / len(alunos_aprovados)
    
    print("--- Pipeline de Processamento Funcional ---")
    print(f"Alunos de Ciência da Computação Aprovados: {[a['nome'] for a in alunos_aprovados]}")
    print(f"Soma das notas (recursiva): {soma_total_notas:.2f}")
    print(f"Média final dos aprovados: {media_final:.2f}")
else:
    print("Nenhum aluno aprovado encontrado após o processamento.")

# O mesmo cálculo de soma poderia ser feito com reduce, outra função de alta ordem:
# soma_com_reduce = reduce(lambda acumulador, aluno: acumulador + aluno['nota'], alunos_aprovados, 0)
