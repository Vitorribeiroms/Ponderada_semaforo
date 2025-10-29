# 🚦 Projeto: Semáforo com Arduino – Departamento de Engenharia de Trânsito

##  Autor
**Vitor Ribeiro**

---

##  Objetivo

Desenvolver um sistema de **semáforo funcional** utilizando **Arduino, LEDs e resistores**, capaz de controlar o fluxo de veículos de forma segura e automatizada.  
O sistema deve seguir a lógica real de tempo e sequência das luzes: **vermelho → amarelo → verde → vermelho**.

---

##  Contexto do Projeto

Durante o estágio no **Departamento de Engenharia de Trânsito**, fui designado para controlar o fluxo de uma via movimentada no bairro **Butantã**.  
O desafio consistia em **montar e programar um semáforo** que garantisse a segurança de pedestres e veículos, aplicando conhecimentos de eletrônica e programação para criar um sistema essencial de controle de tráfego.

---

##  Especificações do Funcionamento

O semáforo foi programado para seguir o **ciclo real** utilizado em vias urbanas:

| Cor | Duração | Função |
|------|----------|--------|
| 🔴 Vermelho | 6 segundos | Interrompe o tráfego |
| 🟡 Amarelo | 2 segundos | Alerta de transição |
| 🟢 Verde | 4 segundos |  Libera o tráfego  |

O ciclo se repete continuamente: **vermelho → amarelo → verde → vermelho.**

---

##  Parte 1: Montagem Física

###  Componentes Utilizados

| Componente | Quantidade | Especificação / Função |
|-------------|-------------|-------------------------|
| Arduino UNO | 1 | Microcontrolador que controla o semáforo |
| Protoboard | 1 | Base para montagem dos componentes |
| LED Verde | 1 | Representa o sinal de "Siga" |
| LED Amarelo | 1 | Representa o sinal de "Atenção" |
| LED Vermelho | 1 | Representa o sinal de "Pare" |
| Resistores | 3 | 220 Ω – Protegem os LEDs contra sobrecorrente |
| Jumpers | 10 | Realizam as conexões entre o Arduino e a protoboard |
| Cabo USB | 1 | Alimentação e comunicação com o Arduino |

---

###  Ligações na Protoboard

| LED | Cor | Pino Digital | Resistor | Ligação |
|------|------|---------------|-----------|----------|
| Vermelho | 🔴 | 5 | 220 Ω | GND → Resistor → LED → Pino 7 |
| Amarelo | 🟡 | 6 | 220 Ω | GND → Resistor → LED → Pino 6 |
| Verde | 🟢 | 7 | 220 Ω | GND → Resistor → LED → Pino 5 |

 

<div align="center">
  <div align="center" style="margin-bottom: 1em;">
<p style="margin-bottom: 0.3em; font-style: italic;"><strong>Projeto fisico </strong> </p>
<img src="assets/Projeto.png"><br />
</p>
</div>

<sub>Fonte: autoria própria Vitor Ribeiro, 2025</sub>
</div>

---- 
### Video explicativo localizado na pasta Assests → Vídeo
---

###  Justificativas da Montagem

- Os **resistores** de 220 Ω foram utilizados para limitar a corrente e evitar a queima dos LEDs.  
- Os LEDs foram dispostos de forma **vertical (vermelho em cima, amarelo no meio, verde embaixo)**, simulando um semáforo real.  
- A montagem foi organizada para garantir **clareza visual** e **facilidade de teste**.  
- Cada cor tem sua **porta digital independente**, controlada por um **objeto da classe `Semaforo`** no código.

---

##  Parte 2: Programação e Lógica

O código abaixo foi desenvolvido em **C++ para Arduino**, aplicando conceitos de **Programação Orientada a Objetos (POO)** e **ponteiros**, tornando o projeto mais modular e organizado.


---

###  Código Fonte

```cpp
// ----- SEMÁFORO REAL COM POO E PONTEIROS -----
// Autor: Vitor Ribeiro
// Descrição: Simula o funcionamento de um semáforo real
// Ciclo: Verde (4s) → Amarelo (2s) → Vermelho (6s) → Verde

class Semaforo {
  private:
    int pinoVermelho;
    int pinoAmarelo;
    int pinoVerde;

  public:
    // ----- Construtor -----
    Semaforo(int vermelho, int amarelo, int verde) {
      pinoVermelho = vermelho;
      pinoAmarelo  = amarelo;
      pinoVerde    = verde;

      pinMode(pinoVermelho, OUTPUT);
      pinMode(pinoAmarelo, OUTPUT);
      pinMode(pinoVerde, OUTPUT);
    }

    // ----- Método auxiliar -----
    void acenderSomente(int pino) {
      digitalWrite(pinoVermelho, LOW);
      digitalWrite(pinoAmarelo, LOW);
      digitalWrite(pinoVerde, LOW);
      digitalWrite(pino, HIGH);
    }

    // ----- Fases do semáforo -----
    void verde() {
      acenderSomente(pinoVerde);
      delay(4000); // 4 segundos
    }

    void amarelo() {
      acenderSomente(pinoAmarelo);
      delay(2000); // 2 segundos
    }

    void vermelho() {
      acenderSomente(pinoVermelho);
      delay(6000); // 6 segundos
    }

    // ----- Ciclo completo -----
    void ciclo() {
      verde();      // Trânsito liberado
      amarelo();    // Atenção
      vermelho();   // Parada total
    }
};

// ----- Ponteiro para o objeto Semáforo -----
Semaforo* semaforo;

void setup() {
  // Cria o objeto semáforo com os pinos corretos
  semaforo = new Semaforo(7, 6, 5);
}

void loop() {
  // Executa o ciclo continuamente
  semaforo->ciclo();
}
