[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/EbtZGzoI)
[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23681066)


# Tecnológico Nacional de México

# Instituto Tecnológico de Tijuana

# Subdirección Académica

# Departamento de Sistemas y Computación

## Ingeniería en Sistemas Computacionales

## LENGUAJES DE INTERFAZ

## Profesor: MC. René Solis Reyes

## Semestre Enero - Julio 2026

## Práctica 4.2: Implementación de un Mini Cloud Log Analyzer en ARM64

## Autor

César Lepe Garcia - C22212360

## Descripción

Este proyecto implementa un módulo simplificado de análisis de logs desarrollado íntegramente en **ARM64 Assembly (GNU Assembler)**. El programa procesa códigos de estado HTTP suministrados mediante la entrada estándar (stdin) y contabiliza métricas clave, replicando tareas reales de monitoreo y observabilidad utilizadas en la administración de infraestructura cloud.

## Objetivo

Diseñar e implementar, a nivel de arquitectura de máquina, una solución para procesar registros de eventos y detectar condiciones específicas. En esta implementación (Variante A), el programa contabiliza de manera independiente:

-   Respuestas exitosas (2xx)
    
-   Errores del cliente (4xx)
    
-   Errores del servidor (5xx)
    

## Tecnologías y Entorno

-   ARM64 Assembly (GNU Assembler)
    
-   Bash Scripting
    
-   GNU Make
    
-   AWS Ubuntu ARM64
    
-   GitHub
    

## Compilación

Bash

```
make

```

## Ejecución

Bash

```
cat logs.txt | ./analyzer

```

----------

## REPORTE

### 1. Introducción

Los sistemas modernos de cómputo en la nube generan grandes volúmenes de registros que son vitales para la salud de los servicios. Aunque estas tareas de análisis suelen delegarse a lenguajes de alto nivel o herramientas especializadas, desarrollarlo en ensamblador permite entender cómo la CPU procesa el flujo de datos. En esta práctica, el programa lee directamente el flujo de texto de `stdin` mediante llamadas al sistema (syscalls) de Linux, analiza la información a nivel de bytes y clasifica numéricamente los resultados utilizando las convenciones de la arquitectura ARM64.

### 2. Marco teórico

-   **Arquitectura ARM64:** Se emplean registros de 64 bits y un modelo RISC. El procesamiento de los datos se realiza en los registros de propósito general, optimizando los accesos a memoria.
    
-   **Llamadas al sistema (Syscalls):** El puente entre el hardware y el sistema operativo. Mediante la instrucción `svc #0` se invocan las operaciones de `read` para capturar la entrada estándar (descriptor 0) y `write` para imprimir los resultados finales (descriptor 1).
    
-   **Manipulación de datos (ASCII a Entero):** La entrada estándar provee caracteres ASCII. Para evaluar los rangos numéricos de los códigos HTTP, se debe convertir la representación textual a un valor entero, manipulando el valor base hexadecimal de los caracteres.
    

### 3. Desarrollo (Lógica de la Variante A)

El flujo de procesamiento se divide en las siguientes etapas:

1.  **Captura de datos:** Se asigna un buffer en memoria para recibir fragmentos del archivo `logs.txt` a través de la syscall de lectura.
    
2.  **Ciclo de análisis:** El programa itera sobre los bytes leídos. Al identificar un código de tres dígitos, lo convierte a su valor numérico real.
    
3.  **Estructuras condicionales:** Se utilizan instrucciones de comparación (`cmp`) y saltos condicionales (`b.ge`, `b.lt`) para evaluar en qué rango cae el código:
    
    -   **200 - 299:** Incrementa el registro contador de respuestas exitosas.
        
    -   **400 - 499:** Incrementa el registro contador de errores del cliente.
        
    -   **500 - 599:** Incrementa el registro contador de errores del servidor.
        
4.  **Formateo y salida:** Al alcanzar el final de la entrada (EOF), los valores numéricos almacenados en los registros contadores se convierten nuevamente a formato ASCII para poder imprimirse en consola.
    

### 4. Resultados esperados

El programa analiza la totalidad de las líneas proporcionadas en la entrada estándar y emite un reporte exacto de la cantidad de eventos 2xx, 4xx y 5xx, con un tiempo de ejecución mínimo debido a la ausencia de la sobrecarga de un intérprete.

### 5. Análisis y Conclusiones

Resolver un problema de análisis de texto en ensamblador demuestra que operaciones aparentemente sencillas (como leer una línea y evaluar un número) requieren una gestión minuciosa del hardware. Mientras que lenguajes de alto nivel abstraen la gestión de la memoria, el manejo de cadenas y los límites de los buffers, el lenguaje ensamblador obliga a programar cada interacción explícitamente. Sin embargo, esta falta de abstracción resulta en una eficiencia computacional inigualable, siendo ideal para sistemas embebidos críticos donde los ciclos de reloj y la memoria son limitados.

### 6. Autorreflexión

Implementar el procesamiento de datos directamente con la arquitectura ARM64 me permitió comprender a profundidad la relación entre las estructuras de control, los saltos de memoria y las llamadas al sistema. Entender cómo gestionar el estado del programa utilizando únicamente registros me hizo dimensionar la complejidad subyacente de las utilidades que usamos a diario en la terminal.

----------

### 7. Evidencias

#### Asciinema
![enter image description here](https://github.com/tectijuana/4miniloganalizer-CesarLepeITT/blob/main/asciinema/compilacion.gif)
[![asciicast](https://asciinema.org/a/963473.svg)](https://asciinema.org/a/963473)
#### Make
![](https://github.com/tectijuana/4miniloganalizer-CesarLepeITT/blob/main/img/make.png)
#### Make test
![](https://github.com/tectijuana/4miniloganalizer-CesarLepeITT/blob/main/img/make_test.png)

#### Make run
![](https://github.com/tectijuana/4miniloganalizer-CesarLepeITT/blob/main/img/make_run.png)

#### Make clean
![](https://github.com/tectijuana/4miniloganalizer-CesarLepeITT/blob/main/img/make_clean.png)

