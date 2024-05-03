# Practica 7: Buses de comunicación (III) - I2S

En esta séptima práctica trabajaremos con el bus de comunicación en serie I2S donde con la ayuda de un altavoz realizaremos las 2 partes que consta la práctica.

## 1- Reproducción desde memoria interna

```c++
#include"AudioGeneratorAAC.h"
#include"AudioOutputI2S.h"
#include"AudioFileSourcePROGMEM.h"
#include"sampleaac.h"

AudioFileSourcePROGMEM*in;
AudioGeneratorAAC*aac;
AudioOutputI2S*out;

voidsetup(){
Serial.begin(115200);
in=newAudioFileSourcePROGMEM(sampleaac,sizeof(sampleaac));
aac=newAudioGeneratorAAC();
out=newAudioOutputI2S();
out->SetGain(0.125);
out->SetPinout(26,25,22);
aac->begin(in,out);
}

///

```

### Funcionamiento y salida por puerto serie 


#### Diagrama sobre el funcionamiento


#### Salida

```

```

## 2- Reproducir un archivo WAVE en ESP32 desde una tarjeta SD externa

```c++

```

### Funcionamiento y salida por puerto serie 


#### Específicaciones

#### Salida