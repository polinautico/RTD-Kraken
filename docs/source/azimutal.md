Propulsor Azimutal
====
O que significa **Azimutal**? e **Porque** usá-lo?
----
A palavra **azimutal** tem origem da palavra azimute, que significa, "ângulo medido no plano horizontal entre o meridiano do lugar do observador e o plano vertical que contém o ponto observado.", exemplificando, seria o plano horizontal de um observador como vemos na imagem a baixo:

.. image:: imagens/azimute-e-altura.webp
  :align: center
  :width: 200
  :alt: Projeção Azimutal (referência)
 
Sendo assim, uma propulsão é um conjunto de motores: 

* O primeiro e o principal é o de propulsão do barco, que será responsável por fornecer aceleração e é geralmente especificado no edital do Duna.

* O segundo é responsável pela rotação do motor principal no eixo azimute.

E por que foi escolhido esse sistema de controle?
Pois esse sistema substitui o uso do leme, que geralmente em embarcações comuns serve para direcionar o fluxo de água, para assim mover o barco para a direção desejada.
Dessa forma, com o azimutal, conseguimos girar o motor para a direção desejada, dessa forma temos uma melhor manobrabilidade além de conseguir movimentar o modelo de formas que seriam impossíveis com um leme convencional, por exemplo, dar ré no barco.

.. image:: imagens/Ex_motor_azimutal.jpg
  :align: center
  :width: 300
  :alt: Exemplo de Azimutal

Projeto para o Kraken
------
 
Neste ano de 2023, para o Kraken decidimos fazer um sistema similar ao Baleia do ano anterior. O sistema azimutal será composto de três principais componentes:

* Motores DC com encoder
* Ponte H BTS7960
* Arduino nano
* Controle PID

Motor com Encoder
------

É simplesmente como o nome sugere, um motor dc com um sistema de encoder, sistema esse responsável pelo sensoreamento do motor em si, para dessa forma realizar controle sob o motor, ou seja, o encoder nos fornecerá informações sobre o motor, para calcularmos a **posição** de seu eixo ou sua **velocidade** de rotação e assim criarmos algorítimos para controlá-lo da forma desejada.

.. image:: imagens/motor_dc_com_encoder.jpg
  :align: center
  :width: 400
  :alt: Motor de Corrente Continua com Encoder

Como vemos na imagem acima do próprio dispositivo, temos duas partes do cilindro, a primeira e mais perto do eixo é um sistema de engrenagens para redução e a segunda e maior é o sistema eletromagnético do motor junto aos dispositivos de sensoriamento, que são nada mais que sensores de **efeito hall**, também vemos as suas conexões que são 6 pinos. Esses pinos são mostrados abaixo:


.. note:: Um sistema de engrenagem de redução servem para diminuir a velocidade de rotação do eixo, por exemplo, enquanto o eixo do motor gira em 750 RPM, e temos um sistema de engrenagens 1:75, o eixo final da caixa de redução irá girar em 1 RPM.


.. _Pinagem:

=====
Pinagem
=====
.. image:: imagens/conexao_motor_dc.png
  :align: center
  :width: 400
  :alt: Pinagem do Motor

Sendo da seguinte forma:

* Os pinos 1 (M1) e 6 (M2) são pinos de tensão para o motor, conectados na ponte h

* Os pinos 2 (GND encoder) e 5 (3.3v encoder) são pinos de tensão para o encoder, conectados no arduino

* Por fim e não menos importante os pinos 4 (C1) e 5 (C2) são pinos de dados do encoder/sensor, conectados no arduino

.. _Dimensionamento:

=====
Dimensionamento
=====

Para dimensionar um motor dc com encoder em nosso projeto devemos olhar 2 variáveis:

* Tensão
* RPM

A tensão deve do motor deve se adequar a tensão do projeto, como neste projeto utilizamos uma bateria de 12V e estamos alimentando a Ponte H com 12v, o ideal será utilizar um motor dc com a mesma tensão.

O RPM, Rotações Por Minuto, está atrelada a redução do motor, portanto se refere ao torque que podemos gerar com ele.
Quando escolhemos o RPM devemos nos atentar sempre ao torque, quanto menor for os RPM maior será o torque e a precisão do encoder, portanto conseguimos escolher uma posição e o motor seguirá com perfeição, porém perdemos velocidade e o torque de stall(parada) fica muito alto. 

O torque de stall é a força que o motor é capaz de fazer para frear o eixo, e conforme a redução aumenta(RPM diminui) esse torque cresce exponencialmente, sendo que a partir de um determinado ponto, frear o motor, chega a ser proibido, pois as engrenagens não aguentarão a força e se romperão.

.. note:: Geralmente é melhor e mais barato comprar pelo Aliexpress, temos uma maior variedade e preços mais baixos, portanto é melhor se planejar para comprar o DC com encoder cedo!

Abaixo um exemplo de tabela de especificações:

.. image:: imagens/DC_Encoder_tabela.png
  :align: center
  :width: 600
  :alt: Circuito Simplificado

.. _Controle:

=====
Controle
=====

Para se controlar a velocidade e o sentido do motor, é necessário utilizar uma **Ponte H** e um Arduino:

* Ponte H : Ela se comunica com o arduino e com o Motor DC, sendo responsável por mandar a tensão correta ao motor. (mais detalhes no próximo item).

* Arduino : Ele é responsável por ler os pinos do encoder (sensor hall), vistos em **Pinagem**, dessa forma detectando o sentido de rotação do motor e a sua velocidade, para assim controlar o motor.

Os pinos do encoder, C1 e C2, enviam sinais binarios de 1 e 0 ao arduino, utilizamos um pino de interrupção do arduino para interromper qualquer processo que o arduino esteja executando, assim que o sinal recebido em C1 *sobe para 1*, caso o sinal em C2 esteja em *0* temos o sentido horário, caso esteja em *1* temos o sentido anti-horário. Veja abaixo como os sensores hall atuam em sentido horário e anti-horario:

.. image:: imagens/DC_Encoder_hall.png
  :align: center
  :width: 600
  :alt: Circuito Simplificado

.. note:: O motor dc com encoder não identifica posição absoluta, ele apenas identifica posição relativa! Ou seja ele não consegue definir sua posição inicial (como por exemplo uma posição inicial para a embarcação ir avante, quando o barco ligar), mas ele sabe girar tantos graus para um lado e retornar de onde ele começou. Resolvemos esse problema com outro sensor que será apresentado posteriormente.

.. note:: Caso duvida ver: https://controlautomaticoeducacion.com/arduino/motor-dc-encoder/

Ponte H BTS7960
------

Ponte H é um circuito eletrônico de potência, ele é um chopper de classe E, mas deixando de lado essa parte teórica, vamos explicá-la de forma prática.
A ponte H tem esse nome por que é composto por um conjunto de chaves eletrônicas, sendo que o motor (load) fica no meio entre elas, veja a imagem abaixo:

.. image:: imagens/Ponte_H_Circuito.png
  :align: center
  :width: 400
  :alt: Circuito Simplificado
  
Esse circuito serve para o controlar motores de corrente contínua, fazendo-os girar tanto no sentido horário, quanto no sentido anti-horário, apenas variando o sentido da corrente elétrica. Além de possibilitar a controle de velocidade de rotação do motor, variando a tensão de saída.

Nesse projeto do Kraken, utilizaremos o modelo BTS7960, o driver (BTS7960B) dessa ponte H é apenas metade da ponte, portanto é utilizado dois drivers como veremos na figura abaixo (os drivers são o encapsulamentos quadrados), escolhemos esse modelo pois, ela aguenta uma corrente bem alta de até 43 A, funciona em um intervalo de tensão de 5 V ~ 45 V, além disso tem uma faixa de controle PWM de 25 kHz e por fim proteção de temperatura, tensão e corrente.

.. image:: imagens/Ponte_H_bts.png
  :align: center
  :width: 400
  :alt: Ponte H BTS7960
  
.. _Pinagem:

=====
Pinagem
=====
.. image:: imagens/ponte_h_conexao.png
  :align: center
  :width: 400
  :alt: Conexções da Ponte H

.. note:: O DATASHEET DESSA PONTE H ESTÁ ERRADA! **UTILIZE AS INSTRUÇÕES ABAIXO!!!!!**.   

Agora falando sobre pinagem, vemos que ele possui 8 pinos de controle e são utilizados da seguinte forma:

* Pinos 8(GND) e 7(VCC): conectados no microcontrolador sendo GND e 5V, respectivamente (INPUT VOLTAGE)

* Pinos 6(L_IS) e 5(R_IS): são pinos de monitoramento de corrente em cada sentido de rotação (OUTPUT) 

* Pinos 4(L_EN) e 3(R_EN): controlam a velocidade do motor em cada sentido de rotação (ANALOG/PWM INPUT)

* Pinos 2(LPWM) e 1(RPWM): controlam o sinal de enable em cada sentido de rotação (HIGH/LOW INPUT)

.. note:: **!!NUNCA LIGUE OS PINOS 4, 3 , 2 e 1 NO HIGH AO MESMO TEMPO**. quando queremos liga o motor no sentido horário mandamos um sinal de VCC (HIGH) para RPWM e um sinal de GND (0v) para o LPWM, e para o sentido oposto basta fazer a lógica oposta. Os pinos 4 e 3 são **OS SINAIS PWM!**. 
  
Arduino nano
------

Esse microcontrolador é o responsável por unir todos esses componentes e fazer todos os cálculo e lógicas necessárias para controlar a posição do motor dc com encoder, ou melhor, controlar a posição de cada azimutal.

Seu principal papel é receber um ângulo de um outro microcontrolador mestre, e movimentar o Motor DC com Encoder para a posição desejada, parece simples neh? Porém tem muita programação por trás disso.

Será considerado que neste ponto, você leitor, já possui uma noção de programação em C++ para entender os códigos que virão a seguir.

Primeiramente listamos abaixo todas as bibliotecas utilizadas no Nano:

```console
foo@bar:~$ whoami
#include <Wire.h>
#include <Arduino.h>
#include <PIDController.h>
```

# Install required Python dependencies (MkDocs etc.)
pip install -r docs/requirements.txt

# Run the mkdocs development server
mkdocs serve





Utilizamos a Wire.h para utilizar conexão I2C, Arduino.h é a biblioteca padrão do arduino, mas como utilizamos o PlataformIO para programar temos que adicioná-la a parte, PIDController.h é um biblioteca própria criada por nós mesmos, onde está programada o controlador PID que mais a frente.

A parte de comunicação I2C deixaremos para discutir no tópico do Heltec Esp32 Wifi LoRa V2.

Primeiramente iremos mostrar como foi programado o encoder do Motor DC:




.. image:: imagens/arduino_nano.png
  :align: center
  :width: 300
  :alt: Arduino nano

.. image:: imagens/arduino_nano_pins.jpeg
  :align: center
  :width: 500
  :alt:  Pinagem do Arduino
  
.. image:: imagens/conexao.jpeg
  :align: center
  :width: 500
  :alt:  Pinagem do Arduino

Esquema de Conexões 
------

Escreva aqui
  
Controle PID
------

Esse controle faz parte da teoria de engenharia de controle, é um controle que une as ações proporcional, integrativo e derivativo. 

Caso não tenha interesse -> apenas use o código que irá funcionar :D


