# Xadrez
#include <stdio.h>
#include <stdlib.h>

#define TAM 8

// Função para imprimir o tabuleiro com os movimentos possíveis
void imprimirTabuleiro(int tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++) {
        for (int j = 0; j < TAM; j++) {
            printf("%2d ", tabuleiro[i][j]);
        }
        printf("\n");
    }
    printf("\n");
}

// Zera o tabuleiro
void limparTabuleiro(int tabuleiro[TAM][TAM]) {
    for (int i = 0; i < TAM; i++)
        for (int j = 0; j < TAM; j++)
            tabuleiro[i][j] = 0;
}

// ----------------- Torre (for) -----------------
void movimentosTorre(int x, int y, int tabuleiro[TAM][TAM]) {
    limparTabuleiro(tabuleiro);
    for (int i = 0; i < TAM; i++) {
        tabuleiro[x][i] = 1; // horizontal
        tabuleiro[i][y] = 1; // vertical
    }
    tabuleiro[x][y] = 9; // posição da torre
}

// ----------------- Bispo (while) -----------------
void movimentosBispo(int x, int y, int tabuleiro[TAM][TAM]) {
    limparTabuleiro(tabuleiro);
    int i, j;

    // Diagonal superior esquerda
    i = x - 1; j = y - 1;
    while (i >= 0 && j >= 0) {
        tabuleiro[i--][j--] = 1;
    }

    // Diagonal superior direita
    i = x - 1; j = y + 1;
    while (i >= 0 && j < TAM) {
        tabuleiro[i--][j++] = 1;
    }

    // Diagonal inferior esquerda
    i = x + 1; j = y - 1;
    while (i < TAM && j >= 0) {
        tabuleiro[i++][j--] = 1;
    }

    // Diagonal inferior direita
    i = x + 1; j = y + 1;
    while (i < TAM && j < TAM) {
        tabuleiro[i++][j++] = 1;
    }

    tabuleiro[x][y] = 9; // posição do bispo
}

// ----------------- Rainha (do-while + bispo + torre) -----------------
void movimentosRainha(int x, int y, int tabuleiro[TAM][TAM]) {
    limparTabuleiro(tabuleiro);
    int i;

    // Torre
    i = 0;
    do {
        tabuleiro[x][i] = 1;
        tabuleiro[i][y] = 1;
        i++;
    } while (i < TAM);

    // Bispo
    i = 1;
    while (x - i >= 0 && y - i >= 0) tabuleiro[x - i][y - i] = 1, i++;
    i = 1;
    while (x - i >= 0 && y + i < TAM) tabuleiro[x - i][y + i] = 1, i++;
    i = 1;
    while (x + i < TAM && y - i >= 0) tabuleiro[x + i][y - i] = 1, i++;
    i = 1;
    while (x + i < TAM && y + i < TAM) tabuleiro[x + i][y + i] = 1, i++;

    tabuleiro[x][y] = 9; // posição da rainha
}

// ----------------- Cavalo (loops aninhados) -----------------
void movimentosCavalo(int x, int y, int tabuleiro[TAM][TAM]) {
    limparTabuleiro(tabuleiro);
    int dx[] = { -2, -1, 1, 2, 2, 1, -1, -2 };
    int dy[] = { 1, 2, 2, 1, -1, -2, -2, -1 };

    for (int i = 0; i < 8; i++) {
        int nx = x + dx[i];
        int ny = y + dy[i];
        if (nx >= 0 && nx < TAM && ny >= 0 && ny < TAM)
            tabuleiro[nx][ny] = 1;
    }

    tabuleiro[x][y] = 9; // posição do cavalo
}

// ----------------- Movimentos Avançados com Recursão (Rainha Avançada) -----------------
void movimentosRainhaAvancada(int x, int y, int tabuleiro[TAM][TAM]) {
    limparTabuleiro(tabuleiro);

    // Movimentos recursivos
    void mover(int x, int y, int dx, int dy) {
        int nx = x + dx;
        int ny = y + dy;
        if (nx >= 0 && ny >= 0 && nx < TAM && ny < TAM) {
            tabuleiro[nx][ny] = 1;
            mover(nx, ny, dx, dy); // chama recursivamente na mesma direção
        }
    }

    // 8 direções possíveis da rainha
    int dx[] = { -1, -1, -1, 0, 1, 1, 1, 0 };
    int dy[] = { -1, 0, 1, 1, 1, 0, -1, -1 };

    for (int i = 0; i < 8; i++)
        mover(x, y, dx[i], dy[i]);

    tabuleiro[x][y] = 9; // posição da rainha
}

// ----------------- Main -----------------
int main() {
    int tabuleiro[TAM][TAM];
    int x = 3, y = 3;

    printf("Movimentos da Torre:\n");
    movimentosTorre(x, y, tabuleiro);
    imprimirTabuleiro(tabuleiro);

    printf("Movimentos do Bispo:\n");
    movimentosBispo(x, y, tabuleiro);
    imprimirTabuleiro(tabuleiro);

    printf("Movimentos da Rainha:\n");
    movimentosRainha(x, y, tabuleiro);
    imprimirTabuleiro(tabuleiro);

    printf("Movimentos do Cavalo:\n");
    movimentosCavalo(x, y, tabuleiro);
    imprimirTabuleiro(tabuleiro);

    printf("Movimentos Avançados da Rainha (com recursividade):\n");
    movimentosRainhaAvancada(x, y, tabuleiro);
    imprimirTabuleiro(tabuleiro);

    return 0;
}
