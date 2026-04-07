# CPU de 8-bits com Arquitetura SAP Simplificada

Este repositório contém o projeto completo de uma **CPU (Unidade Central de Processamento) de 8 bits**, desenvolvida como atividade ponderada para simulação de circuitos digitais. O projeto foi implementado utilizando o software de simulação [Digital (hneemann)](https://github.com/hneemann/Digital).

## Resumo do Projeto
O presente projeto consiste no desenvolvimento de um processador simplificado, inspirado nos preceitos fundamentais da **Arquitetura SAP (Simple-As-Possible)**. A CPU é composta por uma Unidade Lógica e Aritmética (ALU) totalmente funcional, Memória (para armazenar instruções e operandos) e um Circuito de Controle capaz de gerenciar ativamente os ciclos de busca (Fetch) e execução. O sistema se destaca por realizar o endereçamento sequencial em forma de pilha (onde o operando é buscado no endereço subsequente ao Opcode) e pela construção hierárquica baseada em componentes lógicos básicos, evitando macros genéricas não-relacionais.

## Vídeo de Apresentação

[![Vídeo de Demonstração](https://img.youtube.com/vi/NSjUSaYEE7M/0.jpg)](https://youtu.be/NSjUSaYEE7M)

*Clique na imagem acima para assistir à explicação geral, o caminho dos dados (datapath) e a demonstração do funcionamento da CPU em ciclo de execução.*

## Arquitetura e Componentes

A CPU processa instruções em 8 bits e é dividida nos seguintes blocos principais:

- **Unidade de Controle (UC):** Coração da CPU responsável por orquestrar os ciclos de máquina.
  - **Ciclo de Busca (Fetch):** Lê programaticamente a próxima instrução da memória ROM.
  - **Ciclo de Execução:** Decodifica a instrução (Palavra de Comando / Opcode) e aciona a ALU para realizar o cálculo correspondente com o próximo operando (N).
- **Memória e Endereçamento:** A memória armazena tanto as instruções (Opcodes) quanto os operandos (N). O endereçamento projetado dispensa uso explícito por parte do usuário, sendo operado de forma **sequencial (pilha)** — o próximo operando está sempre localizado no endereço imediatamente após a instrução.
- **ALU (Unidade Lógica e Aritmética):** Módulo matemático rigoroso composto por um Seletor de Operações (Multiplexador) e Circuitos complexos de processamento em cascata.
- **Registradores Internos:**
  - **AC (Acumulador):** Registrador principal de 8 bits. Atua rotineiramente como o primeiro operando da ALU e armazena os resultados primários (como o LSB das multiplicações e o Resto das divisões).
  - **MQ (Multiplier/Quotient):** Registrador auxiliar de 8 bits utilizado unicamente para armazenar transbordos extensos (o MSB das multiplicações e o Quociente das divisões).

## Tabela de Operações (ALU)

A CPU consegue decodificar e processar palavras de comando para executar as seguintes operações matemáticas e lógicas, acionando o Acumulador e a Entrada N (vinda sequencialmente da Memória):

| Opcode | Operação | Matemática | Saídas e Registradores (Destino) |
| :---: | :--- | :--- | :--- |
| **000** | **SOMA** | AC + N | AC recebe o resultado (8 bits) |
| **001** | **SUBTR** | AC - N | AC recebe o resultado lícito via Comp. de 2 |
| **010** | **MULT** | AC * N | LSB armazenado no AC; MSB armazenado no MQ |
| **011** | **DIV** | AC / N | Resto permanece no AC; Quociente armazenado no MQ |
| **100** | **NAND** | AC NAND N | AC recebe saída lógica bit a bit |
| **101** | **XOR** | AC XOR N | AC recebe saída lógica bit a bit |
| **110** | **SLL** | AC << N | AC recebe resultado (Shift Left Logical) |
| **111** | **SRL** | AC >> N | AC recebe resultado (Shift Right Logical) |

## Processo de Desenvolvimento

O circuito da CPU coroa o entendimento sobre o Datapath e ciclos de relógio:
* **Decodificação e Ciclos:** O desenvolvimento do **Circuito de Controle** introduziu a complexidade de sincronizar o *Program Counter (PC)* para realizar o Fetch correto. Garantir que a ROM forneça o Opcode em um ciclo, e no ciclo seguinte avance sequencialmente para fornecer o dado `N` correto para a entrada da ALU sem corromper as flag operations foi um grande avanço no design.
* **Divisão por Restauração (ALU):** Manteve-se o êxito no módulo da ALU em resolver multiplicações e divisões de forma nativa. A criação da lógica de restauração da célula de divisão, utilizando o sinal de `Cout` do subtrator diretamente como o seletor do Multiplexador para "restaurar" o valor positivo, reitera a precisão de um projeto bem resolvido na base de silício virtual.
* **Gerenciamento do Split de Barramento:** O desafio final arquitetural foi sincronizar o write enable dos registradores **AC** e **MQ** de acordo com a operação sendo executada durante o sinal de clock, isolando o barramento estendido (16 bits) e preservando MSBs e LSBs de forma idônea perante transbordamentos de multiplicação.

## Como Executar o Projeto

1. **Pré-requisitos:** 
   - Baixar o simulador lógico [Digital (hneemann)](https://github.com/hneemann/Digital/releases) e possuir o Java instalado.
2. **Clonar o Repositório:**
   ```bash
   git clone https://github.com/PietroAlkmin/CPU_8_Bits.git
   ```
3. **Execução:**
   - Abra o simulador `Digital`.
   - Vá em `File > Open` e navegue até a pasta `ArquivosDigital`. Abra o arquivo principal contendo o circuito mestre da **CPU**.
   - (Atenção: Extraia e mantenha toda a hierarquia de arquivos (`ALU.dig`, `Divisor.dig`, e etc) dentro da mesma pasta para que os submódulos da CPU compilem com sucesso).
   - Manipule o gerador de Clock para avançar os ciclos de Busca (Fetch) e Execução. Observe no split visual como a memória despeja dados alternando entre comando lógico na UC e operando material na ALU.

## Identificação e Autoria

- **Nome:** Pietro Alkmin
- **Instituição:** Inteli
- **Projeto:** CPU de 8 bits (Arquitetura SAP Simplificada)
- **Repositório:** [PietroAlkmin/CPU_8_Bits](https://github.com/PietroAlkmin/CPU_8_Bits)
