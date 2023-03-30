#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>
//41783174 - Rodrigo Rios
//32271697 - Francesco Coppola
//32162871 - Matheus Duarte
// a comunicaçao entre as threads vem do vetor de semaforos garfo
#define N 5 // número de filósofos
#define LEFT (i+N-1)%N // índice do vizinho da esquerda
#define RIGHT (i+1)%N // índice do vizinho da direita

pthread_t filosofo[N]; // threads para cada filósofo
sem_t garfo[N]; // semáforos para cada garfo
sem_t mutex; // semáforo para exclusão mútua

void *jantar(void *arg) {
    int i = *(int *) arg;
    while (1) {
        printf("Filósofo %d pensando...\n", i);
        sleep(rand()%5); // filósofo pensando
        sem_wait(&mutex); // entrada na região crítica
        sem_wait(&garfo[LEFT]); // pega garfo da esquerda
        sem_wait(&garfo[RIGHT]); // pega garfo da direita
        printf("Filósofo %d comendo...\n", i);
        sleep(rand()%5); // filósofo comendo
        sem_post(&garfo[RIGHT]); // solta garfo da direita
        sem_post(&garfo[LEFT]); // solta garfo da esquerda
        sem_post(&mutex); // saída da região crítica
    }
    pthread_exit(NULL);
}

int main() {
    int i, id[N];
    sem_init(&mutex, 0, 1); // inicializa semáforo mutex
    for (i = 0; i < N; i++) {
        sem_init(&garfo[i], 0, 1); // inicializa semáforo de cada garfo
    }
    for (i = 0; i < N; i++) {
        id[i] = i;
        pthread_create(&filosofo[i], NULL, jantar, (void *) &id[i]); // cria thread para cada filósofo
    }
    for (i = 0; i < N; i++) {
        pthread_join(filosofo[i], NULL); // aguarda término das threads
    }
    return 0;
}
