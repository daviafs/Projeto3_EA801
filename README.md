# Projeto3_EA801

Projeto para a disciplina EA801- Laboratório Projetos Sistemas Embarcados (FEEC-UNICAMP)

Desenvolvido por: Davi A. F. de Souza & Gabriel M. de Andrade

Docente responsável: Antônio A. F. Quevedo

Data: 11 de Julho, 2025

## Descrição:
Título: SISTEMA DE CONTROLE PWM PARA ILUMINAÇÃO LED E CLIMATIZAÇÃO EM AGRICULTURA INDOOR

Linguagem: C

Plataforma: SMT32CubeIDE

MCU: STM32F411 BlackPill (Através da placa BitDogLab)
	
## Objetivo do Projeto

O projeto consiste em desenvolver um software de controle via PWM para aplicações de iluminação LED e climatização baseado em ventoinhas, garantindo o controle de temperatura e umidade relativa em estufas, por exemplo. Além disso, quer-se controlar a intensidade da iluminação através de um periférico conectado, como um potenciômetro, para um ajuste fino caso seja necessário. Outro objetivo é controlar o sistema remotamente via Wi-Fi, utilizando um circuito dedicado para integração no sistema e comunicação com o celular via rede local usando o protocolo MQTT.

Além disso, há também o desenvolvimento de um hardware multifuncional, para acolher a placa de trabalho e fornecer uma alimentação independente do módulo ST-Link. Ela também deve ser modular para abrigar outros microcontroladores comerciais de forma simples, uma vez que a alimentação deles é padronizada em 3.3V e/ou 5V. Os periféricos também estarão todos ligados à essa nova placa de forma modular em vez de serem ligados à placa BitDogLab diretamente. A ideia é, portanto, desenvolver uma estratégia de controle centralizado, através de uma unidade de controle padronizada, o qual será interligado à placa BitDogLab e seus periféricos. Através do microcontrolador STM32F411 BlackPill e programação em C no software STM32CubeIDE , garantir as seguintes funcionalidades:

1. Exibir os dados no display;
  
2. Mudar a cor e intensidade dos LEDs RGB na matriz de LED;
   
3. Realizar o controle de velocidade de ventiladores;
   
4. Processa entradas dos botões, movimento joystick, e deslize do potenciômetro;
  
5. Coletar dados do sensor de temperatura e umidade via interface I2C;
  
6. Dentro de um loop, o algoritmo garante o monitoramento dos parâmetros ambientais, controle da velocidade de cada ventilado a partir dos valores coletados pelo sensor, e alteração do sistema de iluminação a partir da interação do operador com a placa ou pela mudança do potenciômetro.


## Funcionamento

Para alcançar os objetivos propostos, foi desenvolvido um programa em C, a nível de registradores, via software STM32CubeIDE. O upload do código na placa necessitou de um dispositivo ST-Link para conexão via porta USB. A lógica do algoritmo a ser desenvolvido contou, de maneira geral, com as seguintes funções:

Controle PWM da ventoinhas:
- Envio e leitura de um sinal ADC de 8 bits para o módulo PWM
- Seta o Duty Cycle para cada ventoinha
- Atualizar as ventoinhas: offset e regulação pelo joystick

Comunicação I2C com sensor:
- Leitura de temperatura
- Leitura de umidade relativa
- Controle das luzes da matriz LED
- Checar o estado dos botões (leitura, debounce, checar memória)
- Mudar a intensidade e a composição da luz caso um botão seja acionado
- Checar o sinal do potenciômetro (se foi deslizado ou não)
- Aumentar ou diminuir a intensidade proporcionalmente à mudança do potenciômetro, caso tenha mudado

Atualização do sistema:
- Atualiza o que está sendo apresentado no display OLED
- Checar o estado dos botões (leitura, debounce, checar memória)
- Atualiza valor do offset de temperatura e umidade relativa

Controle remoto (não implementado no final):
- Checa se houve envio de um comando via wifi e o executa
- Envia dados de operação (velocidade da ventoinha, temperatura,etc.)

## Mapeamento das portas <img width="960" height="410" alt="Diagrama de blocos (4)" src="https://github.com/user-attachments/assets/81ed69c8-49c9-4990-a160-c39a79644a83" />

## Fluxograma!<img width="966" height="1262" alt="Fluxograma_projeto3" src="https://github.com/user-attachments/assets/fb2d176c-cd97-45f8-921e-71f809b64bc9" />

## Imagens do projeto

<img width="503" height="184" alt="image" src="https://github.com/user-attachments/assets/c0d517d9-c5ad-4d8a-be13-aecb50925424" />

Descrição de imagem: Diagrama a nível de dispositivo, com placa BitDogLab, unidade de controle padronizadas, e periféricos como atuadores, sensor e módulo WIFI


<img width="960" height="540" alt="Diagrama de blocos (3)" src="https://github.com/user-attachments/assets/a5ccad4d-8a63-4689-8a0e-d2b1fe4216a5" />

Descrição da imagem: Detalhamento das interfaces implementadas na unidade de controle padronizada. O sistema conta com uma alimentação de 12V para os módulos PWM, que também passa por um regulador de tensão ajustável para alimentar os demais componentes da placa. O uso de bornes foi considerado para facilitar as conexões com os periféricos do sistema, bem como viabilizar possíveis mudanças nos dispositivos devido a falhas


<img width="960" height="365" alt="Diagrama de blocos (2)" src="https://github.com/user-attachments/assets/8e27f3c6-4c71-4261-bbd1-93e24d27c7ba" />

Descrição da imagem: Hardware padronizado com interface para ESP8266 e BitDogLab.



## Especificações dos periféricos

Sensor de Umidade e Temperatura
- Modelo: SHT20
- Encapsulamento: IP65
- Medição  (UR): 0 a 100 % ± 3%
- Medição (Temp.): -40°C a 125°C ± 0,3°C 
- Tensão: 3 a 5,5V DC;
- Interface de comunicação: I2C;
- Tempo de resposta: 8s;
- Função: Realizar medições temporizadas de temperatura e umidade relativa do ambiente e envio dos dados para a STM32 black pill

Módulo Driver PWM 
- Modelo: D4184;
- Tensão: 5-36 VDC;
- Tensão de PWM: 3,3-20V;
- Frequência do PWM = 0-20 KHZ
- Corrente: 15A;
- Potência: 400W;
- Função: Controle da velocidade das ventoinhas através dos sinais da black pill

Ventoinhas
- Motor Brushless CC
- Tensão de Entrada: 12V
- Corrente: 0,18 A
- Potência: 2,16 W
- Função: Uma ventoinha foi usada para controle de temperatura (ventilação) e outra para controle de umidade (exaustão)

Conversor AC-DC:
- Modelo: ADP-30BW K
- Tensão de Entrada: 100-240 VAC (60Hz)
- Corrente de Entrada: 1 A (Alternada)
- Tensão de Saída: 12 VDC
- Corrente de Saída: 2,5 A (Contínua)
- Função: Alimentação das ventoinhas
