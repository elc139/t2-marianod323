# Trabalho 2 - Programação Paralela

Mariano Dorneles de Freitas

## PThreads


### Questão 1

O particionamento é feito das linhas 34 à 43, nessas linhas é dividido o vetor e definido o que cada thread irá processar (qual parte do vetor). Vale ressaltar que o particionamento é abstrato, onde vimos que o programa é passível de paralelismo.
```c
int start = offset*wsize;
int end = start + wsize;
double mysum;
for (k = 0; k < dotdata.repeat; k++) {
	mysum = 0.0;
	for (i = start; i < end ; i++) {
		mysum += (a[i] * b[i]);
	} 
}
```
Já a comunicação é feita nas linhas 45 e 47, onde há uma região crítica, ou seja, apenas uma thread poderá alterá-la por vez, forçando-as a se comunicarem para que não sobreponham-se. A região crítica é a aglomeração, linha 46, onde todas as threads incluem os resultados de suas somas parciais.
```c
pthread_mutex_lock (&mutexsum);
dotdata.c += mysum;
pthread_mutex_unlock (&mutexsum);
```
O mapeamento é feito no terminal de forma estática, onde é definida a quantidade de threads que executarão, bem como a quantidade de carga que cada uma executará.
```bash
./pthreads_dotprod 1 1000000 2000
```
```c
int nthreads, wsize, repeat;
long start_time, end_time;
if ((argc != 4)) {
	printf("Uso: %s <nthreads> <worksize> <repetitions>\n", argv[0]);
	exit(EXIT_FAILURE);
} 

nthreads = atoi(argv[1]);
wsize = atoi(argv[2]); // worksize = tamanho do vetor de cada thread
repeat = atoi(argv[3]); // numero de repeticoes dos calculos (para aumentar carga)
```


### Questão 2
Speedup = **6479878 usec**/**3255949 usec** = 1,9902, ou seja, houve uma melhora de aproximadamente **99%** em relação ao programa sequencial.


### Questão 3
Sim, contudo de 4 para 8 threads o speedup é mínimo, tendo, por vezes, um desempenho pior com 8 threads.


### Questão 4

![](images/1000&#32;repetitions&#32;-&#32;PThreads.png)

![](images/2000&#32;repetitions&#32;-&#32;PThreads.png)

![](images/4000&#32;repetitions&#32;-&#32;PThreads.png)

|          |   |         |      |          |             | 
|----------|---|---------|------|----------|-------------| 
| PThreads | 1 | 1000000 | 1000 | 6474747  | 1           | 
| PThreads | 2 | 500000  | 1000 | 3408469  | 1.899605659 | 
| PThreads | 4 | 250000  | 1000 | 2766449  | 2.340454135 | 
| PThreads | 8 | 125000  | 1000 | 2698191  | 2.399662218 | 
| PThreads | 1 | 1000000 | 2000 | 3161379  | 1           | 
| PThreads | 2 | 500000  | 2000 | 1637340  | 1.930801788 | 
| PThreads | 4 | 250000  | 2000 | 1285851  | 2.458588903 | 
| PThreads | 8 | 125000  | 2000 | 1256863  | 2.515293234 | 
| PThreads | 1 | 1000000 | 4000 | 12664758 | 1           | 
| PThreads | 2 | 500000  | 4000 | 6554621  | 1.932187689 | 
| PThreads | 4 | 250000  | 4000 | 5125714  | 2.470828064 | 
| PThreads | 8 | 125000  | 4000 | 5141654  | 2.463168078 | 



### Questão 5

Sem o semáforo, o programa estaria incorreto, pois sua região crítica não está protegida de sobrescrição.


## OpenMP

## Questão 1

Código disponível no diretório openmp.


## Questão 2

![](image/../images/1000&#32;repetitions&#32;-&#32;OpenMP.png)

![](image/../images/2000&#32;repetitions&#32;-&#32;OpenMP.png)

![](image/../images/4000&#32;repetitions&#32;-&#32;OpenMP.png)

|        |   |         |      |          |             | 
|--------|---|---------|------|----------|-------------| 
| OpenMP | 1 | 1000000 | 1000 | 3188231  | 1           | 
| OpenMP | 2 | 500000  | 1000 | 1635935  | 1.948873886 | 
| OpenMP | 4 | 250000  | 1000 | 1321262  | 2.413019522 | 
| OpenMP | 8 | 125000  | 1000 | 1302781  | 2.447250152 | 
| OpenMP | 1 | 1000000 | 2000 | 5471310  | 1           | 
| OpenMP | 2 | 500000  | 2000 | 2881856  | 1.898536915 | 
| OpenMP | 4 | 250000  | 2000 | 2324487  | 2.353770961 | 
| OpenMP | 8 | 125000  | 2000 | 2357830  | 2.320485362 | 
| OpenMP | 1 | 1000000 | 4000 | 13077401 | 1           | 
| OpenMP | 2 | 500000  | 4000 | 6889230  | 1.89823841  | 
| OpenMP | 4 | 250000  | 4000 | 5567089  | 2.349055494 | 
| OpenMP | 8 | 125000  | 4000 | 5607688  | 2.332048609 | 


## Referências

http://www.sergioportari.com.br/wp-content/uploads/2016/08/Aula04-Programacao-Paralela.pdf

http://jakascorner.com/blog/2016/06/omp-for-reduction.html

https://computing.llnl.gov/tutorials/openMP/