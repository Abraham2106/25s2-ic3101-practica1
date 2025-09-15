# Arquitectura de Computadores - Clase 6

## Descripción de la RAM

El **assembler** permite describir no solo la cantidad de bytes que el programa usará para datos, sino también indicar los valores iniciales que pueden existir en algunas partes de la memoria que se está reservando.

---

## Segmento de datos

```nasm
.data
    n dw 3   ; define una palabra (2 bytes) con valor inicial 3
    m dd 0x20 ; define una doubleword (4 bytes) con valor 0x20
    k db ?   ; define un byte sin inicializar
```

**Explicación:**

* `n` es un símbolo definido por el programador. `dw` significa *define word*, reservando 2 bytes que contendrán el valor `3` decimal.
* `m` utiliza `dd` (*define doubleword*), reservando 4 bytes con valor `0x20`.
* `k` usa `db` (*define byte*), y el signo `?` indica que no se inicializa.
* Los datos se colocan de manera secuencial en memoria. No se puede operar directamente sobre la RAM; primero hay que cargar los datos en registros.

---

## Moviendo datos entre memoria y registros

```nasm
mov AX, n  ; mueve 2 bytes de la memoria 'n' al registro AX
mov EBX, n ; mueve 4 bytes de la memoria 'n' al registro EBX
```

* En la arquitectura x86, los registros son la única ubicación en la que se pueden realizar operaciones directas.
* `AX` es un registro de 16 bits, parte del registro `RAX` de 64 bits.

---

## Operandos inmediatos

Se puede cargar un valor directamente a un registro desde la instrucción:

```nasm
mov R10w, 4
```

Aquí, `4` es un **operando inmediato**.

---

## Escribiendo en la RAM

Para pasar un dato de un registro a una dirección de RAM:

```nasm
mov n, DX
```

---

## Jugando con las direcciones

Las arquitecturas permiten usar las direcciones como datos mediante la instrucción `lea` (*Load Effective Address*).

```nasm
mov AX, n   ; AX <- contenido de n (ej. 25)
lea RAX, n  ; RAX <- dirección de n (ej. 0x23185411)
```

---

## Direccionamiento indirecto

```nasm
mov AX, n
lea RBX, n
mov AX, [RBX] ; [] indica que se accede al contenido de la dirección
```

---

## Precaución al usar direccionamiento indirecto

```nasm
mov [EBX], 5 ; incorrecto: el assembler no sabe el tamaño del dato
```

**Corrección (MASM):**

```nasm
mov word ptr [EBX], 5
```

---

## Pasando información entre registros

```nasm
mov RBP, RSP ; copia el valor de RSP en RBP
```

---

## Datos a la RAM

* Se pueden inicializar valores en memoria.
* Para actualizar la RAM con el resultado de un cálculo, primero se hace el cálculo en un registro y luego se mueve a la memoria.

---

## Interrupciones (primera aproximación)

Las interrupciones permiten interactuar con dispositivos periféricos (teclado, mouse, pantalla, red) solicitando al sistema operativo que realice operaciones por nosotros.

* Los datos se colocan en registros generales.
* Se utiliza `int <#int>` para invocar el sistema operativo.
* Ejemplo en Linux:

```nasm
int 0x80
```

---

## Ejemplo de segmento de datos final

```nasm
.data
    n DW 2
    d DW 2
    f DW ?
```
