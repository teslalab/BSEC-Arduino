# Instrucciones para el uso de la librería BSEC en Arduino IDE

## Acerca de BSEC 
Software Bosch Sensortec Enviromental Cluster (BSEC) V1.4.8.0 lanzado el 8 de julio del año 2020

La librería de fusión BSEC se ha conceptualizado para proporcionar un procesamiento e integración de señales de alto nivel para el sensor BME680. La librería recibe valores del sensor compensados del API del sensor. Procesa las señales del BME680 para proporcionar las mediciones solicitadas.	

### Características 

- Cálculo preciso de la temperatura del aire en el ambiente.
- Cálculo preciso de la humedad relativa en el ambiente.
- Cálculo preciso de la presión en el ambiente.
- Cálculo preciso del nivel de calidad de aire IAQ (Índice de calidad del aire).

### Aplicaciones típicas

- Control de salud / bienestar (Advertencia de deshidratación / Insolación).
- Control de domótica.
- Control de aplicaciones de calefacción, ventilación y aire acondicionado (HVAC).
- Aplicaciones de juguete como drones.
- Aplicaciones de Internet de las Cosas.
- Mejora de la navegación GPS.
- Navegación interior(Detección de piso, detección de ascensor), navegación al aire libre.
- Aplicaciones deportivas y de ocio.
- Pronóstico del tiempo.
- Aplicaciones sanitarias.
- Indicación de velocidad vertical (Por ejemplo velocidad de subida / bajada).

### Plataformas compatibles

La librería BSEC es compatible con MCU de 8, 16 y 32 bits.

### Binarios disponibles para descargar: 

| Platform | Compiler | ROM (BSEC) | ROM (BSEC lite*) | RAM  | TYPE |
|----------|----------|------------|------------------|------|------|
| Cortex-ARM | ARMCC | 19-20k | 12-13k | 1k | Cortex-M0, M0+, M3, M4, M4_FPU, M7 |
| Cortex-ARM | GCC | 20-22k | 12-14k | 1k | Cortex-M0, M0+, M3, M4, M4_FPU, M7 |
| Cortex-ARM | IAR | 20k | 12-13k | 1k | Cortex-M0, M0+, M3, M4, M4_FPU, M7 |
| Cortex-A* | GCC | 21k | 13k | 1k | Cortex-A7 |
| AVR_8bit | AVR-GCC | 42k | 25 | 1k | MegaAVR, XMEGA |
| AVR_32bit | AVR-GCC | 24k | 13k | 1k | 32-bit AVR UC3 |
| ESP8266 | xtensa-lx106-elf-gcc | 28k | 17k | 1k | ESP8266 |
| ESP32 | xtensa-esp32-elf-gcc | 24k | 14k | 1k | ESP32 |
| MSP430 | msp430-elf-gcc | 34k | 20k | 1k | MSP430 |
| Android system-x86 | gcc | 39-49k | 22-26k | 1k | x86, x86_64 |
| Android system-arm | gcc | 21-38k | 13-19k | 1k | arm, arm64 |
| Raspberry PI 0 linux | arm-linux-gnueabihf-gcc | 71k | 56k | 1k | armv6-32bits |
| Raspberry PI3 linux | arm-linux-gnueabihf-gcc | 72k | 57k | 1k | armv8-a-64bits |

La información anterior sobre el tamaño de la librería no incluye dependencias adicionales basadas en el proyecto y la plataforma integrada.

# Instalación 

## 1. Instale el IDE de Arduino más reciente

A partir de esta publicación, la última versión de Arduino IDE 1.8.13 se puede descargar desde este [link](https://www.arduino.cc/download_handler.php)

## 2. Instale la librería BSEC

Descargue la librería como zip e impórtela en el IDE de Arduino. Consulte [esta](https://www.arduino.cc/en/Guide/Libraries) guía sobre como importar librerías.

## 3. Modifique el archivo platform.txt

Si ya utilizado el código de ejemplo anterior y la guía de hackeo, elimine la marca `-libalgobsec` en el archivo platform.txt y haga referencia al archivo `complier.c.elf.extra_flags`.

El arduino-builder estándar ahora pasa las banderas del enlazador debajo `compiler.libraries.ldflags`. La mayoría de los archivos `platform.txt` no incluyen esta nueva variable opcional. Por lo tanto, deberá declarar el valor predeterminado de esta variable y agregarlo al final del tutorial. Se recomienda declararlo en la siguiente sección como se ve a continuación.

```
# These can be overridden in platform.local.txt
compiler.c.extra_flags=
compiler.c.elf.extra_flags=
#compiler.c.elf.extra_flags=-v
compiler.cpp.extra_flags=
compiler.S.extra_flags=
compiler.ar.extra_flags=
compiler.elf2hex.extra_flags=
compiler.libraries.ldflags=
```
y agréguelo como en los siguientes ejemplos 

### ESP8266 núcleo del foro de la comunidad ESP8266
Línea original 

```
## Combine gc-sections, archives, and objects
recipe.c.combine.pattern="{compiler.path}{compiler.c.elf.cmd}" {build.exception_flags} -Wl,-Map "-Wl,{build.path}/{build.project_name}.map" {compiler.c.elf.flags} {compiler.c.elf.extra_flags} -o "{build.path}/{build.project_name}.elf" -Wl,--start-group {object_files} "{archive_file_path}" {compiler.c.elf.libs} -Wl,--end-group  "-L{build.path}"
```
Debe reemplazar por la siguiente línea

```
## Combine gc-sections, archives, and objects
recipe.c.combine.pattern="{compiler.path}{compiler.c.elf.cmd}" {build.exception_flags} -Wl,-Map "-Wl,{build.path}/{build.project_name}.map" {compiler.c.elf.flags} {compiler.c.elf.extra_flags} -o "{build.path}/{build.project_name}.elf" -Wl,--start-group {object_files} "{archive_file_path}" {compiler.c.elf.libs} {compiler.libraries.ldflags} -Wl,--end-group  "-L{build.path}"
```

## 4. Verifique y cargue el código de ejemplo
Inicie o reinicie el IDE de Arduino, abra el código de ejemplo que se encuentra en `File>Examples>Bsec software library>Basic`
Seleccione su tarjeta y el puerto COM correcto, suba el ejemplo y abra el monitor serial, debería ver texto en la terminal.

Tenga en cuenta que no todos los núcleos son compatibles. En estos casos, los ejemplos se pueden encontrar en `File>Examples>INCOMPATIBLE>Bsec software library>Basic`

