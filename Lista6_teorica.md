## Lista 6 - Teórica

#### 1. Como se utiliza o comando 'ps' para:

_(a) Mostrar todos os processos rodando na máquina?_

ps aux

_(b) Mostrar os processos de um usuário?_

ps -aux, onde x é o nome do usuário desejado. 

_(c) Ordenar todos os processos de acordo com o uso da CPU?_

ps aux | sort -nrk 3,3 

_(d) Mostrar a quanto tempo cada processo está rodando?_

ps aux

#### 2. De onde vem o nome fork()?

Fork, nesse contexto, significa forquilha, ou bifurcação, pois divide um processo em dois. 

#### 3. Quais são as vantagens e desvantagens em utilizar:

_(a) system()?_

A função system se encontra na biblioteca stdlib.h e faz uma chamada dentro de um programa pra um sistema, criando um sub-processo. O problema é que ela depende do sistema, ou seja, se o sistema demorar para executar ou então apresentar algum erro, isso influenciará todo programa. 

_(b) fork() e exec()?_

A desvantagem é que não existe só uma função para criar e executar o programa de uma só vez, logo precisam de duas funções. A vantagem é de que ele não depende do sistema como o outro. 

#### 4. É possível utilizar o exec() sem executar o fork() antes?

Sim, porém não faz sentido pois sem o fork para copiar o código, executar pelo exec é o mesmo que executar normalmente o programa inteiro, porque será só um programa para ser executado, não dois ou mais. 

#### 5. Quais são as características básicas das seguintes funções:

_(a) execp()?_

O nome do programa está no diretório atual, logo não é necessário mostrar o caminho completo do programa. 

_(b) execv()?_

Aceita que a lista de argumentos do novo programa seja nula. 

_(c) exece()?_

Aceita um argumento adicional. 

_(d) execvp()?_

O programa está no diretório atual e a lista de argumentos pode ser nula. 

_(e) execve()?_

Aceita argumentos adicionais. 

_(f) execle()?_

Aceita mecanismos como van args em sua lista de argumentos. 


