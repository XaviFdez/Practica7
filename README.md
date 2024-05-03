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
#include "Audio.h"
#include "SD.h"
#include "FS.h"

// Digital I/O used
#define SD_CS          5
#define SPI_MOSI      23
#define SPI_MISO      19
#define SPI_SCK       18
#define I2S_DOUT      25
#define I2S_BCLK      27
#define I2S_LRC       26

Audio audio;

void setup(){
    pinMode(SD_CS, OUTPUT);
    digitalWrite(SD_CS, HIGH);
    SPI.begin(SPI_SCK, SPI_MISO, SPI_MOSI);
    Serial.begin(115200);
    SD.begin(SD_CS);
    audio.setPinout(I2S_BCLK, I2S_LRC, I2S_DOUT);
    audio.setVolume(10); // 0...21
    audio.connecttoFS(SD, "/SD card/Saiko.wav");
}

void loop(){
    audio.loop();
}

// optional
void audio_info(const char *info){
    Serial.print("info        "); Serial.println(info);
}
void audio_id3data(const char *info){  //id3 metadata
    Serial.print("id3data     ");Serial.println(info);
}
void audio_eof_mp3(const char *info){  //end of file
    Serial.print("eof_mp3     ");Serial.println(info);
}
void audio_showstation(const char *info){
    Serial.print("station     ");Serial.println(info);
}
void audio_showstreaminfo(const char *info){
    Serial.print("streaminfo  ");Serial.println(info);
}
void audio_showstreamtitle(const char *info){
    Serial.print("streamtitle ");Serial.println(info);
}
void audio_bitrate(const char *info){
    Serial.print("bitrate     ");Serial.println(info);
}
void audio_commercial(const char *info){  //duration in sec
    Serial.print("commercial  ");Serial.println(info);
}
void audio_icyurl(const char *info){  //homepage
    Serial.print("icyurl      ");Serial.println(info);
}
void audio_lasthost(const char *info){  //stream URL played
    Serial.print("lasthost    ");Serial.println(info);
}
void audio_eof_speech(const char *info){
    Serial.print("eof_speech  ");Serial.println(info);
}
```

### Funcionamiento y salida por puerto serie 

En este código se configura un sistema para reproducir audio desde una tarjeta SD en un dispositivo Arduino. 
Define los pines utilizados, inicializa la comunicación con la tarjeta SD y la salida de audio, y proporciona funciones para manejar eventos durante la reproducción del audio, como por ejemplo el final del archivo.


#### Salidas

Las salidas dependerán de los subprogramas de la segunda parte del código (audio_info, audio_bitrate,audio_icyurl, audio_eof_speech) , entre los otros que se encuentran 

Especificando en cada subprograma nos darán la información siguiente lo cual depende lo que ocurra saldrá su serial Print por el puerto serie.

 - *audio_info:* Imprime información general sobre el estado de reproducción del audio.

 - audio_id3data: Imprime metadatos ID3 del archivo de audio.

 - audio_eof_mp3: Indica el final del archivo de audio MP3.

 - audio_showstation: Muestra información sobre la estación de radio que se está reproduciendo.

 - audio_showstreaminfo: Muestra información sobre el flujo de audio.

 - audio_showstreamtitle: Muestra el título del flujo de audio.

 - audio_bitrate: Muestra la tasa de bits del audio.

 - audio_commercial: Muestra la duración de los anuncios comerciales.

 - audio_icyurl: Muestra la URL de la página principal del flujo de audio.

 - audio_lasthost: Muestra la última URL del flujo de audio reproducida.

 - audio_eof_speech: Indica el final de la reproducción de un archivo de audio de tipo discurso.

```
info
id3data
eof_mp3
station
streaminfo
.
.
.
```

### Fotografías de la práctica: 