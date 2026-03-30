# 💻 ULA Digital de 8-bits (ALU)

Este repositório contém o projeto de uma **Unidade Lógica e Aritmética (ALU) de 8 bits**, desenvolvida como atividade ponderada para simulação de circuitos digitais. O projeto foi implementado utilizando o software de simulação [Digital (hneemann)](https://github.com/hneemann/Digital).

## 📄 Resumo do Projeto
O presente projeto consiste no desenvolvimento do núcleo de processamento de um computador, inspirado nos preceitos fundamentais da **Arquitetura de Von Neumann**. Tratando-se de uma ULA totalmente funcional, a implementação não recorre primariamente a macros pré-fabricadas abstratas (blocos de alto nível), mas constrói a sua base relacional utilizando uma hierarquia de portas lógicas e somadores de 1 bit. O projeto demonstra um avanço profundo no entendimento de operações de máquina por hardware, resolvendo através de componentes físicos cálculos complexos como a multiplicação e a divisão não-restauradora de forma nativa. 

## 🎥 Vídeo de Apresentação

[![Vídeo de Demonstração](https://img.youtube.com/vi/LbK_SP19gcc/0.jpg)](https://youtu.be/LbK_SP19gcc)

*Clique na imagem acima para assistir à explicação e demonstração do funcionamento da ULA.*

**Link alternativo (Google Drive):** [Clique aqui para acessar o vídeo reserva](https://drive.google.com/file/d/1SOdocvwvXYWiwySvpyd6bPQsugavxBam/view?usp=sharing)

---

## ⚙️ Arquitetura e Componentes

A ULA foi projetada para processar dados de 8 bits e é composta pelos seguintes componentes principais:

- **Circuitos de Operações:** Módulos lógicos isolados responsáveis por executar cada cálculo aritmético e lógico de forma hierárquica.
- **Seletor de Operações (Multiplexador):** Circuito que recebe um código de instrução (Opcode) de 3 bits e direciona o resultado da operação correta para a saída.
- **Registradores Internos:**
  - **AC (Acumulador):** Registrador principal de 8 bits, atua como um dos operandos e armazena os resultados primários (como o LSB da multiplicação e o Resto da divisão).
  - **MQ (Multiplier/Quotient):** Registrador auxiliar de 8 bits utilizado para armazenar operandos extensos (como o MSB da multiplicação e o Quociente da divisão).

---

## 🧮 Tabela de Operações

A Unidade é capaz de realizar as seguintes operações matemáticas e lógicas, selecionadas a partir de um seletor (Opcode) de 3 bits:

| Opcode (Binário) | Operação | Descrição Técnica |
| :---: | :--- | :--- |
| **000** | **SOMA** | Adição de 8 bits (A + N) |
| **001** | **SUBTR** | Subtração via Complemento de 2 (A - N) |
| **010** | **MULT** | Multiplicação 8x8 (Resultado 16 bits: LSB no AC, MSB no MQ) |
| **011** | **DIV** | Divisão por Restauração (Quociente no MQ, Resto no AC) |
| **100** | **NAND** | Operação Lógica Universal (Bit a Bit) |
| **101** | **XOR** | Operação Lógica de Ou Exclusivo (Bit a Bit) |
| **110** | **SLL** | Shift Left Logical (Deslocamento de bits para esquerda) |
| **111** | **SRL** | Shift Right Logical (Deslocamento de bits para direita) |

---

## 🛠️ Processo de Desenvolvimento

O circuito foi desenvolvido valorizando a construção manual e a hierarquia lógica:
* **Implementação Modular:** A lógica não se limitou a blocos prontos de alto nível do software. A soma e a subtração, por exemplo, foram elaboradas utilizando somadores de completudade (Full-Adders) de 1 bit ligados em cascata.
* **O Maior Desafio (Divisão por Restauração):** A operação de divisão foi um dos pontos mais complexos, exigindo a criação de uma sub-célula dedicada (`Celula_Div.dig`). Foi projetada a lógica de restauração, onde o sinal de `Cout` do subtrator atua ativamente como o seletor de um Multiplexador. Caso a subtração resulte em um valor negativo, o MUX entra em ação e "restaura" o valor original, garantindo a exatidão do cálculo de resto e quociente.
* **Gerenciamento de Barramento:** Outro desafio de arquitetura foi o gerenciamento rígido do barramento de 16 bits resultante da multiplicação, fazendo o split correto para garantir que o **Acumulador (AC)** ficasse responsável pela parte baixa (LSB) e o **MQ** acomodasse a parte alta (MSB), preservando o valor e evitando qualquer overflow indesejado.

---

## 🚀 Como Executar o Projeto

1. **Pré-requisitos:** 
   - Ter o Java instalado na máquina.
   - Baixar o simulador lógico [Digital (hneemann)](https://github.com/hneemann/Digital/releases).
2. **Clonar o Repositório:**
   ```bash
   git clone https://github.com/PietroAlkmin/ULA-Digital.git
   ```
3. **Execução:**
   - Abra o simulador `Digital`.
   - Vá em `File > Open` e abra o arquivo principal **`ALU_8bit_Main.dig`** (certifique-se de que os arquivos auxiliares como `Celula_Div.dig` e `Subtrator_8bit.dig` estejam na mesma pasta para evitar erros de dependência).
   - Inicie a simulação (botão play), configure os valores das entradas de dados e altere os 3 pinos de Opcode para testar todas as funcionalidades descritas na tabela.

---

## 👤 Identificação e Autoria

- **Nome:** Pietro Alkmin
- **Instituição:** Inteli
- **Projeto:** Unidade Lógica e Aritmética (ULA) de 8 bits
- **Repositório:** [PietroAlkmin/ULA-Digital](https://github.com/PietroAlkmin/ULA-Digital)
