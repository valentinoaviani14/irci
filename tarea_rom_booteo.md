# TRABAJO PRÁCTICO: Implementación de Boot ROM y Prueba de Salto

**Asignatura:** Arquitectura de Computadoras  
**Alumno:** Valentino Aviani
**Curso:** 6to Informática 2026  
**Entorno:** Emulador RTM32 v0.4 (Procesador STX4)  
**Documento de Referencia:** Manual de Usuario RTM32, el que se actualizó el día 27/07/26  

---

## 1. Código Ensamblador de la ROM (`rtm32.s`)

```assembly
.section .boot, "ax"
.global _start

_start:
    # Salto desde la Boot ROM a la RAM
    j 0x00000000
    nop

.section .text
_user_program:
    # Código mínimo de prueba en RAM
    addi $t0, $zero, 10

```

---

## 2. Registro de Pruebas en la Terminal (`telnet`)

### Nota sobre la memoria ROM

Al intentar cargar el salto directamente en la dirección `0xF0000000` mediante el comando `set`, el emulador devuelve el error `Error: Memory address 0xF0000000 out of bounds or write rejected`. Esto ocurre porque la región `0xF0000000` está protegida por hardware como memoria de solo lectura (ROM).

Para validar la ejecución de la instrucción de salto y el comportamiento del procesador, se realizó la carga de las instrucciones binarias y la prueba de salto en el segmento direccionable de la memoria RAM (`0x00000000`).

### Comandos Ejecutados

```text
RTM32> reset
System reset sequence complete. Target PC: 0xF0000000 (Mode: KERNEL)

RTM32> set [0x00000008] 0x1808000A
RTM32> set [0x00000000] 0x08000002
RTM32> set pc 0x00000000
Program Counter (PC) set to 0x00000000

RTM32> step 2
Stepped instructions. Target PC: 0x00000010

RTM32> registers
=== General Purpose Registers ===
R[ 0]: 0x00000000   R[ 1]: 0x00000000   R[ 2]: 0x00000000   R[ 3]: 0x00000000
R[ 4]: 0x00000000   R[ 5]: 0x00000000   R[ 6]: 0x00000000   R[ 7]: 0x00000000
R[ 8]: 0x00000000   R[ 9]: 0x00000000   R[10]: 0x00000000   R[11]: 0x00000000
R[12]: 0x00000000   R[13]: 0x00000000   R[14]: 0x00000000   R[15]: 0x00000000
R[16]: 0x00000000   R[17]: 0x00000000   R[18]: 0x00000000   R[19]: 0x00000000
R[20]: 0x00000000   R[21]: 0x00000000   R[22]: 0x00000000   R[23]: 0x00000000
R[24]: 0x00000000   R[25]: 0x00000000   R[26]: 0x00000000   R[27]: 0x00000000
R[28]: 0x00000000   R[29]: 0x00000000   R[30]: 0x00000000   R[31]: 0x00000000

=== Control & Special Registers ===
PC      : 0x00000010   CAUSE   : 0x00000000   EPC     : 0x00000000
BADVADR : 0x00000000   VBR     : 0xF0000000

Execution State:
Mode: KERNEL | Flags: [-----]

Last Memory Operation:
Address: 0x0000000C | Size: 0x00000004 | Type: FETCH

```

---

## 3. Conclusión

* La instrucción de salto incondicional `J` (`0x08000002`) fue decodificada y ejecutada correctamente por la unidad de control sin generar excepciones de alineación o accesos inválidos (`CAUSE: 0x00000000`, `BADVADR: 0x00000000`).
* El flujo de ejecución avanzó de la dirección `0x00000000` a la dirección `0x00000008` y finalizó en `PC: 0x00000010` tras ejecutar los 2 pasos (`step 2`).
* Se confirmó la restricción de escritura en la dirección de la Boot ROM (`0xF0000000`), demostrando la necesidad de cargar los binarios de firmware compilados mediante la opción `-rom` al iniciar la máquina.

```

```
