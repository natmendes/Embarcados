### Lista Teórica 2

## 1. Por que o Linux recebeu esse nome?  
Pois é a união dos nomes Linus e Unix. Linus é o nome do criador, que é Linus Torvalds. Já o Unix é o nome de um sistema operacional de grande porte. 

## 2. O que são daemons?  
Basicamente, um daemon é um programa que roda processos em segundo plano. Geralmente, os sistemas inicializam os daemons no momento do boot, tais como as atividades de hardware.

## 3. O que é o shell?
Shell é o terminal do Linux. 

## 4. Por que é importante evitar executar o terminal como super-usuário?  
Pois é "simples" remover e alterar arquivos diretamente do terminal. E, como super-usuário, pode-se ter acesso a todos os arquivos do computador, inclusive com todos as pastas importantes para o funcionamento do sistema operacional. Logo, se não usado com muita atenção, pode-se apagar algo importante no computador. 

## 5. Qual botão do teclado completa o que o usuário escreve no terminal, de acordo com o contexto?  
Tab. 

## 6. Quais botões do teclado apresentam instruções escritas anteriormente?  
As setas para cima e para baixo. 

## 7. Apresente os respectivos comandos no terminal para:  
*a) Obter mais informações sobre um comando.*
man \[comando] ou \[comando] --help
*b) Apresentar uma lista com os arquivos dentro de uma pasta.*
ls
*c) Apresentar o caminho completo da pasta.*
pwd
*d) Trocar de pasta.*
cd \[pasta desejada]
*e) Criar uma pasta.*
mkdir \[nome da pasta]
*f) Apagar arquivos definitivamente.*
Se quiser apagar com a possibilidade de recuperação, rm. Se for para ser verdadeiramente irrecuperável, shred.
*g) Apagar pastas definitivamente.*
rm -R \[pasta desejada]
*h) Copiar arquivos.*
cp \[nome do arquivo] \[diretório que deseja colocar a cópia]
*i) Copiar pastas.*
cp -a \[pasta que deseja copiar] \[lugar que deseja colar]
*j) Mover arquivos.*
mv \[nome do arquivo] \[local do destino]
*k) Mover pastas.*
mv -a \[pasta que deseja mover] \[local do destino]
*l) Renomear pastas.*
mv \[nome antigo] \[novo nome]
*m) Apresentar o conteúdo de um arquivo.*
ls
*n) Apresentar o tipo de um arquivo.*
ls --file-type
*o) Limpar a tela do terminal.*
clear
*p) Encontrar ocorrências de palavras-chave em um arquivo-texto.*
cat arquivo-texto.txt | grep \[palavra-chave]
*q) Ordenar informações em um arquivo-texto.*
sort \[arquivo-texto]
*r) Substituir ocorrências de palavras-chave em um arquivo-texto.*
sed -i 's/\[nova palavra]/\[palavra que quer substituir]/g'
*s) Conferir se dois arquivos são iguais.*
diff \[arquivo1] \[arquivo2]
*t) Escrever algo na tela.*
echo \[o que deseja escrever] 
