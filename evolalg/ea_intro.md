Los algoritmos evolutivos se inspiran en el paradigma neodarwiniano de la evolución natural de las especies.

Su aplicación en la optimización es sumamente interesante porque a diferencia de los métodos clásicos de descenso por gradiente, consideran un conjunto de soluciones potenciales.

## Métodos de descenso por gradiente

Sea $f$ una función de valor real, $f: \mathbb R \rightarrow \mathbb R$, $x \in \mathbb{R} \rightarrow f(x) \in \mathbb{R}$. El problema de minimizar $f$ se puede escribir como encontrar $x^*$ para el cual el valor de $f(x)$ es el mínimo entre todos los posibles valores de $x$. De forma compacta esto se escribe como en \eqref{eq:prob_opt},


$$
  x^* = \arg \min_{x} f(x).
 \label{eq:prob_opt}
$$

Si $f'$ (la derivada de $f$ con respecto a $x$) existe, entonces a partir de una condición inicial $x^{(0)}$ se puede obtener como se muestra en \eqref{eq:desc_grad},

$$
  x^{(t+1)}=x^{(t)} - \eta f'(x^{(t)}).
 \label{eq:desc_grad}
$$

A $\eta$ (pronunciado eta) se le conoce como \`tasa de aprendizaje\` y controla la velocidad a la que la solución $x^{(t)}$ converge.

Para que lo anterior funcione se requiere que exista al menos un mínimo global. Un **mínimo global** $x^{*}$ es tal si $f(x^{*})$ es más pequeño que $f(x)$ para todos los $x$ cercanos a $x^{*}$ (o formalmente, si $f(x^{*}) \leq f(x)$ para todos los $x$ tales que $d^{2}(x,x^{*}) \leq \epsilon$, para algún $\epsilon > 0$ y $d$ una [función de distancia](https://en.wikipedia.org/wiki/Distance#:~:text=A%20metric%20or%20distance%20function,distinct%20objects%20is%20always%20positive.)).

Un mínimo $x^*$ es un **mínimo global** si $f(x^{*})$ entre todos los posibles valores de $x$ (o formalmente, si $f(x^{*}) \leq f(x)$ para todos los $x$).

El método del descenso por gradiente se puede formalizar en estos tres pasos:

1. Defina $x^{(0)}$,

2. Compute $x^{(1)}=x^{(0)} - \eta f'(x^{(0)})$,

3. Mientras $|(x^{(t+1)}-x^{(t)})|>\epsilon_{prec}$ haga $x^{(t+1)}=x^{(t)} - \eta f'(x^{(t)})$.

El valor $\epsilon_{prec}$ es el nivel de precisión que se define para determinar la convergencia del método.

### Ejemplo: el mínimo de una parábola

Sea $f(x)=x^2$. La derivada es $f'(x)=2x$. Al aplicar la regla de descenso por gradiente  se obtiene que $x^{(t+1)}=x^{(t)} - \eta 2x^{(t)}=x^{(t)}\times (1-2\eta)$. Esta función tiene su mínimo en $x=0$.

Además, 

$$
x^{(t+1)}=x^{(t)} - \eta 2x^{(t)}=x^{(t)}\times (1-2\eta) = x^{(t-1)}\times (1-2\eta) \times (1-2\eta) = x^{(t-1)}\times (1-2\eta)^2,
$$

así que, en general, se puede escribir $x^{(t+1)}=x^{(0)}\times (1-2\eta)^t$. Además, si $0<(1-2\eta)<1$, entonces $(1-2\eta)^t \rightarrow \0$ cuando $t \rightarrow infty$.

En este sencillo ejemplo vemos que, no importa cuál sea la condición inicial $x_{(0)}$, la convergencia está asegurada.

#### Implementación

A continuación se ilustra la implementación en Python del método de descenso por gradiente para este caso.

Este código describe la función y su primera derivada:

```python
def f(x):
  return x**2
  
def f_1(x):
  return 2*x
```

El siguiente código describe la implementación del método de descenso por gradiente:

```python
def min_desc_grad(fun,deriv,x0,eta,eps):
  cambio = eps
  x_t = x0
  x_t1 = x0 - eta * deriv(x0)
  while (cambio>=eps):
    x_t = x_t1
    x_t1 = x_t - eta * deriv(x_t)
    print(x_t1)
    cambio = abs(x_t1-x_t)
  return x_t1
```

Ahora se ejecuta el código:

```python
min_desc_grad(f,f_1,2,0.5,0.000000001)
```

y se obtiene


```python
0.01999999999999999
0.0019999999999999983
0.00019999999999999987
1.999999999999997e-05
1.999999999999998e-06
1.9999999999999978e-07
1.9999999999999967e-08
1.9999999999999947e-09
1.9999999999999952e-10
1.9999999999999957e-11
1.9999999999999957e-11
```

### Funciones de valor real

Si $f$ es una función de valor real, ie., $f: \mathbb R^p \rightarrow \mathbb R$, $x \in \mathbb{R}^p \rightarrow f(x) \in \mathbb{R}$, entonces la regla del descenso por gradiente se expresa como en \eqref{eq:desc_grad_mult},


$$
  x^{(t+1)}=x^{(t)} - \eta \nabla f (x^{(t)}),
 \label{eq:desc_grad_mult}
$$

donde $\nabla f$ representa el vector gradiente de $f$.

### Gradiente complicado

Cuando obtener una expresión simple para el gradiente es complicado es posible reemplazar $\nabla f (x^{(t)})$ por una aproximación usando el método de [diferencias finitas](https://en.wikipedia.org/wiki/Finite_difference), usando la expresión \eqref{eq:dif_finitas} para cada coordenada $x_{j}^{(t)}$, $j=1,2,\ldots,p$,

$$
  \frac{\partial}{\partial x_j} f (x^{(t)})= \frac{f (x^{(t)} + h \times u_j) - f (x^{(t)} - h \times u_j)}{2h},
 \label{eq:dif_finitas}
$$

donde $u_j \in \mathbb{R}^p$ es un vector cuyas componente $j$-ésima componente es 1 y las otras son cero. La constante $h>0$ es un valor pequeño que define la precisión de la aproximación.


### Limitaciones

Hasta aquí hay dos limitaciones que saltan a la vista:

1. Para aplicar los métodos de descenso por gradiente es necesario contar con el gradiente o que el gradiente se deje aproximar razonablemente.

2. Si hay varios óptimos globales, la convergencia a alguno de ellos dependerá de la condición inicial.

Una forma de lidiar con la presencia de varios óptimos, o **multimodalidad**, es ejecutar el algoritmo con diferentes condiciones iniciales.


## Algoritmos evolutivos

Los Algoritmos Evolutivos (AE) son métodos de solución de problemas que utilizan procesos operativos provenientes del paradigma Neo-Darwiniano de la evolución biológica [@holland1992]. Procesos tales como los operadores genéticos de reproducción, mutación, competencia y la selección del más apto. Los Algoritmos Evolutivos (EA) emulan el mecanismo de la selección natural y la herencia genética y se usan básicamente para realizar búsqueda, clasificación, exploración y optimización en un espacio de trabajo determinado.  Los AE se han aplicado en casi todas las disciplinas [@fleming2002,@wang1992]. 

Los AE, en general, comienzan con una población de posibles soluciones a ensayar (no necesariamente correctas ni conocidas) para el problema en estudio.  Luego se crean, aleatoriamente, nuevas soluciones a probar, modificando las existentes por medio de los **operadores genéticos**.  Para conocer si una solución ensayada es aceptable, se usa un indicador de desempeño llamado \index{Función de Aptitud}**Función de Aptitud**, y a partir del **error** encontrado se selecciona las soluciones que se conservarán como padres para la siguiente generación.


# Bibliografía
