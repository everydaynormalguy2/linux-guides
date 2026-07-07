En esta guía nos centraremos solo en sistemas de 64 bits ya que Arch no ofrece una versión oficial para versiones de 32 bits.

# Paso 1: El Menú de Arranque (Boot Menu)

Al encender la computadora con el USB de instalación de Arch Linux conectado, lo primero que verás en pantalla es este menú. Desde aquí seleccionaremos cómo queremos que arranque el instalador.

<img width="632" height="475" alt="Screenshot_20260706_135702" src="https://github.com/user-attachments/assets/9b5fd664-c7cf-4e2f-b485-127ddbf35ad2" />


### Desglose de opciones del menú

* **`Arch Linux install medium (x86_64, BIOS)`**
  Esta es la opción principal y la que debes elegir en la mayoría de los casos. Carga el entorno de instalación estándar para procesadores de 64 bits.
* **`Arch Linux install medium (...) with speech`**
  Es exactamente igual a la anterior, pero activa un lector de voz integrado en la terminal para personas con discapacidad visual.
* **`Boot existing OS`**
  Cancela la instalación y arranca el sistema operativo que ya tengas instalado en el disco duro (como Windows u otra distribución de Linux).
* **`Run Memtest86+ (RAM test)`**
  Una herramienta muy útil para verificar si tus tarjetas de memoria RAM tienen fallos de hardware antes de proceder con la instalación.
* **`Hardware Information (HDT)`**
  Muestra una lista detallada de los componentes internos de tu computadora (procesador, tarjeta gráfica, etc.).
* **`Reboot` / `Power Off`**
  Reinicia o apaga el equipo de forma segura.

---

> [!NOTE]
> **¿Qué debes hacer aquí?**
> Simplemente asegúrate de que la primera opción (**Arch Linux install medium**) esté resaltada (puedes moverte con las flechas de dirección del teclado `↑` `↓`) y presiona la tecla **Enter**.


# Paso 2: Configurar el idioma del teclado

Una vez que presionas **Enter** en el menú de arranque, el sistema cargará una gran cantidad de texto en pantalla hasta dejarte frente a una terminal de comandos en negro esperando tus órdenes. 

Lo primero que debes hacer antes de escribir cualquier otra cosa es asegurarte de que tu teclado responda correctamente a los caracteres especiales (como los guiones, barras o la letra Ñ). Para ello, utilizaremos el comando `loadkeys`.

```bash
loadkeys es
```

# Paso 3: Preparar el particionado del disco

El siguiente paso crucial es preparar el disco duro donde vamos a instalar el sistema. Para ello, necesitamos crear las divisiones (particiones) necesarias para que Arch Linux pueda organizarse.

Para hacer esto, existen dos herramientas principales incluidas en el instalador:

* **`fdisk`**: La herramienta clásica que funciona puramente a base de comandos y argumentos en la terminal.
* **`cfdisk`**: Una alternativa que nos abre un entorno "semi-gráfico" (basado en menús de texto dentro de la terminal), lo cual hace que el proceso sea mucho más visual, intuitivo y seguro para no equivocarse.

---

> [!NOTE]
> Para no hacer esta sección demasiado larga, **las guías detalladas paso a paso sobre cómo usar `cfdisk` o `fdisk` las encontrarás en una sección independiente de esta guía**. De momento, solo ten en cuenta que aquí es donde prepararíamos nuestro disco.
