## Lista Prática 3


#### 1. Crie um "Olá mundo!" em C.

''' C
#include <stdio.h> 

int main()
{
	printf("Olá mundo!\n");
} 
'''

#### 2. Crie um código em C que pergunta ao usuário o seu nome, e imprime no terminal "Ola " e o nome do usuário.

''' C
#include <stdio.h> 
#include <stdlib.h>

int main ()
{
	char nome[100];
	printf("Digite seu nome:\n"); 
	scanf("%s", nome); 
	printf("Ola %s", nome); 
	return 0; 
} 
'''

#### 3. Apresente os comportamentos do código anterior nos seguintes casos:

_(a) Se o usuário insere mais de um nome._

Se for inserido 'Eu mesmo', só aparece o primeiro nome, 'Olá Eu'. 

_(b) Se o usuário insere mais de um nome entre aspas._

Aparece 'Olá "Eu'.

_(c) Se é usado um pipe._

Aparece 'Olá Eu'. 

_(d) Se é usado um pipe com mais de um nome._

Aparece apenas 'Olá Eu'.

_(e) Se é usado um pipe com mais de um nome entre aspas._ 

Aparece apenas 'Olá Eu'.

_(f) Se é usado o redirecionamento de arquivo._

Aparece apenas 'Olá ola'.

#### 4. Crie um código em C que recebe o nome do usuário como um argumento de entrada (usando as variáveis argc e *argv[]), e imprime no terminal "Ola " e o nome do usuário. 

''' C
#include <stdio.h> 
#include <stdlib.h>

int main (int argc, char **argv)
{
	printf("Olá %s!\n", argv[1]);
	return 0;
}
''' 

#### 5. Apresente os comportamentos do código anterior nos seguintes casos:

_(a) Se o usuário insere mais de um nome._

Se for inserido 'Eu mesmo', só aparece o primeiro nome, 'Olá Eu'. 

_(b) Se o usuário insere mais de um nome entre aspas._

Aparece 'Olá Eu mesmo'.

_(c) Se é usado um pipe._

Aparece apenas 'Olá (null)'. 

_(d) Se é usado um pipe com mais de um nome._

Aparece apenas 'Olá (null)'.

_(e) Se é usado um pipe com mais de um nome entre aspas._ 

Aparece apenas 'Olá (null)'.

_(f) Se é usado o redirecionamento de arquivo._

Aparece apenas 'Olá (null)'.

#### 6.Crie um código em C que faz o mesmo que o código da questão 4, e em seguida imprime no terminal quantos valores de entrada foram fornecidos pelo usuário.  

''' C
#include <stdio.h> 
#include <stdlib.h>

int main (int argc, char **argv)
{
	printf("Olá %s!\n", argv[1]);
	printf("Quantidade de argumentos de entrada: %d\n", argc);
	return 0;
}
''' 

#### 7. Crie um código em C que imprime todos os argumentos de entrada fornecidos pelo usuário. 

''' C
#include <stdio.h> 
#include <stdlib.h>

int main ()
{
	char nome[100];
	printf("Digite seu nome:\n"); 
	scanf("%[^\n]", nome); 
	printf("Ola %s\n", nome);
	return 0;
	
}
''' 

#### 8. Crie uma função que retorna a quantidade de caracteres em uma string, usando o seguinte protótipo:

#### int Num_Caracs(char *string);

#### Salve-a em um arquivo separado chamado 'num_caracs.c'. Salve o protótipo em um arquivo chamado 'num_caracs.h'. Compile 'num_caracs.c' para gerar o objeto 'num_caracs.o'.

''' C
#include <stdio.h> 
#include <stdlib.h>
#include <string.h>
#include "num_caracs_bib.h"

int num_caracs (char *string)
{
	int tamanho;
	tamanho = strlen(string);
	return tamanho; 
} 
'''

''' C
#include <stdio.h> 

int num_caracs (char *string); 
'''

#### 9. Re-utilize o objeto criado na questão 8 para criar um código que imprime cada argumento de entrada e a quantidade de caracteres de cada um desses argumentos. 

''' C
#include <stdio.h>
#include <stdlib.h> 
#include <string.h> 
#include "num_caracs_bib.h"


int main (int argc, char **argv)
{
	int i = 0, comparar;
	comparar = argc;
	while (comparar != 0)
	{	
	printf("Argumento: %s\n", argv[i]);
	printf("Número de caracteres: %d\n", num_caracs (argv[i]));
	comparar--; 
	i++; 
	}
	
	return 0;
	
}
'''

#### 10. Crie um Makefile para a questão anterior. 

''' C
exec_num_caracs: questao9.o num_caracs.o
	gcc $(CFLAGS) -o exec_num_caracs questao9.o num_caracs.o
questao9.o: questao9.c num_caracs_bib.h
	gcc $(CFLAGS) -c questao9.c
num_caracs.o: num_caracs.c num_caracs_bib.h
	gcc $(CFLAGS) -c num_caracs.c
clean:
	rm -f *.o num_caracs questao9
'''
	
#### 11. Re-utilize o objeto criado na questão 8 para criar um código que imprime o total de caracteres nos argumentos de entrada. 

''' C
#include <stdio.h>
#include <stdlib.h> 
#include <string.h> 
#include "num_caracs_bib.h"


int main (int argc, char **argv)
{
	int i = 0, comparar, num_caracs_total=0;
	comparar = argc;
	while (comparar != 0)
	{	
		num_caracs_total += num_caracs (argv[i]);
		comparar--; 
		i++; 
	}
	
	printf("Número de caracteres: %d\n", num_caracs_total);
	
	return 0;
	
}
'''

#### 12. Crie um Makefile para a questão anterior.

''' C
exec_num_caracs: questao10.o num_caracs.o
	gcc $(CFLAGS) -o exec_num_caracs questao10.o num_caracs.o
questao9.o: questao10.c num_caracs_bib.h
	gcc $(CFLAGS) -c questao10.c
num_caracs.o: num_caracs.c num_caracs_bib.h
	gcc $(CFLAGS) -c num_caracs.c
clean:
	rm -f *.o num_caracs questao10
'''
	
	
