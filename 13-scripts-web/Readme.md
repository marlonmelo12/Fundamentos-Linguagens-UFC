# Desafio 13: Linguagens para Scripts e Web

[cite_start]Este diretório contém a resolução do décimo terceiro desafio, que consiste na criação de um script para automação.

O objetivo é demonstrar o poder das linguagens de script para resolver tarefas do dia a dia. [cite_start]Para isso, foi desenvolvido um script em Python que organiza arquivos em uma pasta, utilizando os módulos `os` e `shutil`, conforme sugerido pelo guia do trabalho.

---

### Contexto do Script: Organizador Automático de Arquivos

#### O Problema
A pasta "Downloads" da maioria das pessoas costuma ser um lugar caótico, com dezenas ou centenas de arquivos de todos os tipos misturados: PDFs, imagens, instaladores, documentos, etc. Encontrar algo pode ser uma tarefa difícil e demorada.

#### A Solução
O script `organizador.py` resolve esse problema. Ele é uma ferramenta de automação que varre uma pasta especificada (a "pasta de origem") e move cada arquivo para uma subpasta correspondente ao seu tipo. Por exemplo:
* Todos os arquivos `.pdf` são movidos para uma pasta `Documentos PDF`.
* Arquivos `.jpg`, `.png` e `.gif` são movidos para uma pasta `Imagens`.
* Arquivos `.zip` e `.rar` são movidos para `Arquivos Compactados`.

Isso é feito de forma automática, economizando o tempo de ter que organizar tudo manualmente.

### Estrutura de Pastas (Antes e Depois)

**Antes de executar o script:**

minha_bagunca/

├── relatorio_final.pdf

├── foto_ferias.jpg

├── manual_produto.pdf

├── logo_empresa.png

└── projeto.zip

**Depois de executar o script:**

minha_bagunca/

├── Documentos PDF/

│   ├── relatorio_final.pdf

│   └── manual_produto.pdf

├── Imagens/

│   ├── foto_ferias.jpg

│   └── logo_empresa.png

└── Arquivos Compactados/

└── projeto.zip

---

### Código do Script de Automação (`organizador.py`)

O código abaixo é totalmente comentado para explicar cada passo do processo.

```python
import os
import shutil

# 1. DEFINIR O CAMINHO DA PASTA A SER ORGANIZADA
# Para este exemplo, vamos criar e usar uma pasta chamada 'minha_bagunca'
PASTA_ORIGEM = "minha_bagunca"

# 2. DEFINIR O MAPEAMENTO DE EXTENSÕES PARA PASTAS
# As chaves são as extensões dos arquivos, e os valores são os nomes das pastas de destino.
MAPEAMENTO_PASTAS = {
    ".pdf": "Documentos PDF",
    ".docx": "Documentos Word",
    ".txt": "Textos",
    ".jpg": "Imagens",
    ".jpeg": "Imagens",
    ".png": "Imagens",
    ".gif": "Imagens",
    ".zip": "Arquivos Compactados",
    ".rar": "Arquivos Compactados",
    ".exe": "Instaladores",
    ".mp3": "Musicas",
    ".mp4": "Videos",
}

def organizar_pasta(caminho_pasta):
    """
    Função principal que organiza os arquivos na pasta especificada.
    """
    print(f"--- Iniciando organização da pasta: {caminho_pasta} ---")

    # 3. LISTAR TODOS OS ITENS NA PASTA DE ORIGEM
    try:
        lista_de_arquivos = os.listdir(caminho_pasta)
    except FileNotFoundError:
        print(f"ERRO: A pasta '{caminho_pasta}' não foi encontrada.")
        return

    # 4. ITERAR SOBRE CADA ITEM
    for nome_arquivo in lista_de_arquivos:
        caminho_completo_arquivo = os.path.join(caminho_pasta, nome_arquivo)

        # Ignorar se for uma pasta
        if os.path.isdir(caminho_completo_arquivo):
            continue

        # 5. OBTER A EXTENSÃO DO ARQUIVO
        _, extensao = os.path.splitext(nome_arquivo)
        extensao = extensao.lower() # Normalizar para minúsculas

        # 6. ENCONTRAR A PASTA DE DESTINO CORRESPONDENTE
        pasta_destino_nome = MAPEAMENTO_PASTAS.get(extensao, "Outros")
        caminho_pasta_destino = os.path.join(caminho_pasta, pasta_destino_nome)

        # 7. CRIAR A PASTA DE DESTINO SE ELA NÃO EXISTIR
        if not os.path.exists(caminho_pasta_destino):
            os.makedirs(caminho_pasta_destino)
            print(f"Pasta criada: {pasta_destino_nome}")

        # 8. MOVER O ARQUIVO PARA A PASTA DE DESTINO
        try:
            shutil.move(caminho_completo_arquivo, caminho_pasta_destino)
            print(f"Movendo '{nome_arquivo}' para '{pasta_destino_nome}'")
        except Exception as e:
            print(f"ERRO ao mover '{nome_arquivo}': {e}")
            
    print("--- Organização finalizada! ---")

# Função para criar um ambiente de teste
def criar_ambiente_teste():
    if not os.path.exists(PASTA_ORIGEM):
        os.makedirs(PASTA_ORIGEM)
    
    arquivos_teste = [
        "relatorio.pdf", "foto.jpg", "notas.txt", "setup.exe", "arquivo.zip"
    ]
    for arq in arquivos_teste:
        open(os.path.join(PASTA_ORIGEM, arq), 'a').close()
    print("Ambiente de teste criado com arquivos na pasta 'minha_bagunca'.")


if __name__ == "__main__":
    criar_ambiente_teste()
    input("\nPressione Enter para iniciar a organização...") # Pausa para o usuário ver
    organizar_pasta(PASTA_ORIGEM)
```
