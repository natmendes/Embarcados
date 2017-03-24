## Lista 4

#### 1. Crie um código em C para escrever "Ola mundo!" em um arquivo chamado 'ola_mundo.txt'.

``` C
#include <stdio.h>	
#include <fcntl.h>	
#include <unistd.h>	
#include <stdlib.h>	
#include <string.h>

int main()
{
	int fp;
	char escrita[13] = {"Olá mundo!\n"};
	fp = open ("questao1.txt", O_WRONLY | O_CREAT, S_IRWXU); 
	if (fp == -1)
	{
		printf("Não foi possível abrir o arquivo!"); 
		exit (0); 
	}
	int escrever = write(fp, escrita, strlen(escrita)*sizeof(char));

	close(fp); 
	return 0; 
}
```

#### 2. Crie um código em C que pergunta ao usuário seu nome e sua idade, e escreve este conteúdo em um arquivo com o seu nome e extensão '.txt'.

``` C
#include <stdio.h>	
#include <fcntl.h>	
#include <unistd.h>	
#include <stdlib.h>	
#include <string.h>

int main()
{
	int fp;
	char string[100], string_txt[104], idade[4];

	printf("Digite seu nome: \n");
	scanf("%s", string); 
	printf("Digite sua idade: \n"); 
	scanf("%s", idade); 

	sprintf(string_txt, "%s.txt", string); 
	
	fp = open (string_txt, O_WRONLY | O_CREAT, S_IRWXU); 
	if (fp == -1)
	{
		printf("Não foi possível abrir o arquivo!"); 
		exit (0); 
	}
	
	write(fp, "Nome: ", 6); 
	int escrever1 = write(fp, string, strlen(string)*sizeof(char));
	write(fp, "\n", 1);
	write(fp, "Idade: ", 7);
	int escrever2 = write(fp, idade, strlen(idade)*sizeof(char));	
	write(fp, "\n", 1);

	close(fp); 
	return 0; 
}
``` 

#### 3. Crie um código em C que recebe o nome do usuário e e sua idade como argumentos de entrada (usando as variáveis argc e *argv[]), e escreve este conteúdo em um arquivo com o seu nome e extensão '.txt'.

``` C
#include <stdio.h>	
#include <fcntl.h>	
#include <unistd.h>	
#include <stdlib.h>	
#include <string.h>

int main(int argc, char *argv[])
{
	int fp;
	char string_txt[100];
		
	sprintf(string_txt, "%s.txt", argv[1]); 
	
	fp = open (string_txt, O_WRONLY | O_CREAT, S_IRWXU); 
	if (fp == -1)
	{
		printf("Não foi possível abrir o arquivo!"); 
		exit (0); 
	}
	
	write(fp, "Nome: ", 6);
	int retorno1 = write(fp, argv[1], strlen(argv[1])*sizeof(char));
	write(fp, "\n", 1);
	write(fp, "Idade: ", 7);
	int retorno2 = write(fp, argv[2], strlen(argv[2])*sizeof(char));	
	write(fp, "\n", 1);

	close(fp); 
	return 0; 
}
``` 

#### 4. Crie uma função que retorna o tamanho de um arquivo, usando o seguinte protótipo:

#### int tam_arq_texto(char *nome_arquivo);

#### Salve esta função em um arquivo separado chamado 'bib_arqs.c'. Salve o protótipo em um arquivo chamado 'bib_arqs.h'. Compile 'bib_arqs.c' para gerar o objeto 'bib_arqs.o'.

``` C
#include "bib_arqs.h"

int tam_arq_texto(int nome_arquivo) 
{
    int tamanho, atual_posicao = lseek(nome_arquivo, 0, SEEK_CUR);
  
    lseek(nome_arquivo, 0, SEEK_END);
    tamanho = lseek(nome_arquivo, 0, SEEK_CUR);
    lseek(nome_arquivo, atual_posicao, SEEK_SET);
 
    return tamanho;
}
``` 

``` C
#ifndef BIB_ARQS_H
#define BIB_ARQS_H

#include <stdio.h>
#include <stdlib.h>

int tam\_arq\_texto(int nome_arquivo);
void le\_arq\_texto(char *nome_arquivo, char *conteudo);

#endif
```

#### 5. Crie uma função que lê o conteúdo de um arquivo-texto e o guarda em uma string, usando o seguinte protótipo:

#### void le_arq_texto(char *nome_arquivo, char *conteúdo);

#### Repare que o conteúdo do arquivo é armazenado no vetor 'conteudo[]'. Ou seja, ele é passado por referência. Salve esta função no mesmo arquivo da questão 4, chamado 'bib_arqs.c'. Salve o protótipo no arquivo 'bib_arqs.h'. Compile 'bib_arqs.c' novamente para gerar o objeto 'bib_arqs.o'.

``` C
void le_arq_texto(char *nome_arquivo, char *conteudo)
{
	int fp, i = 0;
	char c; 
	fp = open (nome_arquivo, O_RDONLY | O_CREAT, S_IRWXU);
	lseek(fp, 0, SEEK_SET); 
	while ((c = getc(fp)) != EOF)
	{
		conteudo[i] = c;
		i++; 
	}
	
}
```

#### 6.Crie um código em C que copia a funcionalidade básica do comando 'cat': escrever o conteúdo de um arquivo-texto no terminal. Reaproveite as funções já criadas nas questões anteriores. 

``` C
#include <stdio.h>	
#include <fcntl.h>	
#include <unistd.h>	
#include <stdlib.h>	
#include <string.h>

int main (int argc, char **argv)
{
	int fp;
	char conteudo[100000], c;
	int i = 0; 
	if (argv[1] == NULL)
		printf("Não foi indicado o arquivo.\n");
	else {
	fp = open (argv[1], O_RDONLY | O_CREAT, S_IRWXU);
	lseek(fp, 0, SEEK_SET); 
	while ((c = getc(fp)) != EOF)
	{
		conteudo[i] = c;
		i++; 
	}
	
	printf("%s", conteudo);}
	close(fp); 
	
}
```

#### 7. Crie um código em C que conta a ocorrência de uma palavra-chave em um arquivo-texto, e escreve o resultado no terminal. Reaproveite as funções já criadas nas questões anteriores.

``` C
#include <stdio.h>	
#include <fcntl.h>	
#include <unistd.h>	
#include <stdlib.h>	
#include <string.h>

int main (int argc, char **argv)
{
	int fp;
	char palavra[100];
	int i = 0; 
	if (argv[1] == NULL)
		printf("Não foi indicado a palavra.\n");
	else if (argv[2] == NULL)
		printf("Não foi indicado o arquivo.\n");
	else {
	fp = open (argv[2], O_RDONLY | O_CREAT, S_IRWXU);
	lseek(fp, 0, SEEK_SET); 
	while (!feof(fp))
	{
		read(fp, palavra, strlen(palavra)*sizeof(char));
		if (strcmp(palavra, argv[1]) == 0)
			i++; 
	}
	
	printf("'%s' ocorreu %d vezes no arquivo '%s'\n", argv[1], i, argv[2]);}
	
}
```



