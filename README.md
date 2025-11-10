#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>

/* Estrutura da sala (nó da árvore binária) */
typedef struct Sala {
    char nome[50];
    struct Sala *esquerda;
    struct Sala *direita;
} Sala;

/* Função para criar dinamicamente uma nova sala */
Sala* criarSala(const char *nome) {
    Sala *nova = (Sala*)malloc(sizeof(Sala));
    strcpy(nova->nome, nome);
    nova->esquerda = NULL;
    nova->direita = NULL;
    return nova;
}

/* Montagem estática da árvore binária (mapa da mansão) */
Sala* montarMansao() {
    Sala *hall = criarSala("Hall de Entrada");
    Sala *salaEstar = criarSala("Sala de Estar");
    Sala *cozinha = criarSala("Cozinha");
    Sala *biblioteca = criarSala("Biblioteca");
    Sala *escritorio = criarSala("Escritório");
    Sala *jardim = criarSala("Jardim");
    Sala *garagem = criarSala("Garagem");

    /* Ligações (estrutura da árvore) */
    hall->esquerda = salaEstar;
    hall->direita = cozinha;
    salaEstar->esquerda = biblioteca;
    salaEstar->direita = escritorio;
    cozinha->esquerda = jardim;
    cozinha->direita = garagem;

    return hall;
}

/* Função para converter entrada em minúsculas */
void toLower(char *str) {
    for (int i = 0; str[i]; i++)
        str[i] = tolower((unsigned char)str[i]);
}

/* Função principal de exploração das salas */
void explorarSalas(Sala *inicio) {
    Sala *atual = inicio;
    char opcao[10];

    printf("\nBem-vindo ao Detective Quest - Nível Novato!\n");
    printf("Explore a mansão misteriosa!\n");
    printf("Comandos: (e) esquerda | (d) direita | (s) sair\n");

    while (1) {
        printf("\nVocê está em: %s\n", atual->nome);

        /* Verifica se chegou a um nó-folha */
        if (!atual->esquerda && !atual->direita) {
            printf("Fim do caminho! Você chegou ao final da exploração.\n");
            break;
        }

        printf("Escolha o caminho (e/d/s): ");
        fgets(opcao, sizeof(opcao), stdin);
        toLower(opcao);

        if (opcao[0] == 'e') {
            if (atual->esquerda)
                atual = atual->esquerda;
            else
                printf("Não há caminho à esquerda!\n");
        }
        else if (opcao[0] == 'd') {
            if (atual->direita)
                atual = atual->direita;
            else
                printf("Não há caminho à direita!\n");
        }
        else if (opcao[0] == 's') {
            printf("Saindo da exploração...\n");
            break;
        }
        else {
            printf("Opção inválida! Use e, d ou s.\n");
        }
    }
}

/* Função para liberar memória da árvore */
void liberarSalas(Sala *raiz) {
    if (raiz == NULL) return;
    liberarSalas(raiz->esquerda);
    liberarSalas(raiz->direita);
    free(raiz);
}

/* Função principal */
int main() {
    Sala *mansao = montarMansao();
    explorarSalas(mansao);
    liberarSalas(mansao);

    printf("\nObrigado por jogar Detective Quest - Nível Novato!\n");
    return 0;
}
