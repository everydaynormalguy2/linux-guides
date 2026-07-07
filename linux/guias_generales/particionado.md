Aquí vamos a hablar sobre cómo hacer las particiones, cada herramienta de particionado tiene su propia guía.


# Guía de Particionado de Discos

El particionado es el proceso de dividir tu unidad de almacenamiento (SSD o HDD) en secciones lógicas independientes. Esto es fundamental, ya que nos permite definir exactamente dónde se guardará el sistema operativo, los archivos de arranque y tus datos personales.

Dividimos esta guía en dos secciones según la Bios/Firmware de la placa base, es decir, si la computadora arranca usando el sistema antiguo BIOS/MBR o el sistema moderno UEFI/GPT.

Si no sabes qué sistema usa tu ordenador, este comando funciona en todas las distribuciones de Linux:

```bash
[ -d /sys/firmware/efi ] && echo "UEFI" || echo "BIOS"

En esta sección encontrarás las guías detalladas para realizar el proceso. Puedes elegir la herramienta que mejor se adapte a tu nivel de experiencia:

* 🖥️ **[Guía de cfdisk (Recomendado)](./cfdisk.md)**: Una interfaz basada en texto (TUI) con menús interactivos. Es la opción ideal si buscas un proceso visual, intuitivo y con menor riesgo de equivocarte.
* ⌨️ **[Guía de fdisk (Avanzado)](./fdisk.md)**: La herramienta clásica de Linux que se maneja puramente a través de comandos en la terminal. Ideal si prefieres un control absoluto y directo.

