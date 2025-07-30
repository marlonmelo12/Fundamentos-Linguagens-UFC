# Desafio 05: Estruturas de Controle

Este diretório contém a resolução do quinto desafio, que consiste em desenvolver um programa simples para demonstrar o uso de estruturas de controle (seleção, repetição e controle de fluxo) dentro de um contexto original e criativo.

---

### Contexto Original: Simulador de Bateria de Drone

O programa a seguir simula o comportamento de um drone autônomo realizando uma lista de tarefas. O drone possui um nível de bateria que se esgota a cada minuto. O script deve usar estruturas de controle para gerenciar o voo, executar tarefas e tomar decisões com base no nível da bateria.

As estruturas serão utilizadas da seguinte forma:

* **Estrutura de Repetição (`while`):** Manterá o drone em operação enquanto ele tiver bateria suficiente para voar.
* **Estrutura de Seleção (`if`/`elif`/`else`):** Decidirá qual ação o drone deve tomar a cada minuto, com base na sua carga de bateria.
* **Estruturas de Controle de Fluxo (`break` e `continue`):**
    * `continue`: Será usada para pular um ciclo de verificação se uma tarefa específica não puder ser executada.
    * `break`: Será usada para encerrar o loop de voo imediatamente se uma condição crítica for atingida (como o acionamento do pouso de emergência).

---

### Código de Exemplo (Python)

O código abaixo implementa a simulação descrita. Ele está totalmente comentado para explicar o uso de cada estrutura de controle no contexto do drone.

**Arquivo `simulador_drone.py`:**

```python
import time

# --- Configurações Iniciais do Drone ---
nivel_bateria = 100  # Bateria começa em 100%
bateria_critica = 15 # Nível crítico para retorno
tarefas_pendentes = ["Verificar telhado", "Fotografar jardim", "Inspecionar painel solar", "Entregar pacote"]

print("--- Iniciando Simulação de Voo do Drone ---")
print(f"Bateria inicial: {nivel_bateria}%. Tarefas: {tarefas_pendentes}")
print("-" * 40)

# ESTRUTURA DE REPETIÇÃO: o loop 'while' mantém o drone no ar
# enquanto a bateria estiver acima do nível crítico.
while nivel_bateria > bateria_critica:
    
    nivel_bateria -= 5  # Drone consome 5% de bateria por minuto de voo
    print(f"Bateria atual: {nivel_bateria}%")

    # ESTRUTURA DE SELEÇÃO: decide a ação com base na bateria
    if nivel_bateria > 70:
        # Pega a primeira tarefa da lista
        tarefa_atual = tarefas_pendentes.pop(0)
        
        # ESTRUTURA DE CONTROLE DE FLUXO ('continue'): se a tarefa for a entrega de pacote
        # e a bateria não estiver cheia, o drone adia a tarefa e continua para o próximo ciclo.
        if tarefa_atual == "Entregar pacote" and nivel_bateria < 90:
            print("AVISO: Bateria não está ideal para entrega. Adicionando tarefa ao final da fila.")
            tarefas_pendentes.append(tarefa_atual)
            time.sleep(1) # Simula tempo passando
            continue # Pula o resto do loop e vai para a próxima iteração
            
        print(f"EXECUTANDO TAREFA: {tarefa_atual}")

    elif nivel_bateria > 30:
        print("MODO ECONÔMICO: Apenas mantendo voo estável.")
    
    else:
        print("ALERTA: Bateria baixa! Retornando à base.")
        # ESTRUTURA DE CONTROLE DE FLUXO ('break'): encerra o loop imediatamente
        # para iniciar o procedimento de pouso.
        break
    
    # Se não houver mais tarefas, o drone pode retornar
    if not tarefas_pendentes:
        print("Todas as tarefas foram concluídas com sucesso!")
        break

    time.sleep(1) # Pausa de 1 segundo para simular o tempo passando


# --- Fim da Simulação ---
print("-" * 40)
print("Simulação de voo encerrada.")
if nivel_bateria <= bateria_critica:
    print(f"Status Final: Pouso de emergência devido à bateria crítica ({nivel_bateria}%).")
else:
    print(f"Status Final: Drone pousou com segurança. Bateria restante: {nivel_bateria}%.")

if tarefas_pendentes:
    print(f"Tarefas não concluídas: {tarefas_pendentes}")
