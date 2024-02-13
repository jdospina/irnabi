# El juego de la vida

El juego de la vida es un juego sin jugadores. En un arreglo matricial binario, se definen reglas para que cada entrada de la matriz (célula) viva o muera en la siguiente generación. El sitio web [Conway's Game of Life](https://playgameoflife.com){:target="_blank"} permite experimentar con el juego.

El [repositorio git ](https://github.com/mwharrisjr/Game-of-Life/tree/master){:target="_blank"} de mwharrisjr contiene un código Python que implementa el juego.

## Reglas
### Celdas vivas
Una celda viva

1. Muere de soledad si no tiene vecinos vivos
2. Muere por saturación si tiene cuatro o más vecinos vivos
3. Sigue viva en la siguiente generación/iteración si tiene dos o tres vecinos vivos

### Celdas muertas
1. Una celda muerta con exactamente tres vecinos vive en la siguiente generación

[Entrada de Wikipedia sobre el juego de la vida](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)

## Referencias
Johnston, N., & Greene, D. (2022). Conway's Game of Life: Mathematics and Construction. Nathaniel Johnston. [Ver en Amazon.com](https://www.amazon.com.mx/Conways-Game-Life-Mathematics-Construction/dp/1794816968){:target="_blank"}

[Contenido](./content.md)
[Principal](./README.md)
