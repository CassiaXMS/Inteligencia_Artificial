# Inteligencia_Artificial
Este repositório é dedicado aos códigos em Python aplicando técnicas de IA na solução de problemas.

# Algoritmos de Busca



Aqui está um mapa contendo algumas cidades da regiião Sul do Brasil e suas relativas distâncias entre elas e a tabela com as distâncias em 
linha reta.

<img align="center" src="https://github.com/CassiaXMS/Inteligencia_Artificial/blob/main/mapa_portoUniao_Curitiba.jpeg" alt="map"  width="800" height="500" >

# 📌 Objetivo
**_O desafio é o seguinte:_** com base nos dados do mapa e da tabela, qual será a rota onde a cidade de origem é **Porto União** e o cidade final é **Curitiba**. 
Utilizando algoritmo por meio de busca.

### Algoritmo de Busca A*
Vamos começar com o algoritmo de busca A*. Segue os passos:

#### Grafo
> Definir a estrutura do grafo com os **nós** (vértices, são as cidades) e as **arestas** (ligações) contendo as
> distâncias reais entre as cidades. A ideia é escolher os nós vizinhos, os mais próximos.

```python
# Dicionário de dados
grafo = {
    'Porto União': {'Paulo Frontin': 46, 'Canoinhas': 78, 'São Mateus do Sul': 87},
    'Paulo Frontin': {'Irati': 75, 'Porto União': 46},
    'Canoinhas': {'Porto União': 78, 'Três Barras': 12, 'Mafra':66},
    'São Mateus do Sul': {'Lapa': 60, 'Palmeira': 70, 'Irati': 57, 'Três Barras':43},
    'Lapa': {'Contenda': 26, 'Mafra': 57},
    'Contenda': {'Balsa Nova': 19, 'Araucária':18},
    'Curitiba': {'Balsa Nova': 51, 'Araucária': 37, 'Campo Largo':29},
    'Palmeira': {'Campo Largo':55,  'São Mateus do Sul': 77, 'Irati': 75},
    'Araucária': {'Contenda': 18, 'Curitiba':37},
    'Tijucas do Sul': {'Mafra': 99, 'São José dos Pinhais':49},
    'Campo Largo': {'Balsa Nova': 22, 'Curitiba':29},
    'Três Barras': {'São Mateus do Sul':43, 'Canoinhas':12}
}
```
#### Heurística
> É a distância em linha reta de cada cidade. Aqui entra os dados da tabela

```python
# Dicionário de dados
heuristica = {
    'Porto União': 203,
    'Paulo Frontin': 172,
    'Canoinhas':141,
    'Três Barras': 131,
    'São Mateus do Sul': 123,
    'Irati':139,
    'Curitiba':0,
    'Palmeira': 59,
    'Mafra':94,
    'Campo Largo': 27,
    'Balsa Nova':41,
    'Lapa':74,
    'Tijucas do Sul':56,
    'Araucária':23,
    'São José dos Pinhais':13,
    'Contenda':39
}
```
#### Execução do algoritmo 

Import do módulo `heapq`. O heap é uma estrutura de dados eficiente para manter uma lista onde o menor elemento pode ser rapidamente acessado. 

```python

import heapq

# criação da função a_estrela, com o recebimento dos 4 parâmetros (os dicionários de grafo e estrela
# e as cidades de busca.
def a_estrela(grafo, heuristica, inicio, objetivo):

    # Fila de prioridade para armazenar os nós a serem explorados, ordenados pelo menor custo total f(n)
    fila_prioridade = []
    heapq.heappush(fila_prioridade, (0, inicio))

    # Dicionário para armazenar o custo acumulado mais baixo, iniciando em 0.
    custos = {inicio: 0}
    # Dicionário para armazenar o caminho percorrido a cada nó. Seu valor é None, pois só temos o ponto de partida
    caminho = {inicio: None}

    # O loop do algoritmo é processado enquanto tiver nós na fila de prioridade
    while fila_prioridade:

      # Remove o nó com o menor f(n) da fila
      _, atual = heapq.heappop(fila_prioridade)

      # Se chegamos ao objetivo, reconstruímos o caminho
      if atual == objetivo:
        caminho_reconstruido = []

        while atual:
          caminho_reconstruido.append(atual)
          atual = caminho[atual]
        return caminho_reconstruido[::-1], custos[objetivo]

      # Explorar os vizinhos do nó atual
      # Agora o for percorre o grafo calculando a distância de todos os vizinhos do nó atual e o novo custo.
      for vizinho, custo in grafo[atual].items():
        novo_custo = custos[atual] + custo

        # A condição, ainda no loop obedece, a lógica de se o vizinho ainda não foi explorado ou se encontramos um caminho mais curto para o vizinho, o dicionário custos é atualizado 
        if vizinho not in custos or novo_custo < custos[vizinho]:
          custos[vizinho] = novo_custo
          # O cálculo da soma do novo custo com a heurística
          prioridade = novo_custo + heuristica[vizinho]

          #adicionado à fila de prioridade para continuar o laço
          heapq.heappush(fila_prioridade, (prioridade, vizinho))
          caminho[vizinho] = atual
    return None, float('inf')  # Retorna None se não há caminho

# Definindo os pontos de partida e objetivo
inicio = 'Porto União'
objetivo = 'Curitiba'

# Executando o algoritmo A*
caminho, distancia_total = a_estrela(grafo, heuristica, inicio, objetivo)

print("Caminho encontrado:", caminho)
print("Distância total:", distancia_total, "km")

  

```
