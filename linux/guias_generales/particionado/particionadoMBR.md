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
  Un ejemplo general es crear una partición Raíz, otra de home, y una Swap (opcional).
### 📂 Raíz (`/`)

La primera que debemos crear es la partición **Raíz**, con el punto de montaje `/`. Es bastante recomendable ponerla en una partición primaria.

> [!IMPORTANT]
> **¿Por qué debe ser la primera?** > En sistemas BIOS/MBR, la partición raíz (o en su defecto, la partición donde residen los archivos de arranque) actúa como el volumen de inicio (*Boot*). Por lo tanto, debe estar ubicada al principio del disco para asegurar que la BIOS pueda leerla sin inconvenientes y evitar problemas de arranque. Aunque ya no es estrictamente necesario ponerla al principio con las BIOS modernas, sigue siendo una buena práctica.

En esta partición se instalan todos los **archivos del sistema**, las aplicaciones, las configuraciones globales del entorno y los controladores necesarios para el correcto funcionamiento del sistema operativo Linux.

### 📏 Tamaño recomendado para la Raíz (`/`)

El tamaño que le asignaremos depende del uso que le vayamos a dar al sistema operativo, pero lo podemos dividir en estas tres circunstancias:

*  **20 GB a 30 GB:** Es suficiente para distribuciones ligeras o si vas a instalar muy pocos programas.
*  **40 GB a 60 GB:** Este es el punto ideal para la mayoría de usuarios. Te permite usar el sistema con total tranquilidad en el día a día.
*  **80 GB a 100 GB:** Necesario para *gamers* o si planeas instalar entornos de desarrollo pesados (como *Android Studio*), herramientas de `Docker`, o si usas `Flatpak`/`Snap` con frecuencia (ya que estos formatos de paquetes empaquetan sus propias dependencias y ocupan mucho más espacio).
  
En esta partición generalmente usamos el sistema de archivos ext4.
### Swap

La memoria **Swap** (o espacio de intercambio) es una zona de tu unidad de almacenamiento (SSD o HDD) que el sistema operativo Linux utiliza como una extensión de la memoria RAM física.

Cuando la memoria RAM está llena, el ordenador busca procesos que no estés usando en ese momento para transferirlos a la memoria swap. Esta es mucho más lenta que la RAM, pero evita que el ordenador se bloquee o cierre programas a la fuerza. Su función principal aquí no es expandir tu RAM de forma masiva, sino actuar como una **red de seguridad** para que el sistema no se congele.

> [!NOTE]
> Esta es una partición **opcional**, ya que una vez instalado el sistema operativo podemos crear un **Swapfile**. El Swapfile es un archivo (no una partición) que funciona exactamente igual y es mucho más fácil de redimensionar en el futuro sin correr riesgos con los datos del disco.

---

#### 📊 ¿Cómo calcular el tamaño de la memoria Swap?

Para decidir el tamaño de nuestro espacio Swap, debemos tener en cuenta tres factores principales:
1. El tamaño de nuestra memoria **RAM** física.
2. Si nuestro disco duro es un **HDD** (mecánico) o un **SSD** (sólido).
3. Si vamos a utilizar la función de **hibernación**.

### 📊 Tamaño de la Swap (Adaptación de recomendaciones de Red Hat y Canonical)

Para decidir el tamaño de tu espacio Swap, busca tu cantidad de memoria RAM física y elige la opción según si vas a usar o no la **hibernación**:

- **Si tienes 2 GB de RAM o menos:**
  - **SIN hibernación:** Asigna el doble de tu RAM (ej. `4 GB`).
  - **CON hibernación:** Asigna el triple de tu RAM (ej. `6 GB`).

- **Si tienes entre 2 GB y 8 GB de RAM:**
  - **SIN hibernación:** Asigna el mismo tamaño que tu RAM (ej. si tienes 4 GB de RAM, pon `4 GB` de Swap).
  - **CON hibernación:** Asigna el doble de tu RAM.

- **Si tienes entre 8 GB y 64 GB de RAM:**
  - **SIN hibernación:** Asigna un tamaño fijo de **`4 GB`** (es el "colchón" de seguridad ideal).
  - **CON hibernación:** Asigna `1.5` veces el tamaño de tu RAM (ej. si tienes 16 GB de RAM, pon `24 GB` de Swap).

- **Si tienes más de 64 GB de RAM:**
  - **SIN hibernación:** No es necesaria, aunque se recomienda dejar **`4 GB`** por pura seguridad del sistema.
  - **CON hibernación:** Asigna `1.5` veces el tamaño de tu RAM.
### /home (Carpeta de Usuarios)

La partición `/home` es el espacio dedicado exclusivamente a los usuarios del sistema. Aquí es donde se crean tus carpetas personales de **Descargas, Documentos, Escritorio, Imágenes, Música y Vídeos**, además de los archivos de configuración de tus aplicaciones individuales (como los marcadores de tu navegador o los datos de tus juegos).

Separar `/home` en una partición propia no es obligatorio, pero es una de las prácticas más recomendadas en Linux.

---

#### 🌟 ¿Por qué es buena idea crear una partición `/home` separada?

* 🔄 **Formateos e instalaciones limpias sin perder nada:** Si en el futuro el sistema operativo se rompe o decides cambiar de distribución de Linux (por ejemplo, pasarte de Arch a Debian), puedes formatear la partición raíz (`/`) para reinstalar el sistema desde cero y dejar la partición `/home` intacta. Al iniciar el nuevo sistema, conservarás todos tus archivos personales y configuraciones exactamente como los dejaste.
* 🛡️ **Seguridad para el sistema:** Si te quedas sin espacio en el disco descargando archivos grandes, solo se llenará la partición `/home`. La partición raíz (`/`) seguirá teniendo espacio libre, evitando que el sistema operativo colapse o deje de arrancar.
* 💾 **Organización del almacenamiento:** Te permite, por ejemplo, instalar el sistema operativo en un disco rápido (como un SSD NVMe) y asignar la partición `/home` a un disco mecánico (HDD) de gran capacidad para almacenar tus archivos pesados.

En esta partición generalmente usamos el sistema de archivos ext4.
