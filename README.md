#include <stdio.h>

#include <stdlib.h>


// ==========================================

// FUNÇÕES PARA O MERGE SORT

// ==========================================

void merge(int v[], int inicio, int meio, int fim) {

  int n1 = meio - inicio + 1;

  int n2 = fim - meio;

  int *L = (int*) malloc(n1 * sizeof(int));

  int *R = (int*) malloc(n2 * sizeof(int));

  int i, j, k;


  for (i = 0; i < n1; i++) L[i] = v[inicio + i];

  for (j = 0; j < n2; j++) R[j] = v[meio + 1 + j];


  i = 0; j = 0; k = inicio;

  while (i < n1 && j < n2) {

    if (L[i] <= R[j]) v[k++] = L[i++];

    else v[k++] = R[j++];

  }

  while (i < n1) v[k++] = L[i++];

  while (j < n2) v[k++] = R[j++];


  free(L); free(R);

}


void mergeSort(int v[], int inicio, int fim) {

  if (inicio < fim) {

    int meio = (inicio + fim) / 2;

    mergeSort(v, inicio, meio);

    mergeSort(v, meio + 1, fim);

    merge(v, inicio, meio, fim);

  }

}


// ==========================================

// FUNÇÃO PARA O QUICK SORT

// ==========================================

void quickSort(int v[], int inicio, int fim) {

  int i = inicio, j = fim, aux;

  int pivo = v[(inicio + fim) / 2];

  while (i <= j) {

    while (v[i] < pivo) i++;

    while (v[j] > pivo) j--;

    if (i <= j) {

      aux = v[i];

      v[i] = v[j];

      v[j] = aux;

      i++;

      j--;

    }

  }

  if (inicio < j) quickSort(v, inicio, j);

  if (i < fim) quickSort(v, i, fim);

}


// ==========================================

// FUNÇÃO PRINCIPAL

// ==========================================

int main(void) {

  int n, numero, i, j, aux, chave, min;


  // MÓDULO 1: Inicialização (Criar e gravar dados.txt)

  printf("Quantos números deseja inserir no arquivo? ");

  if (scanf("%d", &n) != 1 || n <= 0) return 1;


  FILE *arq = fopen("dados.txt", "w");

  if(arq == NULL) {

    printf("Erro ao criar dados.txt\n");

    return 1;

  }


  for(i = 0; i < n; i++) {

    printf("Digite o %do numero: ", i + 1);

    scanf("%d", &numero);

    fprintf(arq, "%d\n", numero);

  }

  fclose(arq);

  printf("\nArquivo dados.txt criado com sucesso!\n");


  // MÓDULO 2: Leitura (Trazer do disco para a RAM)

  int vetor[n];

  FILE *arqLeitura = fopen("dados.txt", "r");

  if (arqLeitura == NULL) {

    printf("Erro ao ler dados.txt\n");

    return 1;

  }


  for(i = 0; i < n; i++) {

    fscanf(arqLeitura, "%d", &vetor[i]);

  }

  fclose(arqLeitura);


  // MÓDULO 3: Interação (Menu de Algoritmos)

  printf("\nEscolha o algoritmo de ordenacao:\n1 - Bubble Sort\n2 - Insertion Sort\n3 - Selection Sort\n4 - Merge Sort\n5 - Quick Sort\nOpcao: ");

  int opcao;

  scanf("%d", &opcao);


  // MÓDULO 4: Processamento (Ordenação na RAM)

  switch (opcao) {

    case 1:

      for (i = 0; i < n - 1; i++) {

        for (j = 0; j < n - i - 1; j++) {

          if (vetor[j] > vetor[j + 1]) {

            aux = vetor[j]; vetor[j] = vetor[j + 1]; vetor[j + 1] = aux;

          }

        }

      }

      break;

    case 2:

      for (i = 1; i < n; i++) {

        chave = vetor[i]; j = i - 1;

        while (j >= 0 && vetor[j] > chave) {

          vetor[j + 1] = vetor[j]; j--;

        }

        vetor[j + 1] = chave;

      }

      break;

    case 3:

      for (i = 0; i < n - 1; i++) {

        min = i;

        for (j = i + 1; j < n; j++) {

          if (vetor[j] < vetor[min]) min = j;

        }

        aux = vetor[i]; vetor[i] = vetor[min]; vetor[min] = aux;

      }

      break;

    case 4:

      mergeSort(vetor, 0, n - 1);

      break;

    case 5:

      quickSort(vetor, 0, n - 1);

      break;

    default:

      printf("Opcao invalida!\n");

      return 1;

  }


  // MÓDULO 5: Saída Obrigatória (Gera e grava o arquivo "ordenado.txt")

  FILE *arqSaida = fopen("ordenado.txt", "w");

  if (arqSaida == NULL) {

    printf("Erro ao criar o arquivo ordenado.txt\n");

    return 1;

  }


  printf("\nResultado salvo com sucesso em ordenado.txt: ");

  for(i = 0; i < n; i++) {

    fprintf(arqSaida, "%d\n", vetor[i]); // Grava de forma formatada no txt[cite: 18]

    printf("%d ", vetor[i]);      // Exibe na tela do usuário

  }

  fclose(arqSaida);

  printf("\n");


  // MÓDULO 6: Alteração e Reordenação Opcional

  int valorAntigo, valorNovo;

  printf("\nDeseja alterar algum valor no arquivo?\nDigite o valor que deseja buscar (ou -1 para sair): ");

  scanf("%d", &valorAntigo);


  if (valorAntigo != -1) {

    printf("Digite o NOVO valor: ");

    scanf("%d", &valorNovo);


    // Busca o valor antigo no vetor e substitui

    for(i = 0; i < n; i++) {

      if(vetor[i] == valorAntigo) {

        vetor[i] = valorNovo;

        break;

      }

    }


    // Reordena o vetor alterado

    for (i = 0; i < n - 1; i++) {

      for (j = 0; j < n - i - 1; j++) {

        if (vetor[j] > vetor[j + 1]) {

          aux = vetor[j]; vetor[j] = vetor[j + 1]; vetor[j + 1] = aux;

        }

      }

    }


    // Atualiza o arquivo ordenado.txt com os novos dados

    arqSaida = fopen("ordenado.txt", "w");

    printf("\nNovo vetor alterado, reordenado e salvo em ordenado.txt: ");

    for(i = 0; i < n; i++) {

      fprintf(arqSaida, "%d\n", vetor[i]);

      printf("%d ", vetor[i]);

    }

    fclose(arqSaida);

    printf("\n");

  }


  return 0;

}
