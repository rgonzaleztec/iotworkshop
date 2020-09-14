# Repositorio para Taller de Industria 4.0 sobre IoT
En este repositorio se puede encontrar ejemplos de configuración para placas como Arduino Uno y Feather LoraWan.
Se utiliza la plataforma para IoT Ubidots, la mis ofrece una documentación muy completa para la basta cantidad de plataformas de dispositivos que pueden utilizar con esta plataforma.

Para revisar toda la documentación acceder a este [enlace](https://ubidots.com/docs/devices/#devices).

Para la plataforma de The Things Network acceder a este [enlace](https://www.thethingsnetwork.org/docs/)

La información que se presenta en este repositorio es estraida de las documentaciones de las plataformas y organizada de manera que sea posible reproducir y crear una aplicación de IoT con estas placas.

Las explicaciones se basan en el trabajo y experiencia que se ha obtenido desarrollando aplicaciones de este tipo.

Pueden realizar consulta escribiendo a Rogelio Gonzalez al correo rojo@tec.ac.cr


# Sobre las placas
Vamos a utilizar un Arduino Uno con un shield de ethernet.

![Arduino Uno](/imagenes/arduinoUno.jpg)

Las placas que cambiaron las posibilidades de poder desarrollar, probar y prototipar muchas aplicaciones que antes dependian de hardware costoso imposible para prototipar.

![Feather M0 RFM95](/imagenes/feather_3178_top_ORIG.jpg)

Basadas en el SX127x LoRa el cual define el radio de comunición estandarizado para redes LoraWAN. El mismo tiene 10 pines analogos de entrada y uno de salida.



## Primer paso aliste su hardware
Debe colocar el shield de arduino para ethernet como se muestra en la siguiente foto.

![Arduino con placa](/imagenes/shield-ethernet-para-arduino-uno-mega.jpg)

Recordar que existen muchas otras placas o shields que se pueden agregar al arduino para hacer conexiones, todo depende de la solución que se esta planteando.
---

No se requiere más equipo conectado solo requerimos un cable para conectar nuestro dispositivo al computador para configurarlo. Verifique que su placa ethernet pudo conectarse a la red proporcionando electricidad a la placa con el adaptador de pared para su Arduino o mediante el cable conectado al puerto del computador. Para verificar si la placa esta funcionando la misma tiene leds de luz que deberán estar parpadeando.

Si no tiene instalado o no ha trabajado con Arduino IDE debe seguir este [enlace](https://help.ubidots.com/en/articles/2033398-setting-up-the-arduino-ide-for-ubidots).

Si ya tiene instalado y configurado su Arduino IDE, debe instalar la libreria de Ethernet de Ubidots descarguela de [aquí](https://github.com/ubidots/ubidots-arduino-ethernet/archive/master.zip).

El archivo zip descargado se debe instalar dentro del IDE de Arduino en Sketch -> Include Library -> Add .ZIP Library. Luego reinicie el Arduino IDE para que se recargue la librería que acabos de subir.



# Sobre la Plataforma
Ubidots proporciona todo lo necesario para que usted pueda subir o bajar datos de los dispositivos, recuerte que estamos utilizando una cuenta STEM, por lo que solo podemos incluir tres dispositivos de manera perpetua. Si se desean más dispositivos o crear aplicaciones para producción lo recomendable es utilizar una cuenta industrial.

Ubidots nos da acceso a Tokens con los que podemos subir o bajar información, estos son los que aseguran que estamos accediendo a la cuenta correcta.
Cada dispositivo tiene su propio identificador, cada variable también por lo que es sencillo acceder para descargar información. Para subir información basta con tener el token y la url para Industria o STEM según el tipo de cuenta que se tenga.




# Como conectar un Arduino Uno

![Arduino con placa](/imagenes/shield-ethernet-para-arduino-uno-mega.jpg)

Desde luego que sin una tarjeta de red no se puede conectar a la Internet. No se requieren puertos especiales para acceder a la plataforma Ubidots, basta con tener acceso a Internet y navegar. Lo anterior es porque la plataforma tiene APIs seguros que permiten tanto subir como descargar datos de los sensores.

Se pueden utilizar muchos otros shields como el de WiFi o BLuetooth en el caso de este último se requiere que se conecte a algun dispositivo que le permita tener acceso a Internet o aplicación que recolecte y muestre la data como información. Lo anterior como punto final para dar información o generar una nueva acción.


Vamos a modificar el ejemplo que trae por defecto la libreria que acabamos de instalar. Para acceder al ejemplo vamos a abrir la aplicación Arduino IDE, en el menú de **Archivo--> Ejemplos --> UbidotsArduino library** seleccionamos el ejemplo de **UbidotsGetValue**.

Una vez cargado el ejemplo estamos casi listos para cargarlo a nuestro Arduino Uno con placa de ethernet. Este se puede compilar para estar seguros que las librerías requeridas están correctamente instaladas. Si la compilación es exitosa podemos subir el código a la placa para empezar a enviar valores.


Este es el código ejemplo con comentarios en español incluidos por mi persona, el código esta basado en el ejemplo proporcionado por Ubidots.


```c++
/********************************
 * Libraries included
 *******************************/
#include <Ethernet.h>
#include <SPI.h>
#include <UbidotsEthernet.h>

/********************************
 * Constants and objects
 *******************************/
/* Assigns the Ubidots parameters */
char const * TOKEN = "Assign_your_Ubidots_TOKEN_here"; // Assign your Ubidots TOKEN
char const * VARIABLE_LABEL_1 = "temperature"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_2 = "humidity"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_3 = "pressure"; // Assign the unique variable label to send the data

/* Enter a MAC address for your controller below */
/* Newer Ethernet shields have a MAC address printed on a sticker on the shield */
byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };

/* Set the static IP address to use if the DHCP fails to assign */
IPAddress ip(8, 8, 8, 8);

/* initialize the instance */
Ubidots client(TOKEN);

/********************************
 * Main Functions
 *******************************/
void setup() {
    Serial.begin(9600);
    client.setDebug(true);
    //client.setDeviceLabel("my-new-device");// uncomment this line to change the defaul label

    /* start the Ethernet connection */
    if (Ethernet.begin(mac) == 0) {
      Serial.println("Failed to configure Ethernet using DHCP");
      /* Try to configure using IP address instead of DHCP */
      Ethernet.begin(mac, ip);
    }
    /* Give the Ethernet shield a second to initialize */
    delay(1000);
}

void loop() {
  /* Sensors readings */
  float value_1 = analogRead(A0);
  float value_2 = analogRead(A1);
  float value_3 = analogRead(A2);
  /* Sending values to Ubidots */
  client.add(VARIABLE_LABEL_1, value_1);
  client.add(VARIABLE_LABEL_2, value_2);
  client.add(VARIABLE_LABEL_3, value_3);
  client.sendAll();
  delay(5000);
}
```
The library allow you send just **5 variables** per request. But don't worry for that, if you desire send more than 5 variables, go to **Sketch -> Examples -> ubidots-arduino-ethernet library** and select the **"UbidotsSaveMultiValues"** example. Don't forget assign your Ubidots [TOKEN](http://help.ubidots.com/user-guides/find-your-token-from-your-ubidots-account), and the variables labels where is indicated, also the MAC address for your controller.

```c++
/********************************
 * Libraries included
 *******************************/
#include <Ethernet.h>
#include <SPI.h>
#include <UbidotsEthernet.h>

/********************************
 * Constants and objects
 *******************************/
/* Assigns the Ubidots parameters */
char const * TOKEN = "Assign_your_Ubidots_TOKEN_here"; // Assign your Ubidots TOKEN
char const * VARIABLE_LABEL_1 = "temperature"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_2 = "humidity"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_3 = "pressure"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_4 = "speed"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_5 = "current"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_6 = "voltage"; // Assign the unique variable label to send the data
char const * VARIABLE_LABEL_7 = "light"; // Assign the unique variable label to send the data

/* Enter a MAC address for your controller below */
/* Newer Ethernet shields have a MAC address printed on a sticker on the shield */
byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };

/* Set the static IP address to use if the DHCP fails to assign */
IPAddress ip(8, 8, 8, 8);

/* initialize the instance */
Ubidots client(TOKEN);

/********************************
 * Main Functions
 *******************************/
void setup() {
    Serial.begin(9600);
    client.setDebug(true);
    //client.setDeviceLabel("my-new-device");// uncomment this line to change the defaul label

    /* start the Ethernet connection */
    if (Ethernet.begin(mac) == 0) {
      Serial.println("Failed to configure Ethernet using DHCP");
      /* Try to configure using IP address instead of DHCP */
      Ethernet.begin(mac, ip);
    }
    /* Give the Ethernet shield a second to initialize */
    delay(1000);
}

void loop() {
  /* Sensors readings */
  float value = analogRead(A0);
  /* Sending values to Ubidots */
  client.add(VARIABLE_LABEL_1, value);
  client.add(VARIABLE_LABEL_2, value);
  client.add(VARIABLE_LABEL_3, value);
  client.add(VARIABLE_LABEL_4, value);
  client.add(VARIABLE_LABEL_5, value);
  client.sendAll();
  /* Sending more than 5 variables */
  client.add(VARIABLE_LABEL_6, value);
  client.add(VARIABLE_LABEL_7, value);
  client.sendAll();
  delay(5000);
}
```


## Get the last value from Ubidots

To get the last value of a variable from Ubidots, go to **Sketch -> Examples -> UbidotsArduino library** and select the **"UbidotsGetValue"** example.

Assign your Ubidots [TOKEN](http://help.ubidots.com/user-guides/find-your-token-from-your-ubidots-account), the device and variable label from where you desire obtain the value, also the MAC address for your controller.

Upload the code, open the Serial monitor to check the results.

**NOTE**: If no response is seen, try unplugging your Arduino and then plugging it again. Make sure the baud rate of the Serial monitor is set to the same one specified in your code.

```c++
/********************************
 * Libraries included
 *******************************/
#include <Ethernet.h>
#include <SPI.h>
#include <UbidotsEthernet.h>

/********************************
 * Constants and objects
 *******************************/
/* Assigns the Ubidots parameters */
char const * TOKEN = "Assign_your_Ubidots_TOKEN_here"; // Assign your Ubidots TOKEN
char const * DEVICE_LABEL = "Assign_device_label_here"; // Assign the unique device label
char const * VARIABLE_LABEL = "Assign_variable_label_here"; // Assign the unique variable label to get the last value

/* Enter a MAC address for your controller below */
/* Newer Ethernet shields have a MAC address printed on a sticker on the shield */
byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };

/* Set the static IP address to use if the DHCP fails to assign */
IPAddress ip(8, 8, 8, 8);

/* initialize the instance */
Ubidots client(TOKEN);

/********************************
 * Main Functions
 *******************************/
void setup() {
    Serial.begin(9600);
    //client.setDebug(true);// uncomment this line to visualize the debug message
    /* start the Ethernet connection */
    if (Ethernet.begin(mac) == 0) {
      Serial.println("Failed to configure Ethernet using DHCP");
      /* Try to configure using IP address instead of DHCP */
      Ethernet.begin(mac, ip);
    }
    /* Give the Ethernet shield a second to initialize */
    delay(1000);
}

void loop() {
    /* Getting the last value from a variable */
    float value = client.getValue(DEVICE_LABEL, VARIABLE_LABEL);
    /* Print the value obtained */
    Serial.print("the value received is:  ");
    Serial.println(value);
    delay(5000);
}
```


# Como conectar un feather Lora


