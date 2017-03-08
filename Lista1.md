# Lista de Exercícios 1

### 1) O que são sistemas embarcados?

De acordo com definições vistas em sala de aula, um sistema embarcado é um computador encapsulado e dedicado a um projeto específico. É também uma combinação de hardware e software que tem como função desempenhar um processo maior. 


### 2) O que são sistemas microprocessados?

Parecido com o sistema embarcado, o sistema microprocessado também tem como função processar instruções. Porém, a grande diferença dos dois é de que um tem capacidade de embarcar um sistema operacional, já o outro é retido apenas ao processamento. 


### 3) Apresente aplicações de sistemas embarcados:

*- Para a indústria automotiva:* 

	Em um automóvel, o indicativo de quantidade de gasolina no tanque, o aviso de que o motorista não está com cinto de segurança ou que as portas estão abertas são sistemas embarcados.
	
*- Para eletrodomésticos:*

	Desde indicativos de temperaturas ou outros sensores em diversos equipamentos ou automatizações em processos que antes eram considerados mecânicos. 
	
*- Para automoção industrial:*

	A "inteligência" de robôs que ajudam como transporte ou organização de materiais em grandes empresas é um bom exemplo para sistemas embarcados nessa área. 
	

### 4) Cite arquiteturas possíveis e as diferenças entre elas:

Algumas arquiteturas possíveis são DSP, MPSOC, SOC, FPGA e microcontroladores.

DSP, em tradução livre, Processador Digital de Sinal, são processadores que, como o próprio nome diz, são especializados em processamento digital de sinal, seja em tempo real ou offline. Seu grande diferencial é sua velocidade que os outros microprocessadores e que ele é capaz de eliminar ruídos. 

MPSOC é um tipo de system on chip com vários processadores, e tem uma funcionalidade específica pra um determinado projeto. Assim como basicamente todo sistema embarcado, ele deve seguir uma hierarquia de memória, segurança de rede, desempenho de aplicação e eficiência energética. 

SOC são os componentes de um sistema eletrônico em um circuito integrado. Ele tem entradas e saídas digitais e analógicas. Sua diferença em relação aos microcontroladores é que SOC's são mais potentes e com maior memória, pois tem periféricos aclopados. 

Já a FPGA é uma placa reconfigurável. Ou seja, diferente dos chips que já vem com a programação feita, a FPGA é um tipo de hardware em que suas funcionalidades são definidas pelo projetista, não pelo fabricante. Sua utilização vem ganhando mais força ao longo do tempo e ela está cada vez sendo mais aplicada em projetos. 

Os microcontroladores, como dito anteriormente, são um tipo de SOC porém de potência mais baixa, ou seja, eles não tem tanta capacidade de memória, porém o consumo de energia é relativamente baixo, a dificuldade de utilização também e eles são muito usados para automatização. 


### 5) Por que usamos o Raspberry Pi na disciplina, ao invés de outro system on chip?
Porque o Raspberry Pi tem menor custo quando comparado a outros SOC's, além de atender todas as necessidades que a matéria exige quanto ao conteúdo aplicado, já que tem portas HDMI e USB, é compatível com expansão de memória e barramentos e tem cartão de memória. Além disso, sua produção foi feita para escola e estudantes, então uma de suas caracteristícas é a simplicidade.  
