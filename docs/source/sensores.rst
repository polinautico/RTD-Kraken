=====
Sensores e Telemetria
=====

O que é Telemetria?
-----
A telemetria é uma tecnologia que permite a medição e comunicação de informações e dados de interesse do operador, podendo monitorar e controlar determinado produto.
A telemetria é realizada, de modo geral, através de comunicações sem fio, como sinais de rádio, satélite, fibra óptica, entre outras tecnologias. Essas informações são enviadas e monitoradas em tempo real através de uma central, e esses dados são armazenados em um banco de dados. A telemetria pode realizar não só a leitura e monitoramento, mas também realizar comandos remotos pelos operadores. Essa tecnologia oferece eficiência, segurança na operação e pode ajudar a evitar penalidades, como a perda de água.
Como exemplos de aplicação, essa tecnologia está presente na agricultura, em sistemas de informação, no sistema metroviário e até mesmo em corridas de fórmula 1, que nesse caso, ela é usada para monitorar em tempo real a situação dos componentes dos carros.

.. image:: imagens/telemex.png
  :align: center
  :width: 300
  
No caso do nosso rebocador, utilizaremos a telemetria para monitorar o que está havendo com a parte elétrica, afim de entender o desempenho da embarcação.


A Telemetria do Kraken e dispositivos
-----
Para o Kraken, utilizaremos a telemetria para monitorar a tensão e a temperatura dos componentes elétricos utilizando:

* Sensor de Tensão DC 0 a 25V
* Sensor de Temperatura MAX6675 Termopar Tipo KMAX6675 Termopar Tipo K
* ESP LoRa 32 V2

Sensor de Tensão DC 0 a 25V
-----
Como o nome diz, é um sensor que mede a tensões contínua (VDC) na faixa de 0V a 25V. Ele funciona da seguinte forma:
Na entrada do módulo, pode ser conectado um valor de tensão DC até cinco vezes maior que o VCC da porta analógica.
Nele, há dois pinos para o VCC e GND, e um pino (S) para a porta analógica do microcontrolador fazer a leitura.

.. note::  A resolução do ADC (conversor analógico digital) do arduino é de 10 bits, portanto, a resolução do sensor de tensão será de 0,00489V (5V / 1023). Logo, a tensão mínima na entrada para que o sensor possa realizar a leitura é de 0,02445V.

Sensor de Temperatura MAX6675 Termopar
-----
O Termopar é um sensor que mede a temperatura através de dois metais distintos que são unidos e ligados a um dispositivo que tenha capacidade de ler termopares. Se há uma diferença de temperatura entre a extremidade unida e as extremidades livres, uma diferença de potencial surge e se o termopar estiver conectado ao dispositivo que tenha capacidade de interpretar o sinal, um valor de temperatura correspondente será apresentado.
Ele tem a capacidade de ler de 0°C a 300°C
