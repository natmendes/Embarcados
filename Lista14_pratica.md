## Lista 14

#### 1. Escreva um código em C para gerar uma onda quadrada de 1 Hz em um pino GPIO do Raspberry Pi.	

``` C
#define MAXSTR 64
#include <stdio.h>
#include <stdlib.h>

int definir_direcao(int pin);
int definir_valor(int pin, int value);


int main()
{
	while(1)
	{
		definir_direcao(4);
		definir_valor(4,1);
		sleep(1);
		definir_valor(4,0);
		sleep(1);
	}
}

int definir_direcao(int pin)
{
	FILE *sysfs_handle = NULL;
	if ((sysfs_handle = fopen("/sys/class/gpio/export", "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir o arquivo!\n");
		return 1;
	}

	char str_pin[3];
	snprintf(str_pin, (3*sizeof(char)), "%d", pin);	
	
	if (fwrite(&str_pin, sizeof(char), 3, sysfs_handle)!=3)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir esse pino %d\n", pin);
		return 2;
	}
	fclose(sysfs_handle);
	
	char str_direction_file[MAXSTR];
	snprintf(str_direction_file, (MAXSTR*sizeof(char)), "/sys/class/gpio/gpio%d/direction", pin);
	if ((sysfs_handle = fopen(str_direction_file, "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir a pasta de direcao\n");
		return 3;
	}
	
	if (fwrite("out", sizeof(char), 4, sysfs_handle) != 4)
	{
		fprintf(stderr, "ERRO: Nao foi possivel escrever no pin %d na pasta de direcao\n", pin);
		return 4;
	}
	fclose(sysfs_handle);
	
	return 0;
}

int definir_valor(int pin, int value)
{
	FILE *sysfs_handle = NULL;
	char str_value_file[MAXSTR];

	snprintf (str_value_file, (MAXSTR*sizeof(char)), "/sys/class/gpio/gpio%d/value", pin);

	if ((sysfs_handle = fopen(str_value_file, "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir a pasta de valor\n");
		return 1;
	}

	char str_val[2];
	snprintf (str_val, (2*sizeof(char)), "%d", value);

	if(fwrite(str_val, sizeof(char), 2, sysfs_handle) != 2)
	{
		fprintf(stderr, "ERRO: Nao foi possivel escrever o valor %d no %d\n", value, pin);
		return 2;
	}
	fclose(sysfs_handle);
	
	return 0;
}
```

#### 2. Generalize o código acima para qualquer frequência possível.

``` C
#define MAXSTR 64
#include <stdio.h>
#include <stdlib.h>

int definir_direcao(int pin);
int definir_valor(int pin, int value);


int main()
{
	float frequencia, tempo;
	scanf("%f", &frequencia);
	tempo = 1/frequencia;
	while(1)
	{
		definir_direcao(4);
		definir_valor(4,1);
		sleep(tempo);
		definir_valor(4,0);
		sleep(tempo);
	}
}

int definir_direcao(int pin)
{
	FILE *sysfs_handle = NULL;
	if ((sysfs_handle = fopen("/sys/class/gpio/export", "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir o arquivo!\n");
		return 1;
	}

	char str_pin[3];
	snprintf(str_pin, (3*sizeof(char)), "%d", pin);	
	
	if (fwrite(&str_pin, sizeof(char), 3, sysfs_handle)!=3)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir esse pino %d\n", pin);
		return 2;
	}
	fclose(sysfs_handle);
	
	char str_direction_file[MAXSTR];
	snprintf(str_direction_file, (MAXSTR*sizeof(char)), "/sys/class/gpio/gpio%d/direction", pin);
	if ((sysfs_handle = fopen(str_direction_file, "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir a pasta de direcao\n");
		return 3;
	}
	
	if (fwrite("out", sizeof(char), 4, sysfs_handle) != 4)
	{
		fprintf(stderr, "ERRO: Nao foi possivel escrever no pin %d na pasta de direcao\n", pin);
		return 4;
	}
	fclose(sysfs_handle);
	
	return 0;
}

int definir_valor(int pin, int value)
{
	FILE *sysfs_handle = NULL;
	char str_value_file[MAXSTR];

	snprintf (str_value_file, (MAXSTR*sizeof(char)), "/sys/class/gpio/gpio%d/value", pin);

	if ((sysfs_handle = fopen(str_value_file, "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir a pasta de valor\n");
		return 1;
	}

	char str_val[2];
	snprintf (str_val, (2*sizeof(char)), "%d", value);

	if(fwrite(str_val, sizeof(char), 2, sysfs_handle) != 2)
	{
		fprintf(stderr, "ERRO: Nao foi possivel escrever o valor %d no %d\n", value, pin);
		return 2;
	}
	fclose(sysfs_handle);
	
	return 0;
}
```

#### 3. Crie dois processos, e faça com que o processo-filho gere uma onda quadrada, enquanto o processo-pai lê um botão no GPIO, aumentando a frequência da onda sempre que o botão for pressionado. A frequência da onda quadrada deve começar em 1 Hz, e dobrar cada vez que o botão for pressionado. A frequência máxima é de 64 Hz, devendo retornar a 1 Hz se o botão for pressionado novamente.

``` C
#define MAXSTR 64
#define GET_GPIO(g) (*(gpio+13)&(1<<g))
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int definir_direcao(int pin);
int definir_valor(int pin, int value);

volatile unsigned *gpio;


int main()
{
	int filho;
	float frequencia = 1, tempo;
	filho = fork();
	
	if (filho == 0)
	{
		while(1)
		{
			if (frequencia > 64)
				frequencia = 1;
			tempo = 1/frequencia;
			definir_direcao(4);
			definir_valor(4,1);
			sleep(tempo);
			definir_valor(4,0);
			sleep(tempo);
		}
	}
	else
	{
		while(1)
		{
			puts("Pressione o botao");
			while(GET_GPIO(4));
			puts("Botao pressionado");
			while(GET_GPIO(4)==0);
			frequencia *= 2;
		}
	}
}

int definir_direcao(int pin)
{
	FILE *sysfs_handle = NULL;
	if ((sysfs_handle = fopen("/sys/class/gpio/export", "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir o arquivo!\n");
		return 1;
	}

	char str_pin[3];
	snprintf(str_pin, (3*sizeof(char)), "%d", pin);	
	
	if (fwrite(&str_pin, sizeof(char), 3, sysfs_handle)!=3)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir esse pino %d\n", pin);
		return 2;
	}
	fclose(sysfs_handle);
	
	char str_direction_file[MAXSTR];
	snprintf(str_direction_file, (MAXSTR*sizeof(char)), "/sys/class/gpio/gpio%d/direction", pin);
	if ((sysfs_handle = fopen(str_direction_file, "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir a pasta de direcao\n");
		return 3;
	}
	
	if (fwrite("out", sizeof(char), 4, sysfs_handle) != 4)
	{
		fprintf(stderr, "ERRO: Nao foi possivel escrever no pin %d na pasta de direcao\n", pin);
		return 4;
	}
	fclose(sysfs_handle);
	
	return 0;
}

int definir_valor(int pin, int value)
{
	FILE *sysfs_handle = NULL;
	char str_value_file[MAXSTR];

	snprintf (str_value_file, (MAXSTR*sizeof(char)), "/sys/class/gpio/gpio%d/value", pin);

	if ((sysfs_handle = fopen(str_value_file, "w")) == NULL)
	{
		fprintf(stderr, "ERRO: Nao foi possivel abrir a pasta de valor\n");
		return 1;
	}

	/* If the file is good - then we write the string value "0" or "1" to it. */
	char str_val[2];
	snprintf (str_val, (2*sizeof(char)), "%d", value);

	if(fwrite(str_val, sizeof(char), 2, sysfs_handle) != 2)
	{
		fprintf(stderr, "ERRO: Nao foi possivel escrever o valor %d no %d\n", value, pin);
		return 2;
	}
	fclose(sysfs_handle);
	
	return 0;
}
```
