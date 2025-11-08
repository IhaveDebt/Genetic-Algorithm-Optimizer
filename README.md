#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <time.h>

#define POP_SIZE 100
#define GENERATIONS 500
#define MUTATION_RATE 0.1

double fitness(double x) {
    return x * sin(10 * M_PI * x) + 1.0;
}

double random_double() {
    return ((double)rand() / RAND_MAX) * 2 - 1; // range [-1,1]
}

int main() {
    srand(time(0));
    double population[POP_SIZE];
    for (int i = 0; i < POP_SIZE; i++) population[i] = random_double();

    for (int gen = 0; gen < GENERATIONS; gen++) {
        double best = population[0];
        for (int i = 1; i < POP_SIZE; i++)
            if (fitness(population[i]) < fitness(best))
                best = population[i];

        printf("Gen %d | Best: %.5f | Fitness: %.5f\n", gen, best, fitness(best));

        double new_pop[POP_SIZE];
        for (int i = 0; i < POP_SIZE; i++) {
            int p1 = rand() % POP_SIZE, p2 = rand() % POP_SIZE;
            double child = (population[p1] + population[p2]) / 2.0;
            if (((double)rand() / RAND_MAX) < MUTATION_RATE)
                child += random_double() * 0.1;
            new_pop[i] = child;
        }
        memcpy(population, new_pop, sizeof(population));
    }
    return 0;
}
