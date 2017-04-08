## Lista 7

#### 1. 1. Crie um programa em C que cria um processo-filho e um pipe de comunicação. Faça com que o processo-pai envie os valores 1, 2, 3, 4, 5, 6, 7, 8, 9 e 10 para o processo-filho, com intervalos de 1 segundo entre cada envio. Depois de o processo-pai enviar o número 10, ele aguarda 1 segundo e termina a execução. O processo-filho escreve na tela cada valor recebido, e quando ele receber o valor 10, ele termina a execução.

``` C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>
#include <string.h>

int main ()
{
	int pid;
	int fd[2];
	int i=1;
	
	pipe(fd);
	pid = fork();
	if (pid == 0)
	{
		while (i < 11)
		{
			read(fd[0], &i, sizeof(int));
			printf("%d\n", i);
			if (i == 10)
				exit(1);
		}
	}
	else
	{
		while (i < 11)
		{
			write(fd[1], &i, sizeof(int));
			sleep(1);
			i++;
		}
	sleep(1);
	exit(1);
	}
}
```

#### 2. Crie um programa em C que cria um processo-filho e um pipe de comunicação. Utilize o pipe para executar a seguinte conversa entre processos:

#### FILHO: Pai, qual é a verdadeira essência da sabedoria?
#### PAI: Não façais nada violento, praticai somente aquilo que é justo e equilibrado.
#### FILHO: Mas até uma criança de três anos sabe disso!
#### PAI: Sim, mas é uma coisa difícil de ser praticada até mesmo por um velho como eu...

#### Neste exercício, quem recebe a mensagem via pipe é quem as escreve no terminal.

``` C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>
#include <string.h>

int main()
{
	pid_t filho;
	int fd[2];
	char mensagem1[100] = {"Pai, qual é a verdadeira essência da sabedoria?"};
	char mensagem2[100] = {"Não façais nada violento, praticai somente aquilo que é justo e equilibrado."};
	char mensagem3[100] = {"Mas até uma criança de três anos sabe disso!"};
	char mensagem4[100] = {"Sim, mas é uma coisa difícil de ser praticada até mesmo por um velho como eu..."};
	pipe(fd); 
	filho = fork();
	
	if (filho == 0)
	{
		write(fd[1], mensagem1, 100*sizeof(char));
		sleep(1);
		if(read(fd[0], mensagem2, 100*sizeof(char)) < 0) 
			printf("Erro na leitura do pipe\n");
		else printf("PAI: %s\n", mensagem2);
		write(fd[1], mensagem3, 100*sizeof(char));
		sleep(1);
		if(read(fd[0], mensagem4, 100*sizeof(char)) < 0) 
			printf("Erro na leitura do pipe\n");
		else printf("PAI: %s\n", mensagem4);
		exit(1);
	}
	else
	{
		
		if(read(fd[0], mensagem1, 100*sizeof(char)) < 0) 
			printf("Erro na leitura do pipe\n");
		else printf("FILHO: %s\n", mensagem1);
		write(fd[1], mensagem2, 100*sizeof(char));
		sleep(1);
		if(read(fd[0], mensagem3, 100*sizeof(char)) < 0) 
			printf("Erro na leitura do pipe\n");
		else printf("FILHO: %s\n", mensagem3);
		write(fd[1], mensagem4, 100*sizeof(char));
		sleep(1);
		exit(1);
	}
	
	return 0;
}
```

#### 3. Crie um programa em C que cria dois processos-filhos e um pipe de comunicação. Utilize o pipe para executar a seguinte conversa entre processos:

#### FILHO1: Quando o vento passa, é a bandeira que se move.
#### FILHO2: Não, é o vento que se move.
#### PAI: Os dois se enganam. É a mente dos senhores que se move.

#### Neste exercício, quem recebe a mensagem via pipe é quem as escreve no terminal.

``` C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>
#include <string.h>

int main()
{
	pid_t filho1, filho2; 
	int fd[2];
	char mensagem1[100] = {"Quando o vento passa, é a bandeira que se move."};
	char mensagem2[100] = {"Não, é o vento que se move."};
	char mensagem3[100] = {"Os dois se enganam. É a mente dos senhores que se move."};
	pipe(fd);
	filho1 = fork(); 
	
	if (filho1 == 0)
	{
		read(fd[0], mensagem1, 100*sizeof(char)); 
		printf("FILHO1: %s\n", mensagem1); 
		filho2 = fork(); 
		if (filho2 == 0)
		{
			read(fd[0], mensagem2, 100*sizeof(char));
			printf("FILHO2: %s\n", mensagem2); 
			write(fd[1], mensagem3, 100*sizeof(char)); 
		}
		
	}
	else 
	{
		write(fd[1], mensagem1, 100*sizeof(char));
		sleep(1);
		write(fd[1], mensagem2, 100*sizeof(char)); 
		sleep(1);  
		read(fd[0], mensagem3, 100*sizeof(char));
		printf("PAI: %s\n", mensagem3); 
	}
	
	return 0; 

}
```

#### 4. Crie um programa em C que cria um processo-filho e um pipe de comunicação. O processo-filho deverá pedir o nome do usuário, envia-lo para o pai via pipe, e o pai deverá escrever o nome do usuário no terminal.

``` C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

int main()
{
	pid_t filho; 
	int fd[2];
	char nome[100];
	pipe(fd); 
	filho = fork(); 
		
	if (filho == 0)
	{
		printf("Digite seu nome:\n"); 
		scanf("%[^\n]", nome); 
		write(fd[1], nome, 100*sizeof(char)); 
	}
	else
	{
		sleep(1);
		if(read(fd[0], nome, 100*sizeof(char)) < 0) 
			printf("Erro na leitura do pipe\n");
		else
			printf("Seu nome é: %s\n", nome);
	}
	return 0; 
}
```

#### 5. Utilize o sinal de alarme para chamar a cada segundo o comando 'ps' ordenando todos os processos de acordo com o uso da CPU. Ou seja, seu código atualizará a lista de processos a cada segundo. Além disso, o código deverá tratar o sinal do CTRL-C, escrevendo "Processo terminado!" na tela antes de terminar a execução do processo.

``` C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void parar_processo()
{
	printf("Processo terminado.\n"); 
	exit(1); 
}

void alarme(int sig)
{
	system("ps");
	alarm(1);
}

int main()
{
	signal(SIGALRM, alarme);
	alarm(1);
	signal(SIGINT, parar_processo);
	printf("Digite CRTL+C quando desejar parar.\n"); 
	while(1); 
	return 0; 
}
```
