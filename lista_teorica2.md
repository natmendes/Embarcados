## Lista Teórica 2

# 1. Por que o Linux recebeu esse nome?  
Pois é a união dos nomes Linus e Unix. Linus é o nome do criador, que é Linus Torvalds. Já o Unix é o nome de um sistema operacional de grande porte. 

# 2. O que são daemons?  
Basicamente, um daemon é um programa que roda processos em segundo plano. Geralmente, os sistemas inicializam os daemons no momento do boot, tais como as atividades de hardware.

# 3. O que é o shell?
Shell é o terminal do Linux. 

# 4. Por que é importante evitar executar o terminal como super-usuário?  
Pois é "simples" remover e alterar arquivos diretamente do terminal. E, como super-usuário, pode-se ter acesso a todos os arquivos do computador, inclusive com todos as pastas importantes para o funcionamento do sistema operacional. Logo, se não usado com muita atenção, pode-se apagar algo importante no computador. 

# 5. Qual botão do teclado completa o que o usuário escreve no terminal, de acordo com o contexto?  
Tab. 

# 6. Quais botões do teclado apresentam instruções escritas anteriormente?  
As setas para cima e para baixo. 

# 7. Apresente os respectivos comandos no terminal para:  
_a) Obter mais informações sobre um comando._  
man \[comando] ou \[comando] --help
_b) Apresentar uma lista com os arquivos dentro de uma pasta._  
ls
_c) Apresentar o caminho completo da pasta._  
pwd
_d) Trocar de pasta._  
cd \[pasta desejada]
_e) Criar uma pasta._  
mkdir \[nome da pasta]
_f) Apagar arquivos definitivamente._  
Se quiser apagar com a possibilidade de recuperação, rm. Se for para ser verdadeiramente irrecuperável, shred.
_g) Apagar pastas definitivamente._  
rm -R \[pasta desejada]
_h) Copiar arquivos._  
cp \[nome do arquivo] \[diretório que deseja colocar a cópia]
_i) Copiar pastas._  
cp -a \[pasta que deseja copiar] \[lugar que deseja colar]
_j) Mover arquivos._  
mv \[nome do arquivo] \[local do destino]
_k) Mover pastas._  
mv -a \[pasta que deseja mover] \[local do destino]
_l) Renomear pastas._  
mv \[nome antigo] \[novo nome]
_m) Apresentar o conteúdo de um arquivo._  
ls
_n) Apresentar o tipo de um arquivo._  
ls --file-type
_o) Limpar a tela do terminal._  
clear
_p) Encontrar ocorrências de palavras-chave em um arquivo-texto._  
cat arquivo-texto.txt | grep \[palavra-chave]
_q) Ordenar informações em um arquivo-texto._  
sort \[arquivo-texto]
_r) Substituir ocorrências de palavras-chave em um arquivo-texto._  
sed -i 's/\[nova palavra]/\[palavra que quer substituir]/g'
_s) Conferir se dois arquivos são iguais._  
diff \[arquivo1] \[arquivo2]
_t) Escrever algo na tela._  
echo \[o que deseja escrever] 
