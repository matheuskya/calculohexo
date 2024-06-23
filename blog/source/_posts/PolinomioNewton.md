---
title: Polinômios interpoladores - métodos de Newton e Lagrange
date: 2024-06-01 17:41:21
tags:
---

A seguir, esta o código usado para gerar o Polinômio interpolador de Newton e Lagrange.
O código a seguir foi escrito em python.

``` bash

#para melhor compreensao da leitura do codigo, tudo foi organizado em funcoes

#A funcao que pega os inputs do usuario, as coordenadas (x, y) sao separadas em duas listas diferentes
#a lista_x e lista_y
def num_coordenadas():
    num_pontos = int(input("Quantos pontos você deseja criar? \n"))
    lista_x = []
    lista_y = []
    
    for i in range(num_pontos):
        x_y = input(f"Digite as coordenadas do ponto {i + 1} (x e y respectivamente, separados por espaço): \n")
        x_y = x_y.split()
        lista_x.append(float(x_y[0]))
        lista_y.append(float(x_y[1]))
    
    return lista_x, lista_y

#Funcao que realiza as diferencas dividas, organizando os dados em uma tabela e apos as operacoes
#retorna uma lista com a primeira linha da tabela
def dif_divididas(lista_x, lista_y):
    n = len(lista_x)
    table = [[0] * n for _ in range(n)]
    
    for i in range(n):
        table[i][0] = lista_y[i]
    
    for j in range(1, n):
        for i in range(n - j):
            table[i][j] = (table[i + 1][j - 1] - table[i][j - 1]) / (lista_x[i + j] - lista_x[i])
    
    coef = [table[0][j] for j in range(n)]
    return coef

#Funcao que recebe o valor de X desejado, e atraves do polinomio de newton(em conjunto com a linha re
# tornada da tabela (diferencas divididas)), retorna o resultado y.
def pol_newton(lista_x, coef, x):
    n = len(lista_x)
    resultado = coef[0]
    for i in range(1, n):
        term = coef[i]
        for j in range(i):
            term *= (x - lista_x[j])
        resultado += term
    return resultado


def pol_lagrange(lista_x, lista_y, x):
    n = len(lista_x)
    result = 0
    for i in range(n):
        term = lista_y[i]   
        for j in range(n):
            if j != i:
                term *= (x - lista_x[j]) / (lista_x[i] - lista_x[j])
        result += term
    return result

#Funcao principal que inicia as outras funcoes e coleta o input do usuario
def main():
    lista_x, lista_y = num_coordenadas()
    metodo = input("Escolha o método de interpolação (Newton/Lagrange): \n").strip().lower()

    if metodo == "newton":
        coef = dif_divididas(lista_x, lista_y)
        
        print("\nCoeficientes do polinômio de Newton:")
        for i, c in enumerate(coef):
            print(f"a{i} = {c}")
        
        x = float(input("\nDigite o valor de x para avaliar o polinômio de Newton: \n"))
        resultado = pol_newton(lista_x, coef, x)
        print(f"O valor do polinômio de Newton em x = {x} é: {resultado}")

    elif metodo == "lagrange":
        x = float(input("\nDigite o valor de x para avaliar o polinômio de Lagrange: \n"))
        result = pol_lagrange(lista_x, lista_y, x)
        print(f"O valor do polinômio de Lagrange em x = {x} é: {result}")
    
    else:
        print("Erro. Por favor, escolha 'Newton' ou 'Lagrange' (digite).")

if __name__ == "__main__":
    main()

```