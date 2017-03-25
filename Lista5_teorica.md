## Lista 5

#### 1. Considerando a biblioteca-padrão da linguagem C, responda:

_(a) Quais são as funções (e seus protótipos) para abrir e fechar arquivos?_

Para abrir: fopen(arquivo.txt, flag\_para\_indicar\_a_ação);

Essa flag pode ser "r" (somente leitura), "w" (somente escrita) e "a" (leitura e escrita sem sobrescrever o arquivo).

Para fechar: fclose(FILE *fp).

_(b) Quais são as funções (e seus protótipos) para escrever em arquivos?_

Existem várias, como fputs(variável, FILE *fp) ou fprintf(FILE *fp, "%s, %d, %f..", variável). 

_(c) Quais são as funções (e seus protótipos) para ler arquivos?_

fscanf(FILE *fp, "%s, %d, %f..", variável) ou fread(endereço\_da\_variável, num\_bytes, num\_caracteres, FILE *fp). 

_(d) Quais são as funções (e seus protótipos) para reposicionar um ponteiro para arquivo?_

fseek(FILE *fp, num\_bytes\_a\_partir\_da\_constante, constante). Essa constante pode ser SEEK_SET (início do arquivo), SEEK\_CUR (ponto atual do ponteiro) e SEEK\_END (final do arquivo). 

_(e) Quais bibliotecas devem ser incluídas no código para poder utilizar as funções acima?_

stdio.h e stdlib.h. 

#### 2. O que é a norma POSIX?

Essa norma POSIX foi definida pelo IEEE e tem como intuito a compatibilidade entre sistemas operacionais, seja variantes de Unix ou outros sistemas. Assim, será necessário programar apenas uma vez para diferentes sistemas operacionais.

#### 3. Considerando a norma POSIX, responda:

_Quais são as funções (e seus protótipos) para abrir e fechar arquivos?_

Para abrir: int retorno = open(arquivo.txt, FLAG);

Essa flag pode ser "O\_RDONLY" (somente leitura), "O\_WRONLY" (somente escrita), "O\_RDWR" (leitura e escrita), "O\_APPEND" (leitura e escrita sem sobrescrever o arquivo), "O\_CREAT" (criar o arquivo se ele não existir) e "S_IRWXU" (permite leitura, escrita e execução do arquivo). 

Para fechar: close(int fp).

_(b) Quais são as funções (e seus protótipos) para escrever em arquivos?_

int retorno = write(int fp, endereço\_da\_variável, num\_bytes\_para\_escrever).

Esses números bytes para escrever geralmente é dado por (quantidade de bytes)*sizeof(tipo de variável). 

_(c) Quais são as funções (e seus protótipos) para ler arquivos?_

int retorno =  read(int fp, endereço\_da\_variável, num\_bytes\_para\_ler).

Esses números bytes para ler geralmente é dado por (quantidade de bytes)*sizeof(tipo de variável).

_(d) Quais são as funções (e seus protótipos) para reposicionar um ponteiro para arquivo?_

lseek(int fp, num\_bytes\_a\_partir\_da\_constante, constante). Essa constante pode ser SEEK_SET (início do arquivo), SEEK\_CUR (ponto atual do ponteiro) e SEEK\_END (final do arquivo).

_(e) Quais bibliotecas devem ser incluídas no código para poder utilizar as funções acima?_

stdio.h, stdlib.h, fcntl.h e unistd.h.




