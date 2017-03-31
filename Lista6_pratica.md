## Lista 6 - Prática 

#### 1. Crie um código em C para gerar três processos-filho usando o fork().

``` C
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main()
{
	pid_t filho1=1, filho2=1, filho3=1;
	filho1 = fork();
	
	
	if (filho1 == 0)
	{
		printf("Eu sou o primeiro filho!\n");
		filho2 = fork();
	}
	if (filho2 == 0)
	{
		printf("Eu sou o segundo filho!\n");
		filho3 = fork();
	}
	if (filho3 == 0)
		printf("Eu sou o terceiro filho!\n");
	if (filho1 != 0 && filho2 != 0 && filho1 != 0)
		printf("E eu sou o pai!\n");
	
	return 0;	
	 
}
```
#### 2. Crie um código em C que recebe o nome de diversos comandos pelos argumentos de entrada (argc e *argv[]), e executa cada um sequencialmente usando system().

``` C
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main (int argc, char **argv)
{	
	int i=1, a;
	while (i <= argc)
	{
		a = system(argv[i]);
		i++;
	}
	return 0;
}
```

#### 3. Crie um código em C que recebe o nome de diversos comandos pelos argumentos de entrada (argc e *argv[]), e executa cada um usando fork() e exec().

``` C
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main (int argc, char **argv)
{	
	int i=1;
	pid_t child[argc]; 
	while (i <= argc)
	{
		child[i] = fork();
		if (child[i] == 0)
		{
			char *lista[] = {NULL, NULL};
			lista[0] = argv[i];
			execvp(argv[i], lista);
		}
		else sleep(1);
		i++;
	}
}
```

#### 4. Crie um código em C que gera três processos-filho usando o fork(), e que cada processo-filho chama a função uma vez. (Repare que a função Incrementa_Variavel_Global() recebe como entrada o ID do processo que a chamou.) Responda: a variável global 'v_global' foi compartilhada por todos os processos-filho, ou cada processo enxergou um valor diferente para esta variável?

``` C
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int v_global = 0;
void Incrementa_Variavel_Global(pid_t id_atual)
	{
		v_global++;
		printf("ID do processo que executou esta funcao = %d\n", id_atual);
		printf("v_global = %d\n", v_global);
}

int main()
{
	pid_t filho1=1, filho2=1, filho3=1;
	filho1 = fork();
	
	
	if (filho1 == 0)
	{
		printf("Eu sou o primeiro filho!\n");
		Incrementa_Variavel_Global(getpid());
		filho2 = fork();
	}
	if (filho2 == 0)
	{
		printf("Eu sou o segundo filho!\n");
		Incrementa_Variavel_Global(getpid());
		filho3 = fork();
	}
	if (filho3 == 0)
	{
		printf("Eu sou o terceiro filho!\n");
		Incrementa_Variavel_Global(getpid());
	}
	if (filho1 != 0 && filho2 != 0 && filho1 != 0)
		printf("Eu sou o pai!\n");
	
	return 0;	
	 
}
```

Cada processo enxergou um valor diferente para variável global. Isso se dá porque os outros processos filhos só foram criados depois que o primeiro processo filho foi criado. Logo, seguindo a lógica sequencial do programa, primeiro chama-se a função e depois cria-se outro filho. 

#### 5. Repita a questão anterior, mas desta vez faça com que o processo-pai também chame a função Incrementa_Variavel_Global(). Responda: a variável global 'v_global' foi compartilhada por todos os processos-filho e o processo-pai, ou cada processo enxergou um valor diferente para esta variável?

``` C
#include <stdio.h> 
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int v_global = 0;
void Incrementa_Variavel_Global(pid_t id_atual)
	{
		v_global++;
		printf("ID do processo que executou esta funcao = %d\n", id_atual);
		printf("v_global = %d\n", v_global);
}

int main()
{
	pid_t filho1=1, filho2=1, filho3=1;
	filho1 = fork();
	
	
	if (filho1 == 0)
	{
		printf("Eu sou o primeiro filho!\n");
		Incrementa_Variavel_Global(getpid());
		filho2 = fork();
	}
	if (filho2 == 0)
	{
		printf("Eu sou o segundo filho!\n");
		Incrementa_Variavel_Global(getpid());
		filho3 = fork();
	}
	if (filho3 == 0)
	{
		printf("Eu sou o terceiro filho!\n");
		Incrementa_Variavel_Global(getpid());
	}
	if (filho1 != 0 && filho2 != 0 && filho1 != 0)
	{
		printf("Eu sou o pai!\n");
		Incrementa_Variavel_Global(getppid());	
	}
	return 0;	
	 
}
```

Já nesse caso, o primeiro processo filho e o processo pai enxergaram o mesmo valor para variável global, enquanto os filhos subsequentes viram valores diferentes. Isso se dá porque ao fazer o primeiro fork, ele executa tanto o primeiro quanto o último if quase no mesmo tempo. Posteriormente, ele executa os demais fork() para o restante dos filhos. 
