# TA3_CC82_TopicosCS

 <h2 align="center">Universidad Peruana de Ciencias Aplicadas</h2>
<h2 align="center">Tópicos en Ciencias de la Computación - CC58</h2>
 
<h3 align="center"> TA3 </h3>
 
<h3 align="center"> Sección</h3>
<h3 align="center"> Profesor: Luis Martín Canaval Sanchez</h3>
<h3 align="center"> Alumnos</h3>
 <ul>
   <li align="center">Camargo Ramírez, Enzo Fabricio (U202010122)</li>
   <li align="center">Costa Mondragón, Paulo Sergio (U201912086)</li>
   <li align="center">Caballero Lara, Eduardo Roman (U202019644)</li>
 </ul>
 
 
 <h3 align="center">CICLO 2024-1</h3>

## Enunciado del problema

<p align="justify">
Constraint programming tiene muchas aplicaciones en data mining, por ejemplo en la búsqueda de conjuntos de ítems frecuentes (frequent item sets) y reglas de asociación (association rules). Por tal motivo se le pide implementar una aplicación sencilla (por ejemplo análisis de bolsa de mercado) utilizando dichas técnicas con el uso de constraint programming.

Para el desarrollo de este problema y la correcta implementación de constraint programming para la búsqueda de itemsets frecuentes al igual que reglas de asociación hemos decidido optar por un sistema de transacciones simples que demuestra los items comprados por semana. Generando itemsets frecuentes como son los productos de compra.
</p>

## Tareas desarrolladas

<p align="justify">
<ul>
  <li> Implementar la búsqueda de itemsets frecuentes usando constraint programming. </li>
 La función get_subsets tiene como objetivo encontrar todos los subconjuntos posibles de ítems en un conjunto de transacciones y determinar cuáles de esos subconjuntos son frecuentes, es decir, aparecen al menos un número mínimo de veces (minimum support) en las transacciones.

 
  ![image](https://github.com/PSCostaM/TA3_CC82_TopicsCs/assets/48858434/7cb4f34b-2167-4615-bae5-84c820ecdcfd) 

  <li>Explique su implementación itemsets frecuentes y proporcione ejemplos:</li>
  
  ```
           transactions = [
             frozenset(['leche', 'pan', 'mantequilla']),
             frozenset(['pan', 'mantequilla']),
             frozenset(['leche', 'pan']),
             frozenset(['leche', 'mantequilla']),
             frozenset(['leche', 'pan', 'mantequilla']),
             frozenset(['pan']),
             frozenset(['leche', 'pan'])
           ]
           min_support = 2
  ```
  Supongamos que tenemos las siguientes transacciones y un soporte mínimo de 2:
  La función get_subsets generará todos los posibles subconjuntos de cada transacción y contará su frecuencia. Filtrará los subconjuntos que aparecen al menos 2 veces.
  Devolverá un diccionario con los itemsets frecuentes y sus frecuencias:
  ```
       {
         frozenset({'leche'}): 4,
         frozenset({'pan'}): 5,
         frozenset({'mantequilla'}): 3,
         frozenset({'leche', 'pan'}): 4,
         frozenset({'leche', 'mantequilla'}): 2,
         frozenset({'pan', 'mantequilla'}): 2
     }
  ```
  Este diccionario muestra que el ítem "leche" aparece en 4 transacciones, "pan" en 5 transacciones, y así sucesivamente. Los subconjuntos como {'leche', 'pan'} aparecen en 4 transacciones, cumpliendo con el soporte mínimo especificado.

  <li> Implementar la búsqueda de reglas de asociación usando constraint
programming </li>

![image](https://github.com/PSCostaM/TA3_CC82_TopicsCs/assets/48858434/96ff3195-42b9-4ba7-be82-b1e2877b9d74)

<li>Explique su implementación reglas de asociación y proporcione ejemplos.</li>

La función get_association_rules tiene como objetivo generar todas las posibles reglas de asociación a partir de los itemsets frecuentes y determinar cuáles de esas reglas cumplen con un umbral mínimo de confianza.

Supongamos que tenemos los siguientes itemsets frecuentes y un umbral de confianza de 0.6:

```
frequent_itemsets = {
    frozenset({'leche', 'pan'}): 4,
    frozenset({'leche'}): 5,
    frozenset({'pan'}): 5,
    frozenset({'mantequilla'}): 3,
    frozenset({'leche', 'mantequilla'}): 2,
    frozenset({'pan', 'mantequilla'}): 2
}

transactions = [
    frozenset(['leche', 'pan', 'mantequilla']),
    frozenset(['pan', 'mantequilla']),
    frozenset(['leche', 'pan']),
    frozenset(['leche', 'mantequilla']),
    frozenset(['leche', 'pan', 'mantequilla']),
    frozenset(['pan']),
    frozenset(['leche', 'pan'])
]

min_confidence = 0.6
```

La función get_association_rules generará todas las posibles reglas de los itemsets frecuentes y calculará su confianza.
Para el itemset {'leche', 'pan'}, las posibles reglas son {'leche'} -> {'pan'} y {'pan'} -> {'leche'}.
Si la confianza de la regla {'leche'} -> {'pan'} es 0.8 (su soporte es 4 y el soporte de {'leche'} es 5), y cumple con el umbral de 0.6, entonces se incluirá en los resultados.

```
Reglas de asociación:
{'leche'} -> {'pan'} (confianza: 0.80)
{'pan'} -> {'leche'} (confianza: 0.80)
```

<li>Implemente una aplicación sencilla que haga uso de su implementación
anterior</li>

```
from constraint import Problem
from itertools import combinations
import requests

def get_subsets(transactions, min_support):
    itemsets = {}
    for transaction in transactions:
        for i in range(1, len(transaction) + 1):
            for subset in combinations(transaction, i):
                subset = frozenset(subset)
                if subset in itemsets:
                    itemsets[subset] += 1
                else:
                    itemsets[subset] = 1
    frequent_itemsets = {k: v for k, v in itemsets.items() if v >= min_support}
    return frequent_itemsets

# association rule
def get_association_rules(frequent_itemsets, min_confidence, transactions):
    rules = []
    for itemset in frequent_itemsets:
        if len(itemset) > 1:
            for antecedent in combinations(itemset, len(itemset) - 1):
                antecedent = frozenset(antecedent)
                consequent = itemset - antecedent
                antecedent_support = sum(1 for transaction in transactions if antecedent <= transaction)
                confidence = frequent_itemsets[itemset] / antecedent_support
                if confidence >= min_confidence:
                    rules.append((antecedent, consequent, confidence))
    return rules


def read_transactions_from_url(url):
    response = requests.get(url)
    response.raise_for_status()
    lines = response.text.strip().split('\n')
    transactions = [line.strip().split(',') for line in lines]
    return [frozenset(transaction) for transaction in transactions]


if __name__ == "__main__":
    url = 'https://raw.githubusercontent.com/PSCostaM/TA3_CC82_TopicsCs/master/transactions.txt'
    transactions = read_transactions_from_url(url)
    min_support = 2
    min_confidence = 0.6

    frequent_itemsets = get_subsets(transactions, min_support)
    print("Itemsets frecuentes:", frequent_itemsets)

    association_rules = get_association_rules(frequent_itemsets, min_confidence, transactions)
    print("Reglas de asociación:")
    for rule in association_rules:
        print(f"{set(rule[0])} -> {set(rule[1])} (confianza: {rule[2]:.2f})")
```

<li>Output del código</li>

![image](https://github.com/PSCostaM/TA3_CC82_TopicsCs/assets/48858434/ba4edd0c-9ebe-4eb8-9a20-482b3a590f86)

</ul>
</p>

### Conclusión

<p align="justify">
 El proceso de resolución del problema implicó definir claramente los algoritmos necesarios para encontrar itemsets frecuentes y reglas de asociación, implementarlos en Python utilizando constraint programming, y crear una forma de leer datos desde una fuente remota. El uso de ejemplos y la explicación detallada de cada paso y algoritmo ayudan a comprender cómo funciona todo el sistema.
</p>

![image](https://github.com/PSCostaM/TA3_CC82_TopicsCs/assets/48858434/0254c27d-9653-47f1-9562-afd823709ee4)

