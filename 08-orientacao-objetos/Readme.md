# Desafio 08: Programação Orientada a Objetos

Este diretório contém a resolução do oitavo desafio, que consiste em modelar e implementar uma hierarquia simples de classes. O objetivo é demonstrar na prática os conceitos fundamentais da Programação Orientada a Objetos (POO), como herança, encapsulamento e polimorfismo.

O projeto foi desenvolvido em Python e utiliza um sistema de veículos como domínio de aplicação, seguindo a estrutura solicitada.

---

### Domínio Escolhido: Sistema de Veículos

O domínio escolhido foi o de veículos. A ideia é ter uma classe base genérica, `Veiculo`, e classes mais específicas que herdam suas características, como `Carro` e `Bicicleta`, cada uma com comportamentos próprios.

A hierarquia de classes foi estruturada da seguinte forma:
<img width="1010" height="588" alt="image" src="https://github.com/user-attachments/assets/f43c680d-471d-4a00-a8f9-5676adcffdd6" />

### Modelagem da Hierarquia de Classes

#### 1. Classe Abstrata: `Veiculo`
Esta classe representa as características comuns a qualquer veículo.
* **Atributos:**
    * `marca` (String): A fabricante do veículo.
    * `modelo` (String): O modelo do veículo.
    * `velocidade` (int): Atributo protegido para guardar a velocidade atual.
* **Método Abstrato:**
    * `acelerar()`: Um método sem implementação, forçando as subclasses a definirem seu próprio comportamento de aceleração.

#### 2. Subclasse: `Carro`
A classe `Carro` herda de `Veiculo` e adiciona comportamentos e atributos específicos de um carro.
* **Herança:** Herda `marca` e `modelo` de `Veiculo`.
* **Atributo Específico:**
    * `placa` (String): A placa de identificação do carro.
* **Métodos Específicos:**
    * `ligar()`: Altera o estado do carro para ligado.
    * `desligar()`: Altera o estado do carro para desligado.
* **Método Sobrescrito (Polimorfismo):**
    * `acelerar()`: Implementa a lógica de aceleração do carro, que só funciona se ele estiver ligado.

#### 3. Subclasse: `Bicicleta`
A classe `Bicicleta` também herda de `Veiculo`.
* **Herança:** Herda `marca` e `modelo` de `Veiculo`.
* **Método Sobrescrito (Polimorfismo):**
    * `acelerar()`: Implementa sua própria forma de aceleração.

---

### Código de Exemplo (Java)

Todo o código pode ser salvo em um único arquivo chamado `SistemaVeiculos.java` para facilitar a compilação e execução.

```java
import java.util.ArrayList;
import java.util.List;

// 1. CLASSE ABSTRATA BASE
// Não pode ser instanciada diretamente. Serve como um contrato.
abstract class Veiculo {
    protected String marca;
    protected String modelo;
    protected int velocidade;

    public Veiculo(String marca, String modelo) {
        this.marca = marca;
        this.modelo = modelo;
        this.velocidade = 0;
    }

    // Método abstrato: deve ser implementado por todas as subclasses.
    public abstract void acelerar();
}

// 2. SUBCLASSE CARRO
class Carro extends Veiculo {
    private String placa;
    private boolean ligado;

    public Carro(String marca, String modelo, String placa) {
        // Chama o construtor da classe pai (Veiculo)
        super(marca, modelo);
        this.placa = placa;
        this.ligado = false;
    }

    public void ligar() {
        System.out.println("O carro " + this.modelo + " foi ligado.");
        this.ligado = true;
    }

    public void desligar() {
        System.out.println("O carro " + this.modelo + " foi desligado.");
        this.ligado = false;
    }

    // Polimorfismo: implementação obrigatória do método abstrato
    @Override
    public void acelerar() {
        if (this.ligado) {
            System.out.println("O carro " + this.modelo + " está acelerando...");
            this.velocidade += 10;
            System.out.println("Velocidade atual: " + this.velocidade + " km/h");
        } else {
            System.out.println("Não é possível acelerar. O carro " + this.modelo + " está desligado.");
        }
    }
}

// 3. SUBCLASSE BICICLETA
class Bicicleta extends Veiculo {
    public Bicicleta(String marca, String modelo) {
        super(marca, modelo);
    }

    // Polimorfismo: outra implementação do método abstrato
    @Override
    public void acelerar() {
        System.out.println("Pedalando a bicicleta " + this.modelo + " para acelerar...");
        this.velocidade += 2;
        System.out.println("Velocidade atual: " + this.velocidade + " km/h");
    }
}


// 4. CLASSE PRINCIPAL PARA DEMONSTRAÇÃO
public class SistemaVeiculos {
    public static void main(String[] args) {
        System.out.println("--- Criando Veículos ---");
        Carro meuCarro = new Carro("Volkswagen", "Gol", "ABC-1234");
        Bicicleta minhaBike = new Bicicleta("Caloi", "10");
        System.out.println("Veículos criados.\n");

        System.out.println("--- Testando o Carro ---");
        meuCarro.acelerar(); // Tentativa de acelerar com o carro desligado
        meuCarro.ligar();    // Ligando o carro
        meuCarro.acelerar(); // Acelerando com o carro ligado
        System.out.println();

        System.out.println("--- Testando a Bicicleta ---");
        minhaBike.acelerar();
        System.out.println();

        System.out.println("--- Demonstrando Polimorfismo ---");
        // Criando uma lista de Veiculos
        List<Veiculo> listaDeVeiculos = new ArrayList<>();
        listaDeVeiculos.add(meuCarro);
        listaDeVeiculos.add(minhaBike);

        // O loop chama o método 'acelerar()', e a JVM executa a versão
        // correta do método para cada objeto (a do Carro ou a da Bicicleta).
        for (Veiculo veiculo : listaDeVeiculos) {
            veiculo.acelerar();
            System.out.println("--------------------");
        }
    }
}
