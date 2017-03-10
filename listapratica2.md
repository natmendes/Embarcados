## Lista Prática 2


# 1. Escreva o texto "Olá mundo cruel!" em um arquivo denominado ola_mundo.txt e apresente o conteúdo desse arquivo no terminal.    
echo Ola mundo cruel! > ola_mundo.txt 

# 2. Apresente o nome de todos os arquivos e pastas na pasta root.   
su root; cd ../..; cd root; 
ls

# 3. Apresente o tipo de todos os arquivos e pastas na pasta root.   
cd ../..; cd root; ls --file-type

# 4. Apresente somente as pastas dentro da pasta 'root'.  
cd ../..; cd root; ls -D

# 5. Descubra em que dia da semana caiu o seu aniversário nos últimos dez anos.  
for i in {2007..2017}; do date -d '18 Jan '$i; done

# 6. Liste somente os arquivos com extensão .txt.  
ls *.txt

# 7. Liste somente os arquivos com extensão .png.  
ls *.png

# 8. Liste somente os arquivos com extensão .jpg.  
ls *.jpg

# 9. Liste somente os arquivos com extensão .gif.  
ls *.gif

# 10. Liste somente os arquivos que contenham o nome 'cal'.  
ls \*cal*

# 11. Liste somente os arquivos que contenham o nome 'tux'.  
ls \*tux*

# 12. Liste somente os arquivos que comecem com o nome 'tux'.  
ls tux*
