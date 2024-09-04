# Atividade-39

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>

#define MAX_SIZE 100
#define MAX_EXPR_SIZE 256

// Função para empurrar um item para a pilha
void push(int stack[], int *top, int value) {
    if (*top >= MAX_SIZE - 1) {
        printf("Pilha cheia\n");
        return;
    }
    stack[++(*top)] = value;
}

// Função para retirar um item da pilha
int pop(int stack[], int *top) {
    if (*top < 0) {
        printf("Pilha vazia\n");
        exit(1);
    }
    return stack[(*top)--];
}

// Função para avaliar a expressão pós-fixa
int evaluatePostfix(const char *expression) {
    int stack[MAX_SIZE];
    int top = -1;
    int i = 0;

    while (expression[i] != '\0') {
        if (isdigit(expression[i])) {
            // Se o caractere for um dígito, empurre-o para a pilha
            int num = 0;
            while (isdigit(expression[i])) {
                num = num * 10 + (expression[i] - '0');
                i++;
            }
            push(stack, &top, num);
        } else if (expression[i] == ' ') {
            // Ignore espaços
            i++;
        } else {
            // Se o caractere for um operador
            int operand2 = pop(stack, &top);
            int operand1 = pop(stack, &top);
            int result;

            switch (expression[i]) {
                case '+':
                    result = operand1 + operand2;
                    break;
                case '-':
                    result = operand1 - operand2;
                    break;
                case '*':
                    result = operand1 * operand2;
                    break;
                case '/':
                    if (operand2 == 0) {
                        printf("Erro: Divisao por zero\n");
                        exit(1);
                    }
                    result = operand1 / operand2;
                    break;
                default:
                    printf("Erro: Operador desconhecido %c\n", expression[i]);
                    exit(1);
            }

            push(stack, &top, result);
            i++;
        }
    }

    // O resultado final estará no topo da pilha
    return pop(stack, &top);
}

int main() {
    char expression[MAX_EXPR_SIZE];

    printf("Digite a expressao pos-fixa (por exemplo, \"7 8 + 3 2 + /\"): ");
    // Lê a expressão do usuário
    fgets(expression, MAX_EXPR_SIZE, stdin);

    // Remove o caractere de nova linha, se presente
    size_t len = strlen(expression);
    if (len > 0 && expression[len - 1] == '\n') {
        expression[len - 1] = '\0';
    }

    // Avalia a expressão e imprime o resultado
    printf("Resultado da expressao pos-fixa: %d\n", evaluatePostfix(expression));

    return 0;
}
