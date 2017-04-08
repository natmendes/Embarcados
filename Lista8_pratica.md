## Lista 8 

#### 1. Crie um programa em C que cria uma thread, e faça com que o programa principal envie os valores 1, 2, 3, 4, 5, 6, 7, 8, 9 e 10 para a thread, com intervalos de 1 segundo entre cada envio. Depois de o programa principal enviar o número 10, ele aguarda 1 segundo e termina a execução. A thread escreve na tela cada valor recebido, e quando ela receber o valor 10, ela termina a execução.

``` C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

void* printa_numero(void* arg)
{
	int *numero = (int *) arg;
	for (int i=0; i < 10; i++)
	{
		sleep(1);
		printf("%d\n", numero[i]);
		if (numero[i] == 10)
			exit(1);
	}
}

int main()
{
	pthread_t thread1;
	int v[10] = {1,2,3,4,5,6,7,8,9,10}; 
	pthread_create (&thread1, NULL, &printa_numero, &v);
	while(1);
	return 0; 
}
```
#### 2. Crie um programa em C que preenche o vetor 'long int v[50000]' completamente com valores aleatórios (use a função random()), e que procura o valor máximo do vetor por dois métodos:
####	(a) Pela busca completa no vetor v[];
####	(b) Separando o vetor em 4 partes, e usando 4 threads para cada uma encontrar o máximo de cada parte. Ao final das threads, o programa principal compara o resultado das quatro threads para definir o máximo do vetor.
#### Ao final do programa principal, compare os resultados obtidos pelos dois métodos.

``` C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>

int main()
{
	int i, valor_maximo;
	long int v[50000];
	
	for(i=0 ; i <= 50000 ; i++) 
        v[i] = rand();
    
    for(i=0 ; i <= 50000 ; i++)
    {
    	if (i == 0)
    		valor_maximo = v[i];
    	else 
    	{
    		if (v[i] > v[i-1])
    			valor_maximo = v[i];
    	}
    }  
    
    printf("%d", valor_maximo);
    return 0; 
        
}
```

``` C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

struct Argumentos
{
	int f;
	int valor_maximo=0;
};



void* compara_valor (void* arg)
{
	struct Argumentos* dados = (struct Argumentos*) arg;
	if (dados->f > dados->valor_maximo)
		dados->valor_maximo = dados->f;
	return NULL; 
	
}


int main()
{
	long int v[12];
	int i = 1, n=1, j=1, ret[4], val_max;
	pthread_t thread1;
	pthread_t thread2;
	pthread_t thread3;
	pthread_t thread4;
	
	for (j = 0; j < 12; j++)
		v[j] = rand() % 20;
		
	struct Argumentos arg_tr1;
	struct Argumentos arg_tr2;
	struct Argumentos arg_tr3;
	struct Argumentos arg_tr4;
	
	for (i = 1; i <= 3 ; i++) //12499
	{
		arg_tr1.f = v[i];
		pthread_create (&thread1, NULL, &compara_valor, &arg_tr1);	
	}
	
	for (i = 4; i <= 6; i++) //12500 24999
	{
		arg_tr2.f = v[i];
		pthread_create (&thread2, NULL, &compara_valor, &arg_tr2);	
	}
	
	for (i = 7; i <= 9 ; i++) //25000 37599
	{
		arg_tr3.f = v[i];
		pthread_create (&thread3, NULL, &compara_valor, &arg_tr3);	
	}
	
	for (i = 10; i <= 12; i++) // 37500 50000
	{
		arg_tr4.f = v[i];
		pthread_create (&thread4, NULL, &compara_valor, &arg_tr4);	
	}
	
	pthread_join(thread1, NULL);
	pthread_join(thread2, NULL);
	pthread_join(thread3, NULL);
	pthread_join(thread4, NULL);
		
	printf("%d", val_max);
	return 0;
	
}
```

#### 3. Repita o exercício anterior, mas calcule a média do vetor ao invés do valor máximo.

``` C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

struct Argumentos
{
	int N;
	float media;
	long int *v;
};



void* compara_valor (void* arg)
{
	struct Argumentos* dados = (struct Argumentos*) arg;
	dados->media = 0;
	for(int i = 0; i < dados->N; i++)
	{
		dados->media += dados->v[i];
	}
	return NULL;
}

int main()
{
	long int v[50000];
	int i = 1, j=1;
	long int val_max;
	pthread_t thread1;
	pthread_t thread2;
	pthread_t thread3;
	pthread_t thread4;
	
	for (j = 0; j < 50000; j++)
		v[j] = rand() % 2;
		
	struct Argumentos arg_tr1;
	struct Argumentos arg_tr2;
	struct Argumentos arg_tr3;
	struct Argumentos arg_tr4;
	
	arg_tr1.v = &v[0];
	arg_tr1.N = 12500;
	pthread_create (&thread1, NULL, &compara_valor, &arg_tr1);	
		
	arg_tr2.v = &v[12500];
	arg_tr2.N = 12500;
	pthread_create (&thread2, NULL, &compara_valor, &arg_tr2);
	
	arg_tr3.v = &v[25000];
	arg_tr3.N = 12500;
	pthread_create (&thread3, NULL, &compara_valor, &arg_tr3);
	
	arg_tr4.v = &v[37500];
	arg_tr4.N = 12500;
	pthread_create (&thread4, NULL, &compara_valor, &arg_tr4);
	
	pthread_join(thread1, NULL);
	pthread_join(thread2, NULL);
	pthread_join(thread3, NULL);
	pthread_join(thread4, NULL);
	
	struct Argumentos args;
	args.N = 4;
	args.v = &v[0];
	v[0] = arg_tr1.media;
	v[1] = arg_tr2.media;
	v[2] = arg_tr3.media;
	v[3] = arg_tr4.media;
	
	compara_valor((void *) &args);
		
	printf("%f\n", args.media/50000);
	return 0;
}
```

#### 4. Repita o exercício anterior, mas calcule a variância do vetor ao invés da média.

```C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

struct Argumentos
{
	int N;
	float media;
	long int *v;
};

struct Argumentos_variancia
{
	int N;
	float media;
	long int *v;
	float variancia;
};

void* calcula_variancia (void* arg1)
{
	struct Argumentos_variancia* dados1 = (struct Argumentos_variancia*) arg1;
	dados1->variancia = 0;
	for(int i = 0; i < dados1->N; i++)
		dados1->variancia += (dados1->v[i] - dados1->media) * (dados1->v[i] - dados1->media);
	return NULL;	
}


void* compara_valor (void* arg)
{
	struct Argumentos* dados = (struct Argumentos*) arg;
	dados->media = 0;
	for(int i = 0; i < dados->N; i++)
		dados->media += dados->v[i];
	return NULL;
}

int main()
{
	long int v[50000];
	int i = 1, j=1;
	long int val_max;
	float media_total, variancia;
	pthread_t thread1;
	pthread_t thread2;
	pthread_t thread3;
	pthread_t thread4;
	pthread_t thread5;
	pthread_t thread6;
	pthread_t thread7;
	pthread_t thread8;
	
	for (j = 0; j < 50000; j++)
		v[j] = rand();
		
	struct Argumentos arg_tr1;
	struct Argumentos arg_tr2;
	struct Argumentos arg_tr3;
	struct Argumentos arg_tr4;
	
	struct Argumentos_variancia argv_tr1;
	struct Argumentos_variancia argv_tr2;
	struct Argumentos_variancia argv_tr3;
	struct Argumentos_variancia argv_tr4;
	
	arg_tr1.v = &v[0];
	arg_tr1.N = 12500;
	pthread_create (&thread1, NULL, &compara_valor, &arg_tr1);	
		
	arg_tr2.v = &v[12500];
	arg_tr2.N = 12500;
	pthread_create (&thread2, NULL, &compara_valor, &arg_tr2);
	
	arg_tr3.v = &v[25000];
	arg_tr3.N = 12500;
	pthread_create (&thread3, NULL, &compara_valor, &arg_tr3);
	
	arg_tr4.v = &v[37500];
	arg_tr4.N = 12500;
	pthread_create (&thread4, NULL, &compara_valor, &arg_tr4);
	
	pthread_join(thread1, NULL);
	pthread_join(thread2, NULL);
	pthread_join(thread3, NULL);
	pthread_join(thread4, NULL);
	
	struct Argumentos args;
	args.N = 4;
	args.v = &v[0];
	v[0] = arg_tr1.media;
	v[1] = arg_tr2.media;
	v[2] = arg_tr3.media;
	v[3] = arg_tr4.media;
	
	compara_valor((void *) &args);
	
	media_total = args.media/50000;
		
	argv_tr1.v = &v[0];
	argv_tr1.N = 12500;
	argv_tr1.media = media_total;
	pthread_create (&thread5, NULL, &calcula_variancia, &argv_tr1);	
	
	argv_tr2.v = &v[12500];
	argv_tr2.N = 12500;
	argv_tr2.media = media_total;
	pthread_create (&thread6, NULL, &calcula_variancia, &argv_tr2);
	
	argv_tr3.v = &v[25000];
	argv_tr3.N = 12500;
	argv_tr3.media = media_total;
	pthread_create (&thread7, NULL, &calcula_variancia, &argv_tr3);
	
	argv_tr4.v = &v[37500];
	argv_tr4.N = 12500;
	argv_tr4.media = media_total;
	pthread_create (&thread8, NULL, &calcula_variancia, &argv_tr4);
	
	pthread_join(thread5, NULL);
	pthread_join(thread6, NULL);
	pthread_join(thread7, NULL);
	pthread_join(thread8, NULL);
	
	variancia = (argv_tr1.variancia + argv_tr2.variancia + argv_tr3.variancia + argv_tr4.variancia)/50000;
	
	printf("Variância: %f\n", variancia);	
	
	return 0;
}
```
