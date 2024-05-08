# Comparação de otimização do K-means para a classificação de regiões de acordo com a taxa de poluição utilizando OpenMP e MPI
Integrantes:\ Lucas Fiori Magalhaes Machado\
             Rafael Ferreira Garcia Almeida\
             Samuel Spineli Rodrigues
# ⚙️ Como executar
Para compilar o programa no Parcode é necessário utilizar o comando mpic++ -std=c++11 -o polutionkmeans pollutionkmeans.cpp -fopenmp, observe que é necessário utilizar a clausula de comando -std=c++11, pois o compilador do Parcode está em uma versão desatualizada de 2011, o -fopenmp é para a utilização do openmp enquanto o mpic++ é para a utilização do MPI.\
Para executar o programa é necessário usar o comando  mpiexec -n 2 ./pollutionkmeans, onde o número é a quantidade de processos a serem executados.

# Como funciona
O programa conta com funções pertinentes para a execução do K-means (euclideanDistance, initializeCentroids, assignSamplesToClusters, updateCentroids, kmeans) e são nelas onde o OpenMP e o MPI são utilizados, afim de conseguir o maior desempenho possível no processamento dos dados. 

O K-means funciona da seguinte forma:

euclideanDistance: Calcula a distância euclidiana entre duas amostras para determinar sua semelhança (no caso, o nível de poluição).

initializeCentroids: Inicializa os centróides dos clusters com base em amostras aleatórias do conjunto de dados.

assignSamplesToClusters: Atribui cada amostra ao cluster mais próximo com base na distância euclidiana.

updateCentroids: Recalcula os centróides de cada cluster com base nas amostras atribuídas, calculando a média das amostras em cada cluster.

kmeans: Função principal que itera sobre o processo de atribuição e atualização dos centróides até que uma convergência seja alcançada ou um número máximo de iterações seja atingido.

# Funções paralelizadas

euclideanDistance: Determina a distância euclidiana entre duas amostras.

initializeCentroids: Inicializa os centróides utilizando índices com valores aleatórios.

assignSamplesToClusters: Inicializa amostras para cada cluster, utilizamos o OpenMP aqui para que cada amostra seja processada por threads diferentes, individualmente, de forma paralela.

updateCentroids: Distribuí os clusters para cada thread disponível, calcula a média das amostras assim como atualiza as coordenadas do cluster. Cada thread consegue calcular seu cluster individualmente de forma paralela pelo OpenMP.

kmeans: Função principal para executar o algoritmo, o MPI é utilizado aqui afim de distribuir os dados de um centroíde para diferentes processos. Após os centróides terem seus dados atualizados, é realizado o broadcast dos processos para que esse novo valor seja recebido para todos os processos.

# Desempenho alcançado
Tempo Sequencial : 12.8893 segundos

Apenas com o OpenMP:\
Tempo Paralelo 1 thread : 12.8899 segundos\
Tempo Paralelo 2 threads : 7.34364 segundos\
Tempo Paralelo 4 threads : 4.54277 segundos\
Tempo Paralelo 8 threads : 5.30841 segundos

Apenas com MPI\
1 processo: 12.7478 segundos\
2 processos: 6.6243 segundos\
4 processos: 3.7032 segundos\
8 processos: 14.1557 segundos

Com OpenMP e MPI\
1 processo 4 threads : 9.8449 segundos\
2 processos 2 threads : 3.6188 segundos\
4 processos 0 threads : 3.7032 segundos\
8 processos 0 threads : 14.1557 segundos

# Link Github
https://github.com/samuelspineli34/Computacao-Paralela-Projeto-1/tree/main
