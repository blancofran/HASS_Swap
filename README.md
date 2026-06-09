# Creación Automática de Archivo SWAP de 2 GB

## Descripción

Este script crea y administra automáticamente un archivo de memoria SWAP de 2 GB en sistemas Linux.

Su objetivo es proporcionar memoria virtual adicional cuando la memoria RAM disponible resulta insuficiente para ejecutar aplicaciones o servicios que consumen una cantidad significativa de recursos.

El script verifica si el archivo SWAP existe, si está activo y, dependiendo de su estado, lo crea, activa o reinicia automáticamente.

---

## ¿Qué es la memoria SWAP?

La memoria SWAP es un espacio reservado en disco que Linux utiliza como memoria virtual cuando la memoria RAM física disponible se reduce.

Cuando el sistema necesita más memoria de la que tiene disponible en RAM, puede mover temporalmente información menos utilizada hacia el área SWAP.

### Ventajas

- Reduce errores por falta de memoria.
- Permite ejecutar aplicaciones más exigentes.
- Ayuda a evitar cierres inesperados por agotamiento de RAM.

### Desventajas

- Es considerablemente más lenta que la memoria RAM.
- Un uso excesivo puede afectar el rendimiento general del sistema.

---

## Ubicación del Archivo

El archivo SWAP se crea en:

text /backup/_swap.swap 

Tamaño:

text 2 GB 

---

## Funcionamiento

### Escenario 1: El archivo no existe

Si el archivo SWAP no existe:

bash if [ ! -f /backup/_swap.swap ] 

El sistema:

1. Crea un archivo de 2 GB.
2. Lo formatea como área SWAP.
3. Aplica permisos seguros.
4. Lo activa inmediatamente.

Comandos ejecutados:

bash fallocate -l 2G /backup/_swap.swap mkswap /backup/_swap.swap chmod 0600 /backup/_swap.swap swapon /backup/_swap.swap 

Mensaje generado:

text SWAP_NEW_FILE_CREATED_USED 

---

### Escenario 2: El archivo existe pero no está activo

Si el archivo ya existe pero no está siendo utilizado como SWAP:

bash swapon /backup/_swap.swap 

Mensaje generado:

text SWAP_USING_OLD_FILE 

---

### Escenario 3: El archivo existe y ya está activo

Si el archivo existe y aparentemente ya está siendo utilizado:

bash swapoff /backup/_swap.swap swapon /backup/_swap.swap 

Esto reinicia el área SWAP.

Mensaje generado:

text SWAP_FINAL_ELSE_STATEMENT 

---

## Permisos Aplicados

El script utiliza:

bash chmod 0600 /backup/_swap.swap 

Lo que significa:

| Usuario | Permisos |
|----------|----------|
| Propietario | Lectura y escritura |
| Grupo | Sin acceso |
| Otros | Sin acceso |

Esto es una práctica recomendada para archivos SWAP.

---

## Verificación

### Consultar las áreas SWAP activas

bash swapon --show 

o

bash cat /proc/swaps 

Salida esperada:

text Filename                Type      Size      Used    Priority /backup/_swap.swap      file      2097148   0       -2 

---

### Consultar el uso de memoria

bash free -h 

Ejemplo:

text                total        used        free Mem:           2.0Gi       1.2Gi       800Mi Swap:          2.0Gi         0Mi       2.0Gi 

---

## Casos de Uso

Este tipo de configuración suele utilizarse en:

- Home Assistant OS.
- Home Assistant Supervised.
- Raspberry Pi.
- Mini PCs con poca RAM.
- Máquinas virtuales pequeñas.
- Contenedores que ejecutan modelos de IA locales.
- Sistemas que ejecutan múltiples complementos simultáneamente.

---

## Recomendaciones

### Utilizar almacenamiento rápido

La SWAP se encuentra en disco, por lo que se recomienda:

- SSD
- NVMe
- Almacenamiento eMMC

Evitar en lo posible:

- Tarjetas microSD antiguas o de baja calidad.
- Unidades USB lentas.

---

### Supervisar el uso de SWAP

Puede verificarse mediante:

bash free -h 

o

bash htop 

Si la SWAP se utiliza constantemente, puede ser una señal de que el sistema necesita más memoria RAM.

---

## Consideraciones Importantes

- La SWAP no reemplaza la memoria RAM.
- Su función principal es proporcionar estabilidad adicional al sistema.
- Un uso ocasional de SWAP es normal.
- Un uso permanente o elevado puede indicar insuficiencia de memoria física.
- Reiniciar la SWAP periódicamente normalmente no es necesario, aunque este script lo hace cuando detecta que ya está activa.

---

## Licencia

Uso libre para proyectos personales, educativos y de automatización.

Puede adaptarse y modificarse según las necesidades de cada entorno Linux.
