# Desafio 09: Concorrência

Este diretório contém a resolução do nono desafio, que aborda o tema de concorrência. [cite_start]O trabalho está dividido em duas partes: uma explicação teórica sobre as diferenças entre **threads** e **processos**, e uma implementação prática de um sistema concorrente personalizado. 

[cite_start]O exemplo foi desenvolvido em Python, utilizando o módulo `threading`, conforme uma das sugestões do guia do trabalho. 

---

### Parte 1: Diferença entre Threads e Processos

Embora ambos sejam formas de executar tarefas "ao mesmo tempo", threads e processos são fundamentalmente diferentes em como operam e gerenciam recursos.

**Processo:**
Um processo é uma instância de um programa em execução. O sistema operacional aloca para cada processo um **espaço de memória completamente isolado** e recursos próprios (como arquivos abertos e conexões de rede). Pense em cada aba do seu navegador Chrome como um processo separado; se uma delas travar, as outras continuam funcionando.

**Thread:**
Uma thread (ou linha de execução) é a menor unidade de execução *dent инновацион* um processo. Um único processo pode ter múltiplas threads rodando concorrentemente. Todas as threads de um mesmo processo **compartilham o mesmo espaço de memória** e os mesmos recursos. Pense em um editor de texto (o processo): uma thread pode estar salvando o arquivo em segundo plano enquanto outra thread continua respondendo à sua digitação.

#### Quadro Comparativo

| Característica | Processo | Thread |
| :--- | :--- | :--- |
| **Memória** | Cada processo tem seu próprio espaço de memória, isolado dos outros. | Todas as threads de um processo compartilham o mesmo espaço de memória. |
| **Criação** | Lenta e consome muitos recursos (overhead alto). | Rápida e leve (overhead baixo). |
| **Comunicação** | Lenta e complexa (requer IPC - *Inter-Process Communication*). | Rápida e simples (através da memória compartilhada). |
| **Isolamento** | Alto. Uma falha em um processo não afeta outros. | Baixo. Uma falha em uma thread pode derrubar todo o processo. |
| **Segurança** | Mais seguro devido ao isolamento. | Requer mecanismos de sincronização (ex: `Locks`) para evitar *race conditions*. |

---

### Parte 2: Exemplo de Concorrência Personalizado

#### Contexto: "Downloaders Concorrentes"

Para demonstrar a concorrência na prática, criei um script que simula o download concorrente de vários arquivos. Teremos múltiplos "downloaders" (threads) que pegam arquivos de uma lista de tarefas e os "processam".

* **Recurso Compartilhado:** A lista de arquivos a serem baixados (`fila_de_downloads`).
* **Problema de Concorrência:** Se várias threads tentarem acessar a lista ao mesmo tempo para pegar o próximo arquivo, pode acontecer uma *race condition* (condição de corrida), onde duas threads pegam o mesmo arquivo ou uma tenta pegar um item de uma lista que acabou de ficar vazia.
* **Solução:** Usamos um **`Lock` (Cadeado)**. Antes de uma thread acessar a lista, ela "adquire" o lock. Enquanto ela estiver com o lock, nenhuma outra thread pode acessar a lista. Ao terminar, ela "libera" o lock, permitindo que a próxima thread prossiga. Isso garante que o acesso ao recurso compartilhado seja atômico e seguro.

#### Código de Exemplo (`downloaders.py`)

```python
import threading
import time
import random

# Recurso compartilhado: uma lista de arquivos para "baixar"
fila_de_downloads = [
    "arquivo_A.zip", "imagem_HD.jpg", "documento_final.pdf",
    "video_aula.mp4", "planilha_2025.xlsx", "apresentacao.pptx"
]

# Lock para sincronizar o acesso à fila_de_downloads
lock_da_fila = threading.Lock()

def downloader(nome_thread):
    """
    Função que simula o trabalho de um downloader.
    Cada downloader pega um arquivo da fila e o "baixa".
    """
    print(f"[{nome_thread}] iniciado e pronto para trabalhar.")
    
    while True:
        # Adquire o lock antes de acessar a lista
        lock_da_fila.acquire()

        # Verifica se ainda há arquivos na fila
        if not fila_de_downloads:
            # Se não houver mais arquivos, libera o lock e encerra a thread
            lock_da_fila.release()
            break
        
        # Pega o próximo arquivo da lista (operação crítica)
        arquivo = fila_de_downloads.pop(0)
        
        # Libera o lock para que outras threads possam acessar a lista
        lock_da_fila.release()

        print(f"[{nome_thread}] começou a baixar: {arquivo}")
        
        # Simula o tempo de download
        tempo_download = random.uniform(1, 3)
        time.sleep(tempo_download)
        
        print(f"[{nome_thread}] terminou de baixar: {arquivo} em {tempo_download:.2f}s")

    print(f"[{nome_thread}] finalizado, não há mais arquivos.")


# --- Demonstração ---
if __name__ == "__main__":
    print("Iniciando sistema de download concorrente com 3 downloaders.\n")
    
    threads = []
    # Criando e iniciando 3 threads (downloaders)
    for i in range(3):
        nome = f"Downloader-{i+1}"
        thread = threading.Thread(target=downloader, args=(nome,))
        threads.append(thread)
        thread.start() # Inicia a execução da thread

    # O programa principal espera todas as threads terminarem
    for thread in threads:
        thread.join()

    print("\nTodos os arquivos foram baixados. Sistema encerrado.")
