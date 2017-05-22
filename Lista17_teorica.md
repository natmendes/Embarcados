## Lista 17 

#### 1. Cite as vantagens e desvantagens das comunicação serial:

	_(a) Assíncrona (UART)._
	
Uma das vantagens da UART é que ela é assíncrona, logo por não haver a necessidade do clock, também não há preocupações com os resistores de pull-up. Porém, isso ao mesmo tempo pode ser uma desvantagem já que como não há a temporização do bit, tanto o transmissor quanto o receptor devem sabem previamente a duração.  

	_(b) SPI._
	
As vantagens da SPI é que o projetista pode escolher a velocidade da transmissão, podendo chegando até 500000. A desvantagem é que é a comunicação que mais necessita de fios. 

#### 2. Considere o caso em que a Raspberry Pi deve receber leituras analógico/digitais de um MSP430, e que a comunicação entre os dois é UART. É tecnicamente possível que o MSP430 mande os resultados da conversão A/D a qualquer hora, ou ele deve aguardar a Raspberry Pi fazer um pedido ao MSP430? Por quê?

O MSP430 pode mandar informações sem nenhum requisição da Raspberry. Isso se dá porque são dois fios distintos, um para enviar e um para receber, e não há um clock. Logo, tanto a Rasberry pode dar o bit de START e começar a informação quanto o MSP430. 

#### 3. Considere o caso em que a Raspberry Pi deve receber leituras analógico/digitais de um MSP430, que a comunicação entre os dois seja SPI, e que o MSP430 seja o escravo. É tecnicamente possível que o MSP430 mande os resultados da conversão A/D a qualquer hora, ou ele deve aguardar a Raspberry Pi fazer um pedido ao MSP430? Por quê?

Dessa vez, o MSP430 deve aguardar o pedido da Raspberry. Isso se dá porque na comunicação SPI, o clock está no mestre, que é a Raspberry. Logo, é necessário a autorização do mestre para a comunicação pois é ele quem vai dar o sinal do clock.

#### 4. Se o Raspberry Pi tiver de se comunicar com dois dispositivos via UART, como executar a comunicação com o segundo dispositivo?

Para isso, após mandar o bit de START e os bits que contém a informação desejada, o próximo bit é o de endereço do dispositivo escolhido. Sabendo desse endereço, a informação é enviada para o dispositivo requisitado. 

#### 5. Se o Raspberry Pi tiver de se comunicar com dois dispositivos via SPI, como executar a comunicação com o segundo dispositivo?

Já na comunicação SPI é mais fácil pois existe a possibilidade de colocar mais um fio para indicar o endereço do escravo. Dessa forma, basta ligar esse fio nos escravos e setar e/ou zerar os escravos necessários para a comunicação só com um deles. 
