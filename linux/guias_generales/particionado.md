Aquí vamos a hablar sobre cómo hacer las particiones, cada herramienta de particionado tiene su propia guía.
En esta sección encontrarás las guías detalladas para realizar el proceso. Puedes elegir la herramienta que mejor se adapte a tu nivel de experiencia:

* 🖥️ **Guía de `cfdisk` (Recomendado):** Una interfaz basada en texto (TUI) con menús interactivos. Es la opción ideal si buscas un proceso visual, intuitivo y con menor riesgo de equivocarte.
* ⌨️ **Guía de `fdisk` (Avanzado):** La herramienta clásica de Linux que se maneja puramente a través de comandos en la terminal. Ideal si prefieres un control absoluto y directo.
>[!NOTA]
>En esta guía haremos las particiones separando las particiones Raiz(/) y home, aunque se puede hacer todo en una, no es lo recomendable.
---

# Guía de Particionado de Discos

El particionado es el proceso de dividir tu unidad de almacenamiento (SSD o HDD) en secciones lógicas independientes. Esto es fundamental, ya que nos permite definir exactamente dónde se guardará el sistema operativo, los archivos de arranque y tus datos personales.

Dividimos esta guía en dos secciones según la Bios/Firmware de la placa base, es decir, si la computadora arranca usando el sistema antiguo **BIOS/MBR** o el sistema moderno **UEFI/GPT**.

Si no sabes qué sistema usa tu ordenador, este comando funciona en todas las distribuciones de Linux:
```bash
[ -d /sys/firmware/efi ] && echo "UEFI" || echo "BIOS"
```

### ¿Qué hace este comando?

**Desglose paso a paso:**

* **`[ -d /sys/firmware/efi ]` (La Condición)**
  * `[` y `]`: En Bash, los corchetes son en realidad un comando (equivalente al comando `test`) que sirve para evaluar una condición.
  * `-d`: Es un operador que pregunta: *"¿Existe este directorio?"*.
  * `/sys/firmware/efi`: Es la ruta del directorio que se está buscando. Si tu sistema inició en modo UEFI, la placa base crea automáticamente esta carpeta en el sistema de archivos de Linux. Si inició en modo BIOS, esta carpeta no existe.

* **`&& echo "UEFI"` (Si la condición se cumple)**
  * `&&`: Es un operador lógico ("Y"). En la línea de comandos, funciona como un: *"Si lo anterior fue exitoso (verdadero), entonces haz lo siguiente"*.
  * `echo "UEFI"`: Simplemente muestra la palabra "UEFI" en la pantalla. Solo se ejecutará si la carpeta `/sys/firmware/efi` realmente existe.

* **`|| echo "BIOS"` (Si la condición NO se cumple)**
  * `||`: Es un operador lógico ("O"). Funciona como un: *"Si lo anterior falló (falso), entonces haz esto"*.
  * `echo "BIOS"`: Muestra la palabra "BIOS" en la pantalla. Se ejecutará si el directorio no existe (lo que significa que el sistema no es UEFI).

---

Ahora hablemos sobre cada sistema.

# BIOS / MBR

## 💾 ¿Qué es BIOS? (Basic Input/Output System)

La **BIOS** es un firmware (un software básico) integrado en un chip de la placa base de tu ordenador. Cuando pulsas el botón de encendido, la BIOS es lo primero que se despierta.

* **Su función principal:** Realiza un chequeo rápido del hardware (llamado *POST*) para comprobar que la memoria RAM, el procesador y los discos funcionen correctamente.
* **El arranque:** Una vez que todo está en orden, la BIOS busca un disco que tenga un sector de arranque para cederle el control y que empiece a cargar el sistema operativo.

---

## 🛠️ ¿Qué es MBR? (Master Boot Record)

El **MBR** es el sistema de tabla de particiones que utiliza la BIOS. Es literalmente el **primer sector** de tu disco duro (el sector `0`) y mide apenas 512 bytes.

Dentro de esos 512 bytes, el MBR guarda tres cosas cruciales:
* **El código de arranque principal (*Bootloader*):** Las instrucciones para que el ordenador sepa dónde está el sistema operativo.
* **La tabla de particiones:** El mapa que le dice al ordenador cómo está dividido el disco.
* **La firma de arranque:** Un código que confirma que el sector no está dañado.

---

## ⚠️ Limitaciones Importantes de BIOS/MBR

Al ser una tecnología diseñada en la década de 1980, MBR tiene dos limitaciones muy importantes hoy en día:

### 1. El límite de 2 Terabytes (2 TB)
Debido a la forma en que MBR calcula el tamaño del disco (usando direcciones de 32 bits), **no puede reconocer más de 2 TB de espacio**. Si instalas un disco de 4 TB en un sistema MBR, 2 TB se quedarán completamente inutilizables.

### 2. Máximo 4 particiones primarias
Por falta de espacio en esos 512 bytes del sector, MBR solo puede registrar un máximo de **4 particiones primarias**.

> [!WARNING]
> Si necesitas crear más divisiones (por ejemplo, para tener varios sistemas operativos o separar tus datos), tienes que recurrir a un truco:
> 1. Sacrificar una partición primaria para convertirla en una **partición extendida**.
> 2. Dentro de esa partición extendida, puedes crear múltiples **particiones lógicas** (que actúan como subparticiones).

  ¿Qué pariciones crear?  
### 📂 Raíz (`/`)

La primera que debemos crear es la partición **Raíz**, con el punto de montaje `/`. Es bastante recomendable ponerla en una partición primaria.

> [!IMPORTANT]
> **¿Por qué debe ser la primera?** > En sistemas BIOS/MBR, la partición raíz (o en su defecto, la partición donde residen los archivos de arranque) actúa como el volumen de inicio (*Boot*). Por lo tanto, debe estar ubicada al principio del disco para asegurar que la BIOS pueda leerla sin inconvenientes y evitar problemas de arranque.

En esta partición se instalan todos los **archivos del sistema**, las aplicaciones, las configuraciones globales del entorno y los controladores necesarios para el correcto funcionamiento del sistema operativo Linux.

### 📏 Tamaño recomendado para la Raíz (`/`)

El tamaño que le asignaremos depende del uso que le vayamos a dar al sistema operativo, pero lo podemos dividir en estas tres circunstancias:

*  **20 GB a 30 GB:** Es suficiente para distribuciones ligeras o si vas a instalar muy pocos programas.
*  **40 GB a 60 GB:** Este es el punto ideal para la mayoría de usuarios. Te permite usar el sistema con total tranquilidad en el día a día.
*  **80 GB a 100 GB:** Necesario para *gamers* o si planeas instalar entornos de desarrollo pesados (como *Android Studio*), herramientas de `Docker`, o si usas `Flatpak`/`Snap` con frecuencia (ya que estos formatos de paquetes empaquetan sus propias dependencias y ocupan mucho más espacio).

### Swap



