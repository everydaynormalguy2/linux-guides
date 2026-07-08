# 🛠️ Sistemas de Archivos en Linux

Un sistema de archivos es el método que utiliza un sistema operativo para organizar, almacenar y nombrar los datos dentro de una unidad de almacenamiento. En Linux, no solo guardan datos, sino que gestionan los **permisos de seguridad** del sistema (quién puede leer, escribir o ejecutar un archivo).

Los siguientes sistemas de archivos son los que utilizaremos para dar formato a nuestras particiones como Raíz (`/`) o `/home`:

### 1. `ext4` (Fourth Extended Filesystem)
Es el estándar absoluto y la opción por defecto en el 90% de las distribuciones (como Ubuntu, Debian o Mint).
* **¿Cómo funciona?** Utiliza un sistema llamado **Journaling** (un registro previo). Antes de escribir un archivo en el disco, lo anota en un "diario". Si la luz se va en mitad de una copia, el sistema lee el diario al reiniciar y repara el error al instante.
* **¿Cuándo usarlo?** Siempre. Es la opción más segura, madura y estable para la partición Raíz (`/`) y `/home`.

### 2. `Btrfs` (B-tree File System)
Es un sistema de archivos moderno que ya viene por defecto en distribuciones como Fedora u openSUSE.
* **¿Cómo funciona?** Su característica estrella son los **Snapshots** (instantáneas). Puedes hacer una "foto" de todo tu sistema en un milisegundo. Si instalas algo y el sistema se rompe, puedes volver a la "foto" anterior desde el menú de arranque como si nada hubiera pasado. Además, incluye compresión de datos transparente para ahorrar espacio.
* **¿Cuándo usarlo?** Si vas a usar una distro que lo traiga por defecto o si te gusta experimentar y quieres la seguridad de poder "viajar en el tiempo" si rompes algo configurando el sistema.

### 3. `XFS`
Es un sistema de archivos de alto rendimiento optimizado para servidores y grandes empresas (es el predeterminado en Red Hat Enterprise Linux).
* **¿Cómo funciona?** Está diseñado para el procesamiento en paralelo. Es increíblemente rápido manejando archivos gigantescos (de varios Terabytes) y grandes volúmenes de datos simultáneos.
* **¿Cuándo usarlo?** En servidores de bases de datos, almacenamiento masivo o si vas a montar un servidor de archivos en casa. Para un ordenador de escritorio normal, es mejor optar por `ext4` o `Btrfs`.

---

### 💡 El único "intruso" obligatorio: `FAT32`
También hay que hacer una mención obligatoria a **`FAT32`** (o `vfat`), ya que sirve exclusivamente para crear la **partición EFI (Boot)** en sistemas modernos con UEFI. Las placas base y los sistemas de arranque de la placa no saben leer sistemas de archivos nativos de Linux como `ext4`.
