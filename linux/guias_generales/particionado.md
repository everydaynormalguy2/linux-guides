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



