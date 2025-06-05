#include <stdio.h>
#include <string.h>

#define MAX_TAREFAS 100
#define MAX_CARACTERES 50

void cadastrarTarefa(char tarefas[][4][MAX_CARACTERES], int *quantidade) {
    if (*quantidade >= MAX_TAREFAS) {
        printf("\nLimite de tarefas atingido.\n");
        return;
    }

    printf("\n--- Cadastro de Tarefa ---\n");
    printf("Titulo: ");
    fgets(tarefas[*quantidade][0], MAX_CARACTERES, stdin);
    strtok(tarefas[*quantidade][0], "\n");

    printf("Descricao: ");
    fgets(tarefas[*quantidade][1], MAX_CARACTERES, stdin);
    strtok(tarefas[*quantidade][1], "\n");

    printf("Prioridade (Alta/Media/Baixa): ");
    fgets(tarefas[*quantidade][2], MAX_CARACTERES, stdin);
    strtok(tarefas[*quantidade][2], "\n");

    printf("Status (Pendente/Concluida): ");
    fgets(tarefas[*quantidade][3], MAX_CARACTERES, stdin);
    strtok(tarefas[*quantidade][3], "\n");

    (*quantidade)++;
    printf("Tarefa cadastrada com sucesso!\n");
}

void listarTarefas(char tarefas[][4][MAX_CARACTERES], int quantidade) {
    printf("\n--- Lista de Tarefas ---\n");
    for (int i = 0; i < quantidade; i++) {
        printf("\nTarefa %d:\n", i + 1);
        printf("Titulo: %s\n", tarefas[i][0]);
        printf("Descricao: %s\n", tarefas[i][1]);
        printf("Prioridade: %s\n", tarefas[i][2]);
        printf("Status: %s\n", tarefas[i][3]);
    }
    if (quantidade == 0) {
        printf("Nenhuma tarefa cadastrada.\n");
    }
}

void editarTarefa(char tarefas[][4][MAX_CARACTERES], int indice) {
    printf("\n--- Editar Tarefa %d ---\n", indice + 1);
    printf("Novo Titulo: ");
    fgets(tarefas[indice][0], MAX_CARACTERES, stdin);
    strtok(tarefas[indice][0], "\n");

    printf("Nova Descricao: ");
    fgets(tarefas[indice][1], MAX_CARACTERES, stdin);
    strtok(tarefas[indice][1], "\n");

    printf("Nova Prioridade: ");
    fgets(tarefas[indice][2], MAX_CARACTERES, stdin);
    strtok(tarefas[indice][2], "\n");

    printf("Novo Status: ");
    fgets(tarefas[indice][3], MAX_CARACTERES, stdin);
    strtok(tarefas[indice][3], "\n");

    printf("Tarefa editada com sucesso!\n");
}

void excluirTarefa(char tarefas[][4][MAX_CARACTERES], int *quantidade, int indice) {
    for (int i = indice; i < (*quantidade) - 1; i++) {
        for (int j = 0; j < 4; j++) {
            strcpy(tarefas[i][j], tarefas[i + 1][j]);
        }
    }
    (*quantidade)--;
    printf("Tarefa excluida com sucesso!\n");
}

void salvarTarefasEmArquivo(char tarefas[][4][MAX_CARACTERES], int quantidade) {
    FILE *arquivo = fopen("tarefas.txt", "w");
    if (arquivo == NULL) {
        printf("Erro ao abrir o arquivo para escrita.\n");
        return;
    }

    for (int i = 0; i < quantidade; i++) {
        fprintf(arquivo, "Tarefa %d:\n", i + 1);
        fprintf(arquivo, "Titulo: %s\n", tarefas[i][0]);
        fprintf(arquivo, "Descricao: %s\n", tarefas[i][1]);
        fprintf(arquivo, "Prioridade: %s\n", tarefas[i][2]);
        fprintf(arquivo, "Status: %s\n\n", tarefas[i][3]);
    }
    fclose(arquivo);
    printf("Tarefas salvas em tarefas.txt!\n");
}

int main() {
    char tarefas[MAX_TAREFAS][4][MAX_CARACTERES];
    int quantidade = 0;
    int opcao;

    do {
        printf("\n=== Sistema de Gerenciamento de Tarefas ===\n");
        printf("1. Cadastrar Tarefa\n");
        printf("2. Listar Tarefas\n");
        printf("3. Editar Tarefa\n");
        printf("4. Excluir Tarefa\n");
        printf("5. Salvar Tarefas em Arquivo\n");
        printf("6. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);
        getchar(); // limpar buffer

        switch (opcao) {
            case 1:
                cadastrarTarefa(tarefas, &quantidade);
                break;
            case 2:
                listarTarefas(tarefas, quantidade);
                break;
            case 3: {
                int i;
                listarTarefas(tarefas, quantidade);
                printf("Informe o numero da tarefa para editar: ");
                scanf("%d", &i);
                getchar();
                if (i > 0 && i <= quantidade) editarTarefa(tarefas, i - 1);
                else printf("Indice invalido.\n");
                break;
            }
            case 4: {
                int i;
                listarTarefas(tarefas, quantidade);
                printf("Informe o numero da tarefa para excluir: ");
                scanf("%d", &i);
                getchar();
                if (i > 0 && i <= quantidade) excluirTarefa(tarefas, &quantidade, i - 1);
                else printf("Indice invalido.\n");
                break;
            }
            case 5:
                salvarTarefasEmArquivo(tarefas, quantidade);
                break;
            case 6:
                printf("Saindo...\n");
                break;
            default:
                printf("Opcao invalida.\n");
        }
    } while (opcao != 6);

    return 0;
}
