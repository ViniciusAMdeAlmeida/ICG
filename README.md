# TRABALHO I - RASTERIZAÇÃO
###### Disciplina: [GDSCO0051] Introdução à Computação Gráfica - Turma 01. 
###### Professor: Christian Azambuja Pagot (email: christian@ci.ufpb.br).
###### Dupla: Altamir Marconi da Silva Filho – 20170002856
######      Vinícius Amaral Monteiro de Almeida - 20180002250

  Foi proposto pelo professor Christian Azambuja Pagot, da disciplina de Introdução à Computação Gráfica,  que implementássemos em uma Framework criada pelo professor um programa que simulasse o processo de Rasterização manualmente.

## Introdução

   A Rasterização é um processo que converte imagens vetoriais(curvas funcionais) em uma imagem raster(pixel ou pontos). Para que pudéssemos implementar esse processo de forma manual teríamos de escrever direto na memoria, algo que não é permitido pelos sistemas operacionais atuais.
   
   Entretanto, a Framework criada escrita em C++ simula o acesso direto a memória, nos cabendo a necessariamente implementar as funções para a rasterização. As funções a serem implementadas serão 3: putPixel(), drawLine() e a drawTriangle().
   
   As implementações foram feitas apenas nos arquivos:
* main.cpp: Onde as funções são chamadas.
* mygl.h: Contém as funções responsáveis pelas implementações gráficas do programa.

## Função PutPixel()

![image](https://user-images.githubusercontent.com/52431296/61233148-3dcd0700-a706-11e9-91f1-4a65278b760c.png)

  A função putPixel() é responsável por rasterizar um ponto na memória de vídeo, ela recebe com parâmetro a posição na tela representada por (X, Y) e uma estrutura que representará a cor do ponto, contendo os componentes RGBA. Essa função permitirá que as outras funções possam usá-la em futuras operações.
  
![image](https://user-images.githubusercontent.com/52431296/61233315-aae09c80-a706-11e9-9475-ac6470663ee2.png)

  Cada pixel possui 4 componentes de cor (RGBA), cada uma representada por 1 byte (unsigned char), no programa usaremos o ponteiro FBptr que já é predefinida pelo professor que aponta para o início da memória de vídeo(no caso a primeira posição da memória de vídeo apontada seria o pixel (X, Y) = (0, 0) corresponde ao canto superior esquerdo da tela).
  
  Para utilizarmos os pixels teremos de calcular cada byte que precisaremos utilizar, para isso basta apenas achar o 1° dos 4 bytes de cada pixel (o do elemento R), os outros se localizam um byte a frente (acréscimo de 1) do anterior.
  
  ![image](https://user-images.githubusercontent.com/52431296/61233353-c350b700-a706-11e9-968d-e6d920872883.png)
  
  Logo, para realizar o cálculo primeiro testamos se o pixel apontado respeita as delimitações da tela representada pela variável IMAGE_WIDTH que corresponde à altura da tela e a IMAGE_HEIGHT que corresponde a largura, ambas declaradas em definitions.h. Levando em conta que a o número de pixels na tela é dado pela multiplicação de número de linhas pelo número de colunas, a memória deve ter esta mesma quantidade de posições para representar a tela, só que de forma linear.
  
  Para isso usaremos um algoritmo, que decide a posição de cada pixel e cada canal de um pixel na memória, considerando largura em pixels da tela:
  
  ![image](https://user-images.githubusercontent.com/52431296/61233394-dbc0d180-a706-11e9-9341-679e76f20415.png)

  Onde W é a largura da imagem, ou seja, IMAGE_HEIGTH.
  
  ![image](https://user-images.githubusercontent.com/52431296/61233431-eaa78400-a706-11e9-89f9-f01658ce0178.png)
  
  Por fim, após o calculo que identifica o byte, atribuímos a ele a cor definida e assim rasterizando o ponto na tela. Com todas essa definições o nosso código ficará da seguinte maneira:
  
  ![image](https://user-images.githubusercontent.com/52431296/61233453-fa26cd00-a706-11e9-81a6-14be464e7318.png)
  
  Resultado: Pixel na posição (150,150):
  
  ![image](https://user-images.githubusercontent.com/52431296/61233480-0ca10680-a707-11e9-8b48-58d494184806.png)
  
## Função DrawLine()

  Para a função drawLine(), devemos falar um pouco sobre o Algoritmo de Bresenham, principal componente que será usado na próxima função. 
  
  Esse algoritmo é usado para rasterizar linhas, sendo o mais utilizado para esse objetivo,  principalmente pelo seu baixo valor computacional. E o que ele faz é decidir quais pontos irão fazer parte ou não das retas, através do ponto médio, usando apenas operações aritméticas de inteiros de forma incremental, resultando em um ganho de processamento.
  
  ![image](https://user-images.githubusercontent.com/52431296/61233649-65709f00-a707-11e9-87ea-fc1f3590239d.png)
  
  Abaixo está a função drawLine() com  o Algoritmo de Bresenham, considerando x0 e y0 como as coordenadas iniciais e x1 e y1 como as coordenadas finais.
  
  ![image](https://user-images.githubusercontent.com/52431296/61233676-74575180-a707-11e9-8e5c-33c2f3d32e0b.png)

  Entretanto, esse algoritmo só funciona para linhas no primeiro octante(coeficiente angular entre 0 e 1). E para implementar os outros octantes, é necessário verificar em qual quadrante a linha está, com base nas relações entre dx e dy, e a partir disso alterar o algoritmo de Bresenham para que ele funcione em qualquer octante.
  
  ![image](https://user-images.githubusercontent.com/52431296/61233693-83d69a80-a707-11e9-80c8-3eb2dcd2b07c.png)
  
  Resultado:
  
  ![image](https://user-images.githubusercontent.com/52431296/61233726-99e45b00-a707-11e9-924d-e02d89af62fc.png)

  ![image](https://user-images.githubusercontent.com/52431296/61233748-a963a400-a707-11e9-9b02-af88dca316bd.png)
  
  ## Interpolação linear
  
  Para realizar a mudança gradativa da cor da reta, a função drawLine() irá receber dois parâmetros de cor, e ao longo da reta irá variar a sua cor conforme ela se aproxima da outra extremidade.
  
  Para isso, calculamos uma taxa de incremento dividindo cada componente RGBA da cor em questão pelo dx ou dy, dependendo da inclinação da reta, e a partir disso, incrementar em essa taxa em cada componente RGBA ao longo da reta, fazendo com que cada pixel pintado terá uma cor diferente, dando esse efeito de gradiente.
  
  Resultado:

![image](https://user-images.githubusercontent.com/52431296/61233816-d912ac00-a707-11e9-81dd-4a3fde6d5d93.png)

![image](https://user-images.githubusercontent.com/52431296/61233830-e039ba00-a707-11e9-8052-38f9d5593c16.png)

## Função DrawTriangle()

  Para rasterizar um triângulo, utilizaremos a função drawTriangle(), que  recebe 3 coordenadas de pontos e 3 cores. A partir disso a função drawLine() é chamada 3 vezes, fazendo uma reta do primeiro ao segundo ponto, do segundo ao terceiro, e do terceiro ao primeiro.
  
  ![image](https://user-images.githubusercontent.com/52431296/61233902-0e1efe80-a708-11e9-8f83-d5949df97a04.png)

  Resultado:
  
  ![image](https://user-images.githubusercontent.com/52431296/61233921-1bd48400-a708-11e9-9acc-fe2ba0a82b7a.png)
  
## Principais dificuldades

  A principal dificuldade que encontramos foi, sem dúvida, entender o funcionamento do algoritmo de Bresenham e adaptá-lo para os outros octantes.
  
## Referências bibliográficas

  *https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm
  *https://github.com/ThiagoLuizNunes/CG-Assignments/tree/master/cg_framework
  *Material de sala de aula do professor Christian Pagot.





  
  




  
  
