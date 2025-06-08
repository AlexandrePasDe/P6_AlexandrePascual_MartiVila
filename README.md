# P6_AlexandrePascual_MartiVila
Participants: Alexandre Pascual i Marti Vila
# **Busos de Comunicació**


## **1. Introducció**

A la anterior pràctica vam  treballar amb els busos **I2C**, en aquesta treballarem amb el bus **SPI**.
Al primer apartat implementarem la lectura i escriptura d'una memoria SD i a la segona llegirem dades desde una etiqueta 'RFID' amb el protocol **SPI**.


### **Lectura i escriptura en memoria SD**

### *Codi de lectura d'una memòria SD*

```c++
#include <SPI.h>
#include <SD.h>
#include <Arduino.h>
File myFile;
void setup()
{
 Serial.begin(9600);
 Serial.print("Iniciando SD ...");
 if (!SD.begin(10)) {
 Serial.println("No se pudo inicializar");
 return;
 }
 Serial.println("inicializacion exitosa");
 myFile = SD.open("/archivo.txt");//abrimos el archivo
 if (myFile) {
 Serial.println("archivo.txt:");
 while (myFile.available()) {
 Serial.write(myFile.read());
 }
 myFile.close(); //cerramos el archivo
 } else {
 Serial.println("Error al abrir el archivo");
 }
}
void loop()
{

}
```

### **Explicació**

1. El programa inicialitza una comunicació amb una tarjeta SD.
2. Obre l'arxiu de text `archivo.txt`.
3. Llegieix el contingut.
4. Mostra el contingut per el monitor.
5. Tanca l'arxiu.

### **Sortida:**
```
Iniciando SD ... inicializacion exitosa
archivo.txt:
Soy el mejor del mundo
```

![SD](https://github.com/user-attachments/assets/895412b4-20b4-4887-8a45-959b0d6397d1)


### **Lectura d'una etiqueta RFID**

## *Codi*

```c++
#include <SPI.h>
#include <MFRC522.h>
#define RST_PIN 0 //Pin 9 para el reset del RC522
#define SS_PIN 10 //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522
void setup() {
Serial.begin(9600); //Iniciamos la comunicación serial
SPI.begin(); //Iniciamos el Bus SPI
mfrc522.PCD_Init(); // Iniciamos el MFRC522
Serial.println("Lectura del UID");
}
void loop() {
// Revisamos si hay nuevas tarjetas presentes
if ( mfrc522.PICC_IsNewCardPresent())
 {
 //Seleccionamos una tarjeta
 if ( mfrc522.PICC_ReadCardSerial())
 {
 // Enviamos serialemente su UID
 Serial.print("Card UID:");
 for (byte i = 0; i < mfrc522.uid.size; i++) {
  Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0"
    : " ");
     Serial.print(mfrc522.uid.uidByte[i], HEX);
     }
     Serial.println();
     // Terminamos la lectura de la tarjeta actual
     mfrc522.PICC_HaltA();
     }
    }
    }
```


### **Explicació**

1. Inicialitza una comunicació SPI i configura el lector RFID MFRC522
2. Va revisant si apareixen noves tarjetes.
3. Llegeix l'UID d'una tarja que s'ha apropat al lector.
4. Ho mostra al monitor.


### **Sortida:**
De la tarjeta:
```
Lectura del UID
Card UID: 05 E7 F7 04
```
![SPICard](https://github.com/user-attachments/assets/a76bf756-7d58-453a-8a52-5084ffe1cfc3)
