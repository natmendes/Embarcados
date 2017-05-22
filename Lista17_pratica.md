## Lista 17 

#### 1. Considere um MSP430 sendo usado para leituras analógicas. O Raspberry Pi está conectado a ele via UART. O MSP430 foi programado para converter e enviar dados de 10 bits a cada 10 ms. Escreva o código para o Raspberry Pi receber estes dados, e cada 1 segundo apresentar no terminal a média das últimas 100 amostras.

´´´ C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>
#include <fcntl.h>
#include <termios.h>

#define TTY "/dev/ttyS0"

int uart0_fd;
void ctrl_c(int sig)
{
	puts("Fechando " TTY "...");
	close(uart0_fd);
	exit(-1);
}

int main(void)
{
	struct termios options;
	int i = 0; 
	long int info, soma = 0;
	float media; 

	signal(SIGINT, ctrl_c);
	uart0_fd = open(TTY, O_RDWR); // | O_NOCTTY); // | O_NDELAY);
	if(uart0_fd==-1)
	{
		puts("Erro abrindo a UART.");
		return -1;
	}
	puts(TTY " aberto.");
	tcgetattr(uart0_fd, &options);
	options.c_cflag = CS8 | CREAD | CLOCAL;
	options.c_iflag = 0;
	options.c_oflag = 0;
	options.c_lflag = 0;
	options.c_cc[VTIME] = 0;
	options.c_cc[VMIN]  = 1;
	cfsetospeed(&options, B9600);
    cfsetispeed(&options, B9600);
	tcflush(uart0_fd, TCIOFLUSH);
	tcsetattr(uart0_fd, TCSANOW, &options);
	puts("UART configurada:");
	system("stty -F " TTY);
	puts("");
	while(1)
	{
		while (i != 99)
		{
			read(uart0_fd, &info, 2);
			soma += info; 
			i++;
		}
		
		media = soma/100;
		printf("Media: %f\n", media);
		soma = 0; 
		media = 0; 
		i = 0; 
	}
	close(uart0_fd);
}
´´´

#### 3. Considere um MSP430 sendo usado para leituras analógicas. O Raspberry Pi está conectado a ele via SPI, e é o mestre. O MSP430 foi programado para funcionar da seguinte forma:

#### I. Receber o byte 0x55 e enviar o byte 0xAA, o que indica o começo de conversão. 

#### II. 100us depois, o MSP430 recebe os bytes 0x01 e 0x02, e envia o byte menos significativo e o mais significativo da conversão de 10 bits, nesta ordem.

#### Escreva o código para o Raspberry Pi executar este protocolo, de forma a obter conversões a cada 10 ms. A cada 1 segundo ele deve apresentar no terminal a média das últimas 100 amostras.

´´´ C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <wiringPi.h>
#include <wiringSerial.h>
#include <errno.h>

int spi_fd;
void ctrl_c(int sig)
{
	close(spi_fd);
	exit(-1);
}

int main(void)
{
	unsigned char send_msp430[5], send_msp430_1[5], send_msp430_2[5];
	long int soma = 0; 
	float media; 

	signal(SIGINT, ctrl_c);
	if(wiringPiSetup() == -1)
	{
		puts("Erro em wiringPiSetup().");
		return -1;
	}
	spi_fd = wiringPiSPISetup (0, 500000);
	if(spi_fd==-1)
	{
		puts("Erro abrindo a SPI.);
		return -1;
	}
	while(1)
	{
		while (i != 99)
		{
			send_msp430 = 0x55; 
			wiringPiSPIDataRW(0, &send_msp430, 1024);
			printf("MSP430_return = %d\n", send_msp430);	
		
			if (send_msp430 == 0xAA)
			{
				send_msp430_1 = 0x01;
				wiringPiSPIDataRW(0, &send_msp430_1, 5);
				printf("Bit menos significativo = %d\n", send_msp430_1);
		
				send_msp430_2 = 0x02;
				wiringPiSPIDataRW(0, &send_msp430_2, 5);
				printf("Bit mais significativo = %d\n", send_msp430_2);
			}
			else printf("Erro na comunicacao!\n");
			soma += atoi(send_msp430_1) + atoi(send_msp430_2);
			i++;
		}
		
		media = soma/100;
		printf("Media: %f\n", media); 
		media = 0; 
		i = 0; 
		soma = 0; 
	}
	close(spi_fd);
}
´´´
