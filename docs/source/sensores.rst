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
  
No caso do nosso rebocador, utilizaremos a telemetria para monitorar o que está havendo com a parte elétrica, afim de entender o desempenho da embarcação.


A Telemetria do Kraken e dispositivos
-----
Para o Kraken, utilizaremos a telemetria para monitorar a tensão e a temperatura dos componentes elétricos utilizando:

* Sensor de Tensão DC 0 a 25V
* Sensor de Temperatura MAX6675 Termopar Tipo K
* ESP LoRa 32 V2

Sensor de Tensão DC 0 a 25V
-----
Como o nome diz, é um sensor que mede a tensões contínua (VDC) na faixa de 0V a 25V. Ele funciona da seguinte forma:
Na entrada do módulo, pode ser conectado um valor de tensão DC até cinco vezes maior que o VCC da porta analógica.
Nele, há dois pinos para o VCC e GND, e um pino (S) para a porta analógica do microcontrolador fazer a leitura.

.. image:: imagens/tensaosens.png
  :align: center
  :width: 300

.. note::  A resolução do ADC (conversor analógico digital) do arduino é de 10 bits, portanto, a resolução do sensor de tensão será de 0,00489V (5V / 1023). Logo, a tensão mínima na entrada para que o sensor possa realizar a leitura é de 0,02445V.

Sensor de Temperatura MAX6675 Termopar
-----
O Termopar é um sensor que mede a temperatura que tem a capacidade de ler de 0°C a 300°C, através de dois metais distintos que são unidos e ligados a um dispositivo que tenha capacidade de ler a temperatura. Se há uma diferença de temperatura entre a extremidade unida e as extremidades livres, uma diferença de potencial surge e se o Termopar estiver conectado ao dispositivo que tenha capacidade de interpretar o sinal (no nosso caso, o ESP32), um valor de temperatura correspondente será apresentado.
A sonda que faz partedo MAX6675 é revestida de aço inoxidável, com uma blindagem na ponta, o que permite que o dispositivo capte temperaturas altas.

.. image:: imagens/senstemp2.png
  :align: center
  :width: 300
 
.. image:: imagens/senstemp1.jpg
  :align: center
  :width: 500

.. note:: O sensor tem uma biblioteca própria, que é a "max6675.h"

Na questão dos pinos, ele possui os seguintes para se comunicar com o microcontrolador e alimentação:

* GND: Terra;
* VCC: 3V a 5,5V;
* SCK: Serial Clock input, canal utilizado para sincronizar a transmissão dos dados;
* CS: Chip Select;
* SO: Serial Data output.

Heltec ESP 32 LoRa (V2)
-----
Pedro Pedro Pedro Pedro Pedro Pedro Pedro Pedro

Esquema de Conexões
-----

Dashboard e Node-Red
-----
Primeiramente, precisamos detalhar como faremos para visualizar os dados da telemetria do Kraken. Nesse caso, utilizaremos uma estratégia chamada "Dashboard"

Uma dashboard é nada mais e nada menos que um painel visual que apresenta, de forma compacta e centralizada, diversas informações as quais o usuário necessite. É uma entratégia de controle utilizada para facilitar a tomada de decisões. Junto com a telemetria, a dashboard permite que tenhamos, de fato, o controle sobre as informações necessárias para o entendimento da nossa embarcação.

Ok, agora fica a pergunta: como que construímos essa dashboard?
Bom, a resposta é simples: utilizamos uma ferramenta Open-Source (isso é, Código Aberto, onde todos podem contrubuir para o desenvolvimento da mesma). E para a telemetria do Kraken, escolhemos o Node-Red.

.. image:: imagens/noderedex.png
  :align: center
