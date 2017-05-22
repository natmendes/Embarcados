## Lista 18 

#### 1. Considere um MSP430 sendo usado para leituras analógicas. O Raspberry Pi está conectado a ele via I2C, e é o mestre. O MSP430 foi programado para funcionar da seguinte forma:

#### I. Receber o byte 0x55, o que indica o começo de conversão. 

#### II. 100us depois, o MSP430 envia o byte menos significativo e o mais significativo da conversão de 10 bits, nesta ordem.

#### Escreva o código para o Raspberry Pi executar este protocolo, de forma a obter conversões a cada 10 ms. A cada 1 segundo ele deve apresentar no terminal a média das últimas 100 amostras.

´´´ C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <fcntl.h>
#include <linux/ioctl.h>
#include <linux/i2c-dev.h>

int i2c_fd;
void ctrl_c(int sig)
{
	close(i2c_fd);
	exit(-1);
}

int main(void)
{
	unsigned char begin = 0x55, msp430_ret_1, msp430_ret_2, rpi_addr = 0xDA, slave_addr=0x0F;
	long int soma = 0;
	int i = 0; 
	float media = 0; 
	signal(SIGINT, ctrl_c);
	i2c_fd = open("/dev/i2c-1", O_RDWR);
	ioctl(i2c_fd, I2C_SLAVE, slave_addr);

	while(1)
	{
		write(i2c_fd, &begin, 2);		
		usleep(100);
		
		while(i != 99)
		{
			read(i2c_fd, &msp430_ret_1, 2);
			printf("Byte menos significativo: %s\n", msp430_ret_1);
		
			read(i2c_fd, &msp430_ret_2, 2);
			printf("Byte mais significativo: %s\n", msp430_ret_2);
		
			soma += atoi(msp430_ret_1) + atoi(msp430_ret_1);
			i++;
		}
		media = soma/100;
		printf("Media: %f\n", media); 
		media = 0; 
		i = 0; 
		soma = 0;
	}
		
	close(i2c_fd);
}
´´´
