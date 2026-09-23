# LeetCode 121: Best Time to Buy and Sell Stock (RTM32)

Solucion implementada en Assembler RTM32 para la materia IRCI.

## Descripcion
Dado un arreglo de precios de acciones en dias consecutivos, se calcula la ganancia maxima posible comprando un dia y vendiendo en un dia posterior.

* **Entrada:** `precios = [7, 1, 5, 3, 6, 4]`
* **Resultado:** `5` (compra a 1, venta a 6)
* **Complejidad Temporal:** O(n)
* **Complejidad Espacial:** O(1)

## Compilacion y Ejecucion
```bash
./rtm32.asm max_profit.rtm -o max_profit.bin
```
El resultado queda almacenado en la posicion de memoria `max_ganancia` y en el registro `$t1`.
