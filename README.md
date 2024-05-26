# Practica-6A
## Part A: LECTURA/ESCRIPTURA DE MEMÒRIA SD
### Codi Complet
```cpp
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>

File myFile;
#define SS 40

void setup() {
  Serial.begin(9600);
  SPI.begin(37,38,36);
  Serial.print("Iniciando SD ...");
  //begin(SS, SPI, 4000000, "/sd", 5, true);
  if (!SD.begin(SS, SPI, 4000000, "/sd", 5, true)) {
    Serial.println("No se pudo inicializar la tarjeta SD.");
    return;;
  }
  
  
  Serial.println("Inicialización exitosa.");

  // Crear y escribir en el archivo en la tarjeta SD
  myFile = SD.open("/archivo_copiado.txt", FILE_WRITE);
  if (myFile) {
    Serial.println("Archivo creado correctamente.");

    // Escribir datos en el archivo
    myFile.println("¡Hola, mundo!");
    myFile.println("Este es un archivo de texto creado desde Arduino.");

    // Cerrar el archivo
    myFile.close();
    Serial.println("Datos escritos correctamente en el archivo.");
  } else {
    Serial.println("Error al crear el archivo en la tarjeta SD.");
  }

  // Abrir y leer el archivo original desde la tarjeta SD
  myFile = SD.open("/archivo_original.txt");//abrimos el archivo
  if (myFile) {
    Serial.println("/archivo_original.txt");
    while (myFile.available()) {
      Serial.write(myFile.read());
    }
    myFile.close(); //cerramos el archivo
  } else {
    Serial.println("Error al abrir el archivo.");
  }
}

void loop() {
 
}
```
### Funcionament per parts del codi
__1. Inclusió de Biblioteques__
```cpp
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
```
- Arduino.h: Biblioteca estàndart d'Arduino
- SPI.h:
- SD.h:
  
__2. Declaracions i Definicions__
```cpp
File myFile;
#define SS 40
```
Es declara una variable tipus File per gestionar arxius a la tarjeta SD.
Es defineix el pin 40 com el pin SS (Slave Select) per a la comunicació SPI amb la targeta SD.

__3. Setup__
```cpp
void setup() {
  Serial.begin(9600);
  SPI.begin(37,38,36);
  Serial.print("Iniciando SD ...");
  //begin(SS, SPI, 4000000, "/sd", 5, true);
  if (!SD.begin(SS, SPI, 4000000, "/sd", 5, true)) {
    Serial.println("No se pudo inicializar la tarjeta SD.");
    return;;
  }
 Serial.println("Inicialización exitosa.");
```
- Serial.begin(9600);: Inicialitza la comunicació sèrie a 9600 bauds per a la comunicació amb l'ordinador.
- SPI.begin(37, 38, 36);: Inicialitza la interfície SPI amb els pins 37 (MISO), 38 (MOSI) i 36 (SCK).
- Serial.print("Iniciant SD ...");: Imprimeix un missatge al monitor sèrie indicant que s'està iniciant la targeta SD.
- if (!SD.begin(SS, SPI, 4000000, "/sd", 5, true)): Intenta inicialitzar la targeta SD. Els paràmetres de SD.begin inclouen el pin SS, la interfície SPI, la velocitat de rellotge (4 MHz), el nom del dispositiu ("/sd"), el nombre d'intents d'inicialització (5) i un booleà que indica si s'han d'utilitzar els pins de maquinari SPI (true). Si la inicialització falla, s'imprimeix un missatge d'error i es surt de la funció setup().
- Serial.println("Inicialització exitosa.");: Si la inicialització de la targeta SD és exitosa, s'imprimeix un missatge al monitor sèrie.

  
__4. Crear i Escriure en un Arxiu__
```cpp
 // Crear y escribir en el archivo en la tarjeta SD
  myFile = SD.open("/archivo_copiado.txt", FILE_WRITE);
  if (myFile) {
    Serial.println("Archivo creado correctamente.");

    // Escribir datos en el archivo
    myFile.println("¡Hola, mundo!");
    myFile.println("Este es un archivo de texto creado desde Arduino.");

    // Cerrar el archivo
    myFile.close();
    Serial.println("Datos escritos correctamente en el archivo.");
  } else {
    Serial.println("Error al crear el archivo en la tarjeta SD.");
  }

```
- myFile = SD.open("/archivo_copiado.txt", FILE_WRITE);: Obre (o crea si no existeix) un arxiu anomenat "archivo_copiado.txt" a la targeta SD en mode d'escriptura.
- if (myFile): Verifica si l'arxiu s'ha obert correctament.
- myFile.println("¡Hola, mundo!");: Escriu "¡Hola, mundo!" a l'arxiu.
- myFile.println("Este es un archivo de texto creado desde Arduino.");: Escriu una altra línia de text a l'arxiu.
- myFile.close();: Tanca l'arxiu per assegurar que les dades es guardin correctament.
- Serial.println("Dades escrites correctament a l'arxiu.");: Imprimeix un missatge al monitor sèrie indicant que les dades s'han escrit correctament.

  
__5. Obrir i Llegir un Arxiu Existent__
```cpp
  // Abrir y leer el archivo original desde la tarjeta SD
  myFile = SD.open("/archivo_original.txt");//abrimos el archivo
  if (myFile) {
    Serial.println("/archivo_original.txt");
    while (myFile.available()) {
      Serial.write(myFile.read());
    }
    myFile.close(); //cerramos el archivo
  } else {
    Serial.println("Error al abrir el archivo.");
  }
}
```
- myFile = SD.open("/archivo_original.txt");: Obre un arxiu anomenat "archivo_original.txt" a la targeta SD en mode de lectura.
- if (myFile): Verifica si l'arxiu s'ha obert correctament.
- while (myFile.available()) { Serial.write(myFile.read()); }: Mentre hi hagi dades disponibles a l'arxiu, llegeix i escriu aquestes dades al monitor sèrie.
- myFile.close();: Tanca l'arxiu.
- Serial.println("Error en obrir l'arxiu.");: Si l'arxiu no es pot obrir, imprimeix un missatge d'error al monitor sèrie.


Resum del funcionament: 

Inicialitza la comunicació SPI i la targeta SD.

Crea i escriu en un arxiu anomenat "archivo_copiado.txt".

Obre i llegeix un arxiu anomenat "archivo_original.txt", imprimint el seu contingut al monitor sèrie.
