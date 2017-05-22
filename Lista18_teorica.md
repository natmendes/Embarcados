## Lista 18

#### 1. Cite as vantagens e desvantagens das comunicação serial I2C.

Uma das vantagens desse comunicação é que há o bit "acknowledgment", onde é possível saber se os escravos (ou o mestre) foram identificados com sucesso e assim a comunicação foi passada. Já a desvantagem é que são só dois fios, um para o clock e um para a informação, logo é preciso de autorização do mestre para comunicação e também todas as informações devem ser passadas pelo SDA (serial data line), ou seja, tanto a informação desejada, como o endereço do escravo, o bit indicando se é de leitura ou escrita e também o bit de acknowledgment. 

#### 2. Considere o caso em que a Raspberry Pi deve receber leituras analógico/digitais de um MSP430, e que a comunicação entre os dois é I2C. É tecnicamente possível que o MSP430 mande os resultados da conversão A/D a qualquer hora, ou ele deve aguardar a Raspberry Pi fazer um pedido ao MSP430? Por quê?

Ele deve aguardar a Raspberry, pois como é só um fio para enviar e receber as informações, deve haver uma ordem de transmissão. Logo, o mestre indica o endereço do escravo e decide se ele vai ler ou enviar a informação. 

#### 3. Se o Raspberry Pi tiver de se comunicar com dois dispositivos via I2C, como executar a comunicação com o segundo dispositivo?

Basta indicar o endereço do segundo escravo na hora de enviar a informação. Inicialmente envia-se o bit de start na borda de descida quando o clock está em 1, depois envia-se o endereço do escravo, depois se o mestre vai ler ou escrever, posteriormente o bit de reconhecimento do escravo e finalmente a informação a ser passada. 
