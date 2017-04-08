## Lista 7 

#### 1. Quantos pipes serão criados após as linhas de código a seguir? Por quê?

_(a) int pid;_
	_int	fd[2];_
	_pipe(fd);_
	_pid = fork();_
	
Será criado apenas um pipe pois ele foi criado antes da criação do processo pai e processo filho. 

_(b) int pid;_
	 _int	fd[2];_
	 _pid = fork();_
	 _pipe(fd);_
	 
Nesse caso, será criado dois pipes, um no processo pai e um no processo filho, pois depois do fork() já houve a divisão de processos. 

#### 2. Apresente mais cinco sinais importantes do ambiente Unix, além do SIGSEGV, SIGUSR1, SIGUSR2, SIGALRM e SIGINT. Quais são suas características e utilidades?

_SIGQUIT_: Acontece quando a tecla de abandono (CRTL+D) é acionada.

_SIGKILL_: "Mata" o programa. 

_SIGSTOP_: Para o processo. Não pode ser ignorada. 

_SIGCHLD_: Quando o status do processo filho muda. 

_SIGPIPE_: Escreve em um pipe quando ele não é aberto em leitura. 

#### 3. Considere o código a seguir:

``` C
#include <signal.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

void tratamento_alarme(int sig)
{
	system("date");
	alarm(1);
}
 
int main()
{
	signal(SIGALRM, tratamento_alarme);
 	alarm(1);
 	printf("Aperte CTRL+C para acabar:\n");
 	while(1);
 	return 0;
}
```

#### Sabendo que a função alarm() tem como entrada a quantidade de segundos para terminar a contagem, quão precisos são os alarmes criados neste código? De onde vem a imprecisão? Este é um método confiável para desenvolver aplicações em tempo real?

A função alarm() espera a quantidade de segundo para determinada instrução acontecer. Não pode ser usada em tempo real pois sua imprecisão não permite isso, já que a contagem pode variar dependendo de um comando ou outro, além de depender de outros fatores. 

