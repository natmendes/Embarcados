## Lista 9 

#### 1. 1. Crie um programa em C que preenche matrizes 'long int A[60][60]' e 'long int B[60][60]' completamente com valores aleatórios (use a função random()), e que calcula a soma das duas matrizes por dois métodos:

_(a) De forma sequencial;_

``` C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main()
{
	int A[60][60], B[60][60], i, j, mat_result[60][60];
	for(i = 0; i < 60; i++)
	{
		for(j = 0; j < 60; j++)
			A[i][j] = rand() % 5;			
	}
	
	for(i = 0; i < 60; i++)
	{
		for(j = 0; j < 60; j++)
			B[i][j] = rand() % 5;			
	}
	
	for(i = 0; i < 60; i++)
	{
		for(j = 0; j < 60; j++)
		{
			mat_result[i][j] = A[i][j] + B[i][j];			
			printf("%d ", mat_result[i][j]);
		}
		printf("\n");
	}
	
}
```

_(b) De forma paralela._

``` C
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

#define LINHAS 60
#define COLUNAS 60

int A[LINHAS][COLUNAS];
int B[LINHAS][COLUNAS];
int mat_result[LINHAS][COLUNAS];

// static pthread_mutex_t mutexLock; 
void* soma_matriz (void *linha)
{
	int num_linha = * (int *) linha;
	for (int i = num_linha; i < num_linha + 10; i++)
	{
		for (int j = 0; j < COLUNAS; j++)
		{
			mat_result[i][j] = A[i][j] + B[i][j];
			// printf("%d ", matriz->mat_result[i][j]);
		}
	}
	//pthread_mutex_unlock(&mutexLock);
}

int main()
{
	
	int i, j;
	pthread_t thread[6];
	
	for(i = 0; i < LINHAS; i++)
	{
		for(j = 0; j < COLUNAS; j++)
		{	
			A[i][j] = rand() % 5;	
			printf("%d ", A[i][j]);
		}
		printf("\n"); 		
	}
	
	printf("\n");
	
	for(i = 0; i < LINHAS; i++)
	{
		for(j = 0; j < COLUNAS; j++)
		{
			B[i][j] = rand() % 5;
			printf("%d ", B[i][j]);
		}
		printf("\n");
	}

	printf("\n");

	//pthread_mutex_init(&mutexLock, NULL);
	
	int linhas[10];
	for (i = 0; i < 6; i++)
		linhas[i] = 10 * i;
	
	for (i = 0; i < 6; i++)
		pthread_create (&thread[i], NULL, &soma_matriz, &linhas[i]);
	
	for(i=0; i<6; i++)
		pthread_join(thread[i], NULL);
	
	printf("\n"); 
	
	for(i = 0; i < LINHAS; i++)
	{
		for(j = 0; j < COLUNAS; j++)
		{
			printf("%d ", mat_result[i][j]);
		}
		printf("\n");
	}
	
	//pthread_mutex_destroy(&mutexLock);		
}
```
