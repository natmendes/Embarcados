## Lista 9

#### 1. Crie dois programas em C para criar um cliente e um servidor. Faça com que o cliente envie os valores 1, 2, 3, 4, 5, 6, 7, 8, 9 e 10 para o servidor, com intervalos de 1 segundo entre cada envio. Depois de o cliente enviar o número 10, ele aguarda 1 segundo e termina a execução. O servidor escreve na tela cada valor recebido, e quando ele receber o valor 10, ele termina a execução.

Cliente:

``` C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <unistd.h>

int main (int argc, char* const argv[])
{
	char *socket_name;
	char *mensagem;

	int socket_id, i = 1;
	struct sockaddr name;
	int length;

	if (argc < 2)
	{
		puts("   Este programa cria um cliente que se comunica");
		puts("   a um servidor local no caminho especificado");
		puts("   pelo usuario. Para permitir que o cliente comunique-se");
		puts("   com este servidor, o servidor deve ser");
		puts("   executado inicialmente com um caminho definido,");
		puts("   e o cliente devera ser executado em outra");
		puts("   janela ou em outra aba do terminal, utilizando");
		puts("   o mesmo caminho. O servidor escreve na tela");
		puts("   todo texto enviado pelo cliente. Se o cliente");
		puts("   transmitir o texto \"sair\", o servidor se");
		puts("   encerra. Se o usuario pressionar CTRL-C para");
		puts("   o servidor, ele tambem se encerra.");
		puts("   encerra.");
		puts("   Modo de Uso:");
		printf("      %s <caminho_do_socket> <mensagem>\n", argv[0]);
		printf("   Exemplo: %s /tmp/socket1 \"Ola socket\"\n", argv[0]);
		exit(1);
	}
	socket_name = argv[1];
	

	
	
	while (i < 11)
	{
		fprintf(stderr, "Abrindo o socket local... ");
		socket_id = socket(PF_LOCAL, SOCK_STREAM,0);
		fprintf(stderr, "Feito!\n");

		fprintf(stderr, "Conectando o socket ao servidor no endereco local \"%s\"... ", socket_name);
		name.sa_family = AF_LOCAL;
		strcpy(name.sa_data, socket_name);
		connect(socket_id, &name, sizeof(name));
		fprintf(stderr, "Feito!\n");
		
		//mensagem = i;
		fprintf(stderr, "Mandando mensagem ao servidor... ");
		//length = strlen(mensagem) + 1;
		//write(socket_id, &length, sizeof(length));
		write(socket_id, &i, sizeof(int));
		fprintf(stderr, "Feito!\n");
		i++;
		sleep(1);
	}

	fprintf(stderr, "Fechando o socket local... ");
	close(socket_id);
	fprintf(stderr, "Feito!\n");
	return 0;
}
```

Servidor:

``` C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/socket.h>
#include <sys/un.h>
#include <signal.h>


char socket_name[50];
int socket_id;
void sigint_handler(int signum);
void print_client_message(int client_socket);
void end_server(void);

int main (int argc, char* const argv[])
{
	struct sockaddr socket_struct;

	if (argc < 2)
	{
		puts("   Este programa cria um servidor local ");
		puts("   no caminho especificado pelo usuario.");
		puts("   Para permitir que o cliente comunique-se");
		puts("   com este servidor, o servidor deve ser");
		puts("   executado inicialmente com um caminho definido,");
		puts("   e o cliente devera ser executado em outra");
		puts("   janela ou em outra aba do terminal, utilizando");
		puts("   o mesmo caminho. O servidor escreve na tela");
		puts("   todo texto enviado pelo cliente. Se o cliente");
		puts("   transmitir o texto \"sair\", o servidor se");
		puts("   encerra. Se o usuario pressionar CTRL-C,");
		puts("   o servidor tambem se encerra.");
		puts("   encerra.");
		puts("   Modo de Uso:");
		printf("      %s <caminho_do_socket>\n", argv[0]);
		printf("   Exemplo: %s /tmp/socket1\n", argv[0]);
		exit(1);
	}
	else
		strcpy(socket_name, argv[1]);


	fprintf(stderr, "Definindo o tratamento de SIGINT... ");
	signal(SIGINT, sigint_handler);
	fprintf(stderr, "Feito!\n");
	
	fprintf(stderr, "Abrindo o socket local... ");
	socket_id = socket(PF_LOCAL, SOCK_STREAM, 0);
	fprintf(stderr, "Feito!\n");

	fprintf(stderr, "Ligando o socket ao endereco local \"%s\"... ", socket_name);
	socket_struct.sa_family = AF_LOCAL;
	strcpy(socket_struct.sa_data, socket_name);
	bind(socket_id, &socket_struct, sizeof(socket_struct));
	fprintf(stderr, "Feito!\n");

	fprintf(stderr, "Tornando o socket passivo (para virar um servidor)... ");
	listen(socket_id, 10);
	fprintf(stderr, "Feito!\n");

	while(1)
	{
		struct sockaddr cliente;
		int socket_id_cliente;
		socklen_t cliente_len;

		fprintf(stderr, "Aguardando a conexao de um cliente... ");
		socket_id_cliente = accept(socket_id, &cliente, &cliente_len);
		fprintf(stderr, "Feito!\n");

		fprintf(stderr, "Obtendo a informacao transmitida pelo cliente...");
		print_client_message(socket_id_cliente);
		fprintf(stderr, "Feito!\n");

		fprintf(stderr, "Fechando a conexao com o cliente... ");
		close(socket_id_cliente);
		fprintf(stderr, "Feito!\n");
	}
	return 0;
}

void sigint_handler(int signum)
{
	fprintf(stderr, "\nRecebido o sinal CTRL+C... vamos desligar o servidor!\n");
	end_server();
}

void print_client_message(int client_socket)
{
	int length, i;
	//fprintf(stderr, "\nMensagem enviada pelo cliente tem ");
	//read(client_socket, &length, sizeof (length));
	//fprintf(stderr, "%d bytes.", length);
	//text = (char*) malloc (length);
	read(client_socket, &i, sizeof(int));
	fprintf(stderr,"\n\n   Mensagem = %d\n\n", i);
	//free (text);
	//if (!strcmp (text, "sair"))
	//{
	//	fprintf(stderr, "Cliente pediu para o servidor fechar.\n");
	//	end_server();
	//}
}

void end_server(void)
{
	fprintf(stderr, "Apagando \"%s\" do sistema... ", socket_name);
	unlink(socket_name);
	fprintf(stderr, "Feito!\n");
	fprintf(stderr, "Fechando o socket local... ");
	close(socket_id);
	fprintf(stderr, "Feito!\n");
exit(0);
}
```
