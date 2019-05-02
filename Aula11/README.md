# Trabalho Prático 8 - 29/Abr/2019

## 1. Buffer Overflow

### Pergunta P1.1 - Buffer overflow em várias linguagens

TODO: Mafalda

### Pergunta P1.2 - Buffer overflow em várias linguagens

TODO: Daniel

### Pergunta P1.3 - Buffer overflow

> DÚVIDA: 64 bytes (buffer) + 12 bytes => control

bc &control
   endereço?
70 &buffer

TODO: Afonso

### Pergunta P1.4 - Read overflow

TODO: Mafalda

### Pergunta P1.5

TODO: Daniel

## 2. Vulnerabilidade de inteiros

### Pergunta P2.1

TODO: Afonso

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>

void vulneravel (char *matriz, size_t x, size_t y, char valor) {
    int i, j;

    matriz = (char *) malloc(x*y);

    printf("HERE\n");

    for (i = 0; i < x; i++) {
        for (j = 0; j < y; j++) {
            matriz[i*y+j] = valor;
        }
    }

}

int main() {
    char *matriz;
    printf("Tamanho da matriz: %zu\n", SIZE_MAX*SIZE_MAX);
    vulneravel(matriz, SIZE_MAX, SIZE_MAX, '1');
    return 0;
}
```

### Pergunta P2.2

TODO: Mafalda

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

const int MAX_SIZE = 2048;


void vulneravel (char *origem, size_t tamanho) {
    size_t tamanho_real;
    char *destino;
    if (tamanho < MAX_SIZE) {
        tamanho_real = tamanho - 1; // Não copiar \0 de origem para destino
        destino = (char *) malloc(tamanho_real);
        printf("%zu\n", tamanho_real);
        memcpy(destino, origem, tamanho_real);
        printf("%s\n", destino);
    }
}

int main() {
    char origem[10] = "MENSAGEM";
    vulneravel(origem, 0);
}
```