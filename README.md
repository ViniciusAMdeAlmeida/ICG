# TRABALHO I - RASTERIZAÇÃO
###### Disciplina: [GDSCO0051] Introdução à Computação Gráfica - Turma 01. 
###### Professor: Christian Azambuja Pagot (email: christian@ci.ufpb.br).
###### Dupla: Altamir Marconi da Silva Filho – 20170002856
######      Vinícius Amaral Monteiro de ALmeida - 20180002250

  Foi proposto pelo professor Christian Azambuja Pagot, da disciplina de Introdução à Computação Gráfica,  que implementássemos em uma Framework criada pelo professor um programa que simulasse o processo de Rasterização manualmente.

### Introdução

   A Rasterização é um processo que converte imagens vetoriais(curvas funcionais) em uma imagem raster(pixel ou pontos). Para que pudéssemos implementar esse processo de forma manual teríamos de escrever direto na memoria, algo que não é permitido pelos sistemas operacionais atuais.
   
   Entretanto, a Framework criada escrita em C++ simula o acesso direto a memória, nos cabendo a necessariamente implementar as funções para a rasterização. As funções a serem implementadas serão 3: putPixel(), drawLine() e a drawTriangle().
   
   As implementações foram feitas apenas nos arquivos:
* main.cpp: Onde as funções são chamadas.
* mygl.h: Contém as funções responsáveis pelas implementações gráficas do programa.


