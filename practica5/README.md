## PRACTICA 5

En esta practica veremos como utilizar el bus de comunicacion I2C

El bus I2C se utiliza principalmente para la comunicación entre circuitos integrados de baja velocidad y corta distancia.

El bus I2C utiliza dos líneas principales para la comunicación:

Línea de reloj (SCL): Esta línea es controlada por el dispositivo maestro y genera los pulsos de reloj que sincronizan la comunicación.
Línea de datos (SDA): Esta línea lleva los datos que se transmiten entre los dispositivos conectados.
Además de estas dos líneas, también puede haber una línea de alimentación (Vcc) y una línea de tierra (GND) para proporcionar la energía y la referencia de voltaje necesarios.

El bus I2C permite la comunicación bidireccional, lo que significa que los dispositivos conectados pueden enviar y recibir datos. También admite la conexión de múltiples dispositivos a la misma línea de bus mediante direcciones únicas asignadas a cada dispositivo.

Primero comenzaremos con un programa para detectar que dispositivos tenemos conectados a traves del bus I2C 

**Codigo**

```cpp
#include <Arduino.h>
#include <Wire.h>
void setup()
{
Wire.begin();
Serial.begin(115200);
while (!Serial); // Leonardo: wait for serial monitor
Serial.println("\nI2C Scanner");
}
void loop()
{
byte error, address;
int nDevices;
Serial.println("Scanning...");
nDevices = 0;
for(address = 1; address < 127; address++ )
{
// The i2c_scanner uses the return value of
// the Write.endTransmisstion to see if
// a device did acknowledge to the address.
Wire.beginTransmission(address);
error = Wire.endTransmission();
if (error == 0)
{
Serial.print("I2C device found at address 0x");
if (address<16)
Serial.print("0");
Serial.print(address,HEX);
Serial.println(" !");
nDevices++;
}
else if (error==4)
{
Serial.print("Unknown error at address 0x");
if (address<16)
Serial.print("0");
Serial.println(address,HEX);
}
}
if (nDevices == 0)
Serial.println("No I2C devices found\n");
else
Serial.println("done\n");
delay(5000); // wait 5 seconds for next scan
}
```

**Librerias**
```cpp
#include <Arduino.h>
#include <Wire.h>
```
En este caso utilizaremos la libreria arduino.h para las funciones basicas y la libreria wire.h para poder establecer la conexion  a traves del bus i2c

**SETUP**

```cpp
void setup()
{
Wire.begin();
Serial.begin(115200);
while (!Serial); // Leonardo: wait for serial monitor
Serial.println("\nI2C Scanner");
}
```

En el void setup como de costumbre empezaremos inicializando el serial y ademas iniciamos el bus con la linea de codigo Wire.begin() tambien mostraremos por el serial un texto que dice I2C scanner.

**LOOP**
```cpp
void loop()
{
byte error, address;
int nDevices;
Serial.println("Scanning...");
nDevices = 0;
for(address = 1; address < 127; address++ )
{
// The i2c_scanner uses the return value of
// the Write.endTransmisstion to see if
// a device did acknowledge to the address.
Wire.beginTransmission(address);
error = Wire.endTransmission();
if (error == 0)
{
Serial.print("I2C device found at address 0x");
if (address<16)
Serial.print("0");
Serial.print(address,HEX);
Serial.println(" !");
nDevices++;
}
else if (error==4)
{
Serial.print("Unknown error at address 0x");
if (address<16)
Serial.print("0");
Serial.println(address,HEX);
}
}
if (nDevices == 0)
Serial.println("No I2C devices found\n");
else
Serial.println("done\n");
delay(5000); // wait 5 seconds for next scan
}
```
La principal funcion del Loop sera mostrar por el serial que dispositivos hemos conectado al bus i2c con su correspondinte direccion donde esta alojado.
Si detecta una direccion pero no hay respuesta dira que hay un error desconocido en la direccion mostrada.
Sin embargo si no encuentra dispositivos mostrara un texto diciendo que no se encuentran dispositivos.