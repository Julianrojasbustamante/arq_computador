### ENTREGA 2 SEMANA 5
### ARQUITECTURA DEL COMPUTADOR
### GRUPO B01

DETERMINAR SI UN NUMERO ES PRIMO O NO

### PROFESOR:
IGNACIO BLANCO

### INTEGRANTES DEL GRUPO:
JULIAN ROJAS BUSTMANTE.

JHON ALEXANDER RAMIREZ RAMIREZ.

CARLOS ANDRES CASTILLO PAJARO.

DIANA CAROLINA TOVAR HERNANDEZ.

### POLITECNICO GRAN COLOMBIANO - 2022 



### INTRODUCCION
La continua evolución tecnológica nos ha llevado cada vez a un lugar superior en términos de avance y descubrimientos. La gran cantidad de herramientas tecnológicas es ahora el gran generador de productividad a comparación de siglos anteriores; La arquitectura del computador, los continuos avances en semiconductores, además de los constantes avances y mejoramientos tecnológicos han logrado una mayor capacidad en términos de almacenamiento, administración de información y fluidez en comunicaciones. Es esta la principal finalidad de la Arquitectura del computador la que nos permite aprovechar completamente el potencial tecnológico de este, conociendo así los fundamentos principales que lo rigen.

### OBJETIVOS

### OBJETIVO GENERAL
Identificar y entender la microarquitectura del hardware.
### OBJETIVO ESPECÍFICO

- Diseñar un código de alto nivel que dé solución al problema.
- Especificar el tipo de instrucciones en alto nivel para determinar si un número es primo o no.


### DISEÑO DEL ALGORITMO EN LENGUAJE DE PROGRAMACION DE ALTO NIVEL
```js

let numero = 5;
let x = 1;
let contador = 0;

while (x <= numero) {
  if (numero % x === 0) {
    contador += 1;
  }

  x += 1;
}

if (contador === 2) {
  console.log(`El número ${numero} es primo`);
} else {
  console.log(`El número ${numero} no es primo`);
}
```

### ESPECIFICACION DE INSTRUCCIONES
| INSTRUCCIÓNES (R, I, J) | INSTRUCCIÓN ALTO NIVEL | DEFINICION |
| :---        |    :----:   |          :--- |
| add      | let numero = 5;       | Se guarda el número en una variable llamada “numero”. |
| addi    | let x = 1;        | Se crea una variable para verificar más adelante en el ciclo que este no |
| add      | let contador = 0;       | Contador para que sirva de referencia en el ciclo while siguiente.   |
| beq   | while (x <= numero)        | Mientras que la condición se cumpla, se evalua si el número guardado en la variable “numero”. if (numero % x === 0) { contador += 1;} En cada iteración del ciclo, verificamos si el residuo de la “numero” con “x” es igual a cero, de ser así, le sumamos 1 a la variable “contador”. Por lo tanto, la variable “contador” se va incrementado de uno en uno, cada que esta condición se cumple. Como sabemos los números primos son aquellos que únicamente tienen dos divisores, el mismo número y el número 1.      |
| add   | x = x + 1        | Incrementamos la variable “x” en uno, para continuar dividiendo el número ingresado en la variable “numero”, por todos los números anteriores a dicho número. if (contador === 2) { console.log(`El número ${numero} es primo`); } else { console.log(`El número ${numero} no es primo`); } Si la condición se cumple, entonces mostramos el mensaje de que el número es primo. Si no, mostramos mensaje que dice que no lo es. Fin del algoritmo.      |

### DEFINICION DE LA CANTIDAD DE REGISTROS
| Nombre | Bytes | Uso                                                                                                           |
|:------:| :---: |:--------------------------------------------------------------------------------------------------------------|
|  $v0   | 4	| Cambia constantemente de valores, corresponde a un valor de retorno.                                          |
|  $a0   | 1	| Funciona como argumento en la función, genera un input de texto donde el numero es ingresado para validación. |
|  $a1   | 5	| Registro reservado por el sistema.                                                                            |
|  $t0   | 1	| Registro que almacena el valor ingresado el cual será posteriormente comparado.                               |
|  $t1   | 2	| Registro de espacio en memoria que almacena la constante con la que realizamos comparación.                   |
|  $t2   | 1	| Registro de desbordamiento, almacena la operación matemática, limitándola a 32 bits de salida.                |

### LISTADO DE INSTRUCCIONES

| NOMBRE         | MNEMONICO   | DESCRIPCION                                                   | EJEMPLO                   |
| :---           |    :----:   |                                                          :--- |                      :--- |
|Load address    | La          | Carga en $t0 el contenido de la palabra almacenada en dir     | La $t0,dir                |
| Adition        | Add         | Sumar $t2 + $t3 y almacenar en $t1                            | Add $t1, $t2,$t3          |
| Branch if equal| Beq         | Comparativo entre registros $t0 $t1                           | Beq $t0 $t1 es_primo      |
| Divide         | Div         | División directa                                              | $t2, $t3                  |
| Load integer   | Li          | Carga inmediatamente en el registro $v0                       | Li $v0                    |
| Move           | Move        | Mover a $t1 el registro de $t2	                               | $t1, $t2                  |
| Syscall        | Syscall     | Emite una llamada al sistema                                  | Syscall                   |
| Branch         | B           | Salta a ejecutar la instrucción etiquetada incondicionalmente | B exit                    |
| Move from HI register| Mfhi | Establece $t1 en el contenido de HI(observa operaciones de multiplicación y división      | Mfhi $t2  |

### MODOS DE DIRECCIONAMIENTO

### Direccionamiento inmediato

El operando se proporciona en el byte o bytes que siguen al código de la instrucción.

![Imgur](https://i.imgur.com/T2YTOga.jpg)

### Direccionamiento absoluto

Un registro definido por la instrucción contiene la dirección en memoria donde está el operando

![Imgur](https://i.imgur.com/5Pv00Zr.jpg)

### Traducción de lenguaje de alto nivel a lenguaje ensamblador
```
.data
    intro:        .asciiz ¨\n Validar numero Primo¨
    ingresar_num: .asciiz ¨\n Ingresar numero:¨
    es_primo:     .asciiz ¨\n Es primo:¨
    no_primo:     .asciiz ¨\n No es primo:¨
.text
    main:
        li $v0 4
        la $a0 intro
        syscall
        la $a0 ingresar_num
        syscall     
        li $v0 5
        syscall
        move $t0 $v0
        li $t1 2
    loop:
        beg $t0 $t1 es_primo
        div $t0 $t1
        mfh1 $t2
        beqz $t2 no_primo
        addi $t1 $t1 1
        b loop
    no_es_primo:
        li $v0 4
        la $a0 no_primo
        syscall
        b exit
    si_es_primo:
        l1 $v0 4
        la $a0 es_primo
        syscall 
        b exit
    exit:
        l1 $v0 10
        syscall
```

### LISTADO BINARIO A HEXADECIMAL QUE REPRESENTA EL PROGRAMA
| Binario                          | Hexadecimal |
|:---------------------------------|------------:|
| 00100100000000100000000000000100 |  0x24020004 |
| 00111100000000010001000000000001 |  0x3c011001 |
| 00110100001001000000000000000000 |  0x34240000 |
| 00000000000000000000000000001100 |  0x0000000c |
| 00111100000000010001000000000001 |  0x3c011001 |
| 00110100001001000000000000011111 |  0x3424001f |
| 00000000000000000000000000001100 |  0x0000000c |
| 00100100000000100000000000000101 |  0x24020005 |
| 00000000000000000000000000001100 |  0x0000000c |
| 00000000000000100100000000100001 |  0x00024021 |
| 00100100000010010000000000000010 |  0x24090002 |
| 00010001000010010000000000001010 |  0x1109000a |
| 00000001000010010000000000011010 |  0x0109001a |
| 00000000000000000101000000010000 |  0x00005010 |
| 00010001010000000000000000000010 |  0x11400002 |
| 00100001001010010000000000000001 |  0x21290001 |
| 00000100000000011111111111111010 |  0x0401fffa |
| 00100100000000100000000000000100 |  0x24020004 |
| 00111100000000010001000000000001 |  0x3c011001 |
| 00110100001001000000000000111110 |  0x3424003e |
| 00000000000000000000000000001100 |  0x0000000c |
| 00000100000000010000000000000101 |  0x04010005 |
| 00100100000000100000000000000100 |  0x24020004 |
| 00111100000000010001000000000001 |  0x3c011001 |
| 00110100001001000000000001010110 |  0x34240056 |
| 00000000000000000000000000001100 |  0x0000000c |
| 00000100000000010000000000000000 |  0x04010000 |
| 00100100000000100000000000001010 |  0x2402000a |
| 00000000000000000000000000001100 |  0x0000000c |

### ALU EN LOGISIM

![Imgur](https://i.imgur.com/P5YxDV4.png)

![Imgur](https://i.imgur.com/qkGLXJb.png)

![Imgur](https://i.imgur.com/6ULh2u8.png)

![Imgur](https://i.imgur.com/yUw3Kdt.png)

![Imgur](https://i.imgur.com/dfcNFiR.png)

![Imgur](https://i.imgur.com/Taxx719.png)

![Imgur](https://i.imgur.com/2Onyyd0.png)

![Imgur](https://i.imgur.com/GSmjpLH.png)

![Imgur](https://i.imgur.com/LGmjH3z.png)


### CONCLUSIONES

-	El algoritmo de alto nivel permite determinar si un numero se primó basado en el set de instrucciones requeridas para el lenguaje de máquina.
-	A medida que el número va creciendo es más complejo validarse si el número es primo, por tal motivo el código nos va a hacer más útil para poder detectar rápidamente.
-	Se pudieron evidenciar las diferencias de los tipos de instrucción y aplicabilidad de cada uno.
-	Se realizó la validación de todas las entradas y salidas para lograr interpretar el lenguaje de la maquina en el ALU ensamblado en Logisim.
-	Las instrucciones permitieron identificar los modos de direccionamiento que aplican para el problema planteado, poniendo en práctica lo aprendido en lo corrido del módulo.


### REFERENCIAS BIBLIOGRÁFICAS

- Marker, G. (2020, marzo 22). Pseudocódigo: ¿Qué es? Ejemplos. Tecnología + Informática; Tecnología+Informatica. http://www.tecnologia-informatica.com/pseudocodigo/

- Pseudocódigo. (s/f). Ecured.cu. Recuperado el 13 de septiembre de 2022, de http://www.ecured.cu/Pseudoc%C3%B3digo

- (S/f). Edu.co. Recuperado el 13 de septiembre de 2022, de https://tesuva.edu.co/phocadownloadpap/Arquitectura_computadoras_I.pdf

