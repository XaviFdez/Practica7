# Practica 7: Buses de comunicación (III) - I2S

En esta séptima práctica trabajaremos con el bus de comunicación en serie I2S donde con la ayuda de un altavoz realizaremos las 2 partes que consta la práctica.

## 1- Reproducción desde memoria interna

```c++
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"

AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;

void setup()
{
    Serial.begin(115200);
    in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
    aac = new AudioGeneratorAAC();
    out = new AudioOutputI2S();
    out -> SetGain(0.125);
    out -> SetPinout(26,25,22);
    aac->begin(in, out);
}
void loop()
{
    if (aac->isRunning()) 
    {
        aac->loop();
    } 
    else 
    {
        aac -> stop();
        Serial.printf("Sound Generator\n");
        delay(1000);
    }
}
```

### Funcionamiento y salida por puerto serie 

Este código reproduce un archivo de audio AAC en un dispositivo Arduino utilizando un generador de audio AAC y una salida de audio I2S. 
El archivo de audio está almacenado en la memoria de programa del Arduino. El código verifica si el generador de audio está en ejecución y reproduce el archivo continuamente.

Cuando la reproducción termina, se detiene el generador de audio y se muestra un mensaje antes de volver a empezar.

#### Diagrama sobre el funcionamiento

```mermaid
graph TD;
    Inicio --> |Inicialización Serial| Inicialización_AudioFileSourcePROGMEM;
    Inicialización_AudioFileSourcePROGMEM --> |Inicialización AudioGeneratorAAC| Inicialización_AudioGeneratorAAC;
    Inicialización_AudioGeneratorAAC --> |Inicialización AudioOutputI2S| Inicialización_AudioOutputI2S;
    Inicialización_AudioOutputI2S --> |Configuración del ganancia de salida| Configuración_ganancia_salida;
    Configuración_ganancia_salida --> |Configuración de los pines de salida| Configuración_pines_salida;
    Configuración_pines_salida --> |Inicio de la reproducción del archivo AAC| Inicio_reproducción_archivo_AAC;
    Inicio_reproducción_archivo_AAC --> |¿El archivo está en reproducción?| ¿Archivo_en_reproducción?;
    ¿Archivo_en_reproducción? -- No --> |Detener la reproducción| Detener_reproducción;
    Detener_reproducción --> |Mostrar mensaje "Sound Generator"| Mostrar_mensaje;
    Mostrar_mensaje --> |Esperar 1 segundo| Esperar;
    Esperar --> Inicio;
    ¿Archivo_en_reproducción? -- Sí --> |Continuar reproducción del archivo AAC| Continuar_reproducción;
    Continuar_reproducción --> Fin;
```

#### Salida

Una vez se ejecuta el código la salida por puerto serie es: 

```
Sound Generator
Sound Generator

```

## 2- Reproducir un archivo WAVE en ESP32 desde una tarjeta SD externa

```c++

```

### Funcionamiento y salida por puerto serie 


#### Específicaciones

#### Salida

### Fotografías de la práctica: 