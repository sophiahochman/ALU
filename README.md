# ALU 8 bits — Documentação Detalhada 

Este repositório contém a implementação de uma ALU (Unidade Lógica e Aritmética) de 8 bits.

https://drive.google.com/file/d/1qXpKlVjQ8h-6CAzaRASRqP8-i-O0p1l_/view?usp=drive_link

A ALU é responsável por executar operações aritméticas e lógicas dentro de um sistema computacional, sendo um dos componentes fundamentais de qualquer processador. Este projeto foi desenvolvido utilizando o simulador de circuitos digitais (Digital), com suporte a múltiplas operações controladas por um seletor (SEL) e uso de registradores para armazenamento de resultados.

---

## Operações implementadas

| SEL | Operação       | Entrada       | Saída                        |
|-----|----------------|---------------|------------------------------|
| 000 | Soma           | AC + N        | AC (8 bits)                  |
| 001 | Subtração      | AC - N        | AC (8 bits)                  |
| 010 | Multiplicação  | AC × N        | AC (LSB) e MQ (MSB)          |
| 011 | Divisão        | AC ÷ N        | AC (Resto) e MQ (Quociente)  |
| 100 | Shift lógico   | AC            | AC (8 bits)                  |
| 101 | NAND           | AC NAND N     | AC (8 bits)                  |
| 110 | XOR            | AC XOR N      | AC (8 bits)                  |

---

## Tecnologias utilizadas

- **Digital** — simulador de circuitos digitais  
- Álgebra Booleana  
- Circuitos combinacionais  
- Circuitos sequenciais  
- Arquitetura de Computadores  

---

## Estrutura do projeto

```
📁 ALU/
├── ALU.dig                  
├── somador_1bit.dig         
├── somador_8bits.dig        
├── subtrator_8bits.dig      
├── multiplicador_8bits.dig  
├── divisor_8bits.dig        
├── shift.dig                
├── register_8bits.dig       
├── nand.dig                 
├── xor.dig                  
└── README.md
```

---

## Descrição das operações

### Soma

A operação de soma é baseada em um **Full Adder de 1 bit**, que recebe três entradas (A, B e Cin) e produz duas saídas (S e Cout).

- A saída **S (soma)** é obtida por operações XOR  
- O **carry (Cout)** é gerado quando há pelo menos dois bits iguais a 1  

O somador de 8 bits é construído a partir do encadeamento de 8 Full Adders.

---

### Subtração

A subtração é realizada utilizando o método de **complemento de 2**:

```
A - B = A + (~B) + 1
```

- Os bits de B são invertidos  
- Um carry inicial (Cin = 1) é adicionado  
- O mesmo circuito do somador é reutilizado  

---

### Multiplicação

A multiplicação segue o modelo de **array multiplier**, semelhante à multiplicação manual:

- Cada bit do multiplicador gera um produto parcial com AND  
- Os produtos são somados com deslocamento adequado  
- O resultado final possui 16 bits:
  - 8 bits menos significativos → AC  
  - 8 bits mais significativos → MQ  

---

### Divisão

A divisão foi implementada utilizando o algoritmo de **Restoring Division**:

1. O dividendo é processado bit a bit  
2. A cada passo ocorre um shift à esquerda  
3. O divisor é subtraído do valor parcial  
4. Se o resultado for negativo, o valor é restaurado  

Saídas:
- **Quociente → MQ**  
- **Resto → AC**

---

### Shift lógico

O deslocamento lógico pode ocorrer em duas direções:

- **Shift Left (<<)**: desloca os bits para a esquerda e insere 0 no LSB  
- **Shift Right (>>)**: desloca os bits para a direita e insere 0 no MSB  

A direção é controlada por um sinal de controle.

---

### Operações lógicas

- **XOR**: retorna 1 quando os bits são diferentes  
- **NAND**: inversão do AND entre os bits  

Essas operações são realizadas bit a bit.

---

### ALU completa

Todas as operações são executadas em paralelo, e o resultado final é selecionado por:

- **MUX (Multiplexador) de 8 entradas**  
- Controlado pelo sinal **SEL (3 bits)**  

O resultado passa por registradores antes de ser exibido, garantindo sincronização com o clock.

---

## Como executar

1. Baixe o simulador **Digital**  
2. Abra o arquivo `ALU.dig`  
3. Inicie a simulação  
4. Defina os valores de:
   - **AC**
   - **N**
   - **SEL**  
5. Acione o **CLK** para visualizar o resultado  

---

## Considerações finais

Este projeto demonstra, na prática, a construção de uma ALU completa utilizando conceitos fundamentais de arquitetura de computadores. A implementação modular facilita a compreensão, manutenção e expansão do sistema, permitindo adicionar novas operações ou otimizações futuras.
