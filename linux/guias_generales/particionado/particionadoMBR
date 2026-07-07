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
