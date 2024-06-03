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

  <li> </li>
  
</ul>
</p>
