# Optimización de la Restauración Ecológica en la Cuenca del Río Cauca mediante Programación Lineal

**Asignatura:** Programación Lineal / Investigación de Operaciones
**Institución:** <!-- Nombre de la universidad -->
**Semestre:** <!-- Semestre y año -->
**Autor(es):** <!-- Nombre(s) -->

---

## Descripción

Este repositorio contiene el desarrollo completo de un proyecto universitario que formula y resuelve el problema de **minimización del costo de restauración ecológica** en la cuenca del río Cauca como un problema de programación lineal. El trabajo tiene dos componentes principales:

1. **Experimento ecológico:** se construye un modelo primal de minimización de costos de intervención por zonas candidatas, sujeto a restricciones de cobertura ecológica, conectividad y capacidad operativa institucional. La solución dual se analiza para extraer los **precios sombra** de cada restricción, identificando los cuellos de botella del proceso restaurador.

2. **Experimento computacional:** se compara el número de iteraciones del método Símplex al resolver la formulación primal versus la dual sobre instancias generadas con distintas relaciones $n/m$ (zonas candidatas / restricciones), evaluando cuándo conviene resolver cada formulación y si el comportamiento empírico concuerda con las predicciones teóricas de Borgwardt (1987).

---

## Estructura del repositorio

```
├── notebooks/
│   └── experimento_iteraciones.ipynb   # Comparación Primal vs Dual
├── src/
│   ├── modelo_primal.py                # Formulación y solución del primal
│   ├── modelo_dual.py                  # Formulación y solución del dual
│   └── estimacion_iteraciones.py       # Función de estimación sintética
├── docs/
│   ├── introduccion.docx               # Sección 1 del informe
│   └── planteamiento.docx              # Sección 2 del informe
├── referencias.bib                     # Bibliografía en formato BibTeX
└── README.md
```

---

## Contexto del problema

La cuenca del río Cauca abarca aproximadamente 63.300 km² y atraviesa cinco departamentos colombianos. Décadas de deforestación y expansión agropecuaria han reducido la cobertura boscosa ribereña a menos del 15% de su extensión original. Frente a compromisos nacionales de restauración (Plan Nacional de Restauración, Desafío de Bonn), las autoridades ambientales deben asignar recursos limitados —presupuesto, personal técnico, material vegetal— entre cientos de zonas candidatas de intervención.

El problema se formaliza como:

$$\min \sum_{j=1}^{n} c_j x_j$$

$$\text{s.a.} \quad \sum_{j=1}^{n} a_{ij} x_j \geq b_i, \quad i = 1, \dots, k \quad \text{(metas ecológicas)}$$

$$\sum_{j=1}^{n} a_{ij} x_j \leq b_i, \quad i = k+1, \dots, m \quad \text{(restricciones de capacidad)}$$

$$x_j \geq 0, \quad j = 1, \dots, n$$

donde $x_j$ es el número de hectáreas a restaurar en la zona $j$, $c_j$ el costo unitario por hectárea y $b_i$ los límites de cada restricción.

---

## Experimentos

### Experimento 1 — Modelo de restauración y análisis dual

- Construcción del modelo primal con datos sintéticos representativos de la cuenca Cauca.
- Resolución con `scipy.optimize.linprog` (método Símplex).
- Extracción e interpretación de los **precios sombra** $y_i^*$: cuánto cambia el costo mínimo ante una unidad adicional de presupuesto, material vegetal o relajación de una meta ecológica.
- Verificación de la **holgura complementaria**: $y_i^*(b_i - A_i x^*) = 0$.

### Experimento 2 — Comparación de eficiencia Primal vs. Dual

- Generación de instancias aleatorias variando $n \in \{5, 10, 20, 50, 100\}$ con $m$ fijo, y viceversa.
- Medición del número real de iteraciones del Símplex para cada formulación.
- Comparación con la estimación teórica $O(m^{1/3} \cdot n^{2/3})$ de Borgwardt (1987).
- Análisis del efecto de la dispersidad de $A$ sobre el comportamiento observado.

---

## Requisitos

```bash
pip install numpy scipy matplotlib pulp
```

Versiones usadas:

| Paquete      | Versión |
|-------------|---------|
| `numpy`     | ≥ 1.24  |
| `scipy`     | ≥ 1.11  |
| `matplotlib`| ≥ 3.7   |
| `pulp`      | ≥ 2.7   |

---

## Cómo ejecutar

**Modelo primal y análisis dual:**
```bash
python src/modelo_primal.py
```

**Comparación de iteraciones:**
```bash
jupyter notebook notebooks/experimento_iteraciones.ipynb
```

O directamente:
```bash
python src/estimacion_iteraciones.py
```

---

## Resultados esperados

- Mapa de precios sombra por tipo de restricción, identificando cuáles zonas y recursos son los verdaderos cuellos de botella del plan de restauración.
- Curvas de iteraciones Primal vs. Dual en función de la relación $n/m$, con identificación empírica del umbral de cruce de eficiencia.
- Evaluación de si la estructura dispersa y no negativa de $A$ modifica las predicciones teóricas estándar.

---

## Referencias principales

| Clave | Referencia |
|---|---|
| Dantzig (1963) | Dantzig, G.B. *Linear Programming and Extensions*. Princeton University Press. |
| Borgwardt (1987) | Borgwardt, K.H. *The Simplex Method: A Probabilistic Analysis*. Springer. DOI: [10.1007/978-3-642-61578-8](https://doi.org/10.1007/978-3-642-61578-8) |
| Smale (1983) | Smale, S. On the average number of steps of the simplex method. *Mathematical Programming*, 27, 241–262. DOI: [10.1007/BF02591902](https://doi.org/10.1007/BF02591902) |
| Bazaraa et al. (2010) | Bazaraa, M.S., Jarvis, J.J., Sherali, H.D. *Linear Programming and Network Flows*, 4ª ed. Wiley. ISBN: 978-0-470-46272-0 |
| Vanderbei (2014) | Vanderbei, R.J. *Linear Programming: Foundations and Extensions*, 4ª ed. Springer. DOI: [10.1007/978-1-4614-7630-6](https://doi.org/10.1007/978-1-4614-7630-6) |
| Williams et al. (2024) | Williams, B.A. et al. Bringing the forest back: Restoration priorities in Colombia. *Diversity and Distributions*, 30(4), e13821. DOI: [10.1111/ddi.13821](https://doi.org/10.1111/ddi.13821) |
| González-Montañez et al. (2023) | González-Montañez, J. et al. Harmonization approach to spatial and social techniques to define landscape restoration areas. *Forests*, 14(9), 1913. DOI: [10.3390/f14091913](https://doi.org/10.3390/f14091913) |
| Duarte Hernández y Avella Muñoz (2018) | Duarte Hernández, D., Avella Muñoz, E.A. Análisis socio-ecológico de una iniciativa de restauración en Santander. *Colombia Forestal*, 22(1), 68–86. DOI: [10.14483/2256201X.13101](https://doi.org/10.14483/2256201X.13101) |

La bibliografía completa en formato BibTeX está en [`referencias.bib`](./referencias.bib).

---

## Licencia

Este proyecto es de uso académico. Se permite su consulta y reutilización con atribución al autor original.
