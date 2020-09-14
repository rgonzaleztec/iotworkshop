# Repositorio para Taller de Industria 4.0 sobre IoT
En este repositorio se puede encontrar ejemplos de configuración para placas como Arduino Uno y Feather LoraWan.
Se utiliza la plataforma para IoT Ubidots, la misma ofrece una documentación muy completa para la basta cantidad de plataformas de dispositivos que pueden utilizarse con esta plataforma.

Para revisar toda la documentación acceder a este [enlace](https://ubidots.com/docs/devices/#devices).

Para la plataforma de The Things Network acceder a este [enlace](https://www.thethingsnetwork.org/docs/)

La información que se presenta en este repositorio es estraída de las documentaciones de las plataformas y organizada de manera que sea posible reproducir y crear una aplicación de IoT con estas placas.

Las explicaciones se basan en el trabajo y experiencia que se ha obtenido desarrollando aplicaciones de este tipo.

Pueden realizar consultas escribiendo a Rogelio Gonzalez al correo rojo@tec.ac.cr


# Sobre las placas
Vamos a utilizar un Arduino Uno con un shield de ethernet.

![Arduino Uno](/imagenes/arduinoUno.jpg)

Las placas que cambiaron las posibilidades de poder desarrollar, probar y prototipar muchas aplicaciones que antes dependían de hardware costoso imposible para prototipar.

![Feather M0 RFM95](/imagenes/feather_3178_top_ORIG.jpg)

Basadas en el SX127x LoRa el cual define el radio de comunición estandarizado para redes LoraWAN. El mismo tiene 10 pines análogos de entrada y uno de salida.



## Primer paso aliste su hardware y Software
En esta sección vamos a configurar nuestro ambiente de trabajo básico para desarrollar este laboratorio

### Hardware
Debe colocar el shield de arduino para ethernet como se muestra en la siguiente foto.

![Arduino con placa](/imagenes/shield-ethernet-para-arduino-uno-mega.jpg)

Recordar que existen muchas otras placas o shields que se pueden agregar al arduino para hacer conexiones, todo depende de la solución que se esta planteando.
---

No se requiere más equipo conectado solo requerimos un cable para conectar nuestro dispositivo al computador para configurarlo. Verifique que su placa ethernet pudo conectarse a la red proporcionando electricidad a la placa con el adaptador de pared para su Arduino o mediante el cable conectado al puerto del computador. Para verificar si la placa esta funcionando la misma tiene leds de luz que deberán estar parpadeando.

El sensor que usted seleccione o tenga a la mano, para este taller se simularan datos. Este parte del código quedará documentada para que recuerde cambiarla por los datos del sensor que utilizará. Cada sensor tendrá sus requerimientos de conexión y deberá cablearlo según recomendación del fabricante. Pulse click [aqui](https://proyectosconarduino.com/sensores/) para conocer un poco más sobre diferentes sensores disponibles para Arduino que se pueden acoplar al proyecto que usted esta desarrollando.

### Software
Si no tiene instalado o no ha trabajado con Arduino IDE debe seguir este [enlace](https://help.ubidots.com/en/articles/2033398-setting-up-the-arduino-ide-for-ubidots).

Si ya tiene instalado y configurado su Arduino IDE, debe instalar la libreria de Ethernet de Ubidots descarguela de [aquí](https://github.com/ubidots/ubidots-arduino-ethernet/archive/master.zip).

El archivo zip descargado se debe instalar dentro del IDE de Arduino en Sketch -> Include Library -> Add .ZIP Library. Luego reinicie el Arduino IDE para que se recargue la librería que acabos de subir. Puede guiarse en con la siguiente imagen, los resaltados muestran donde se dio click.
[Cargar Libreria](/imagenes/agregarlibreria.png)

**No olvide reiniciar el IDE para que puedar recargar la librería**

Esto es lo único que requerimos para trabajar nuestro proyecto.

# Sobre la Plataforma
Ubidots proporciona todo lo necesario para que usted pueda subir o bajar datos de los dispositivos, recuerte que estamos utilizando una cuenta STEM, por lo que solo podemos incluir tres dispositivos de manera perpetua. Si se desean más dispositivos o crear aplicaciones para producción lo recomendable es utilizar una cuenta industrial.

Si no ha completo el proceso de registro en Ubidots STEM realicelo en el siguiente [enlace](https://ubidots.com/stem/). El registro es ágil y gratuito, esta muy bien documentado cuenta con soporte para 200+ dispositivos de fuente abierta y tutoriales para guiarte. Tienes acceso a tableros en tiempo real con 30+ tipos de widgets. Por último puedes utilizar los triggers o disparadores para tomar acción cuando un sensor tiene un lectura que nos indica reacción.

Ubidots nos da acceso a Tokens con los que podemos subir o bajar información, estos son los que aseguran que estamos accediendo a la cuenta correcta. Ver [imagen](/imagenes/ubidotstokens.png)

Cada dispositivo tiene su propio identificador, cada variable también por lo que es sencillo acceder para descargar información. Para subir información basta con tener el token y la url para Industria o STEM según el tipo de cuenta que se tenga.

Ver [Identificador Dispositivo](/imagenes/dispositivo.png)

Ver [Identificador de Variable](/imagenes/variable.png)

**Manejo de Dispositivos**
Se pueden crear manualmente en la plataforma para administrar los identificadores y tener control sobre lo que estamos subiendo a nuestra cuenta. Tambien es posible que se creen los dispositivos e identificadores al subir por primera vez un dato a la plataforma.

**Credits --> La plataforma trabaja con este modelo para cobrar o rebajar para créditos para las acciones que así lo requieren, por lo que enviar un SMS por ejemplo tiene un costo. Revise la sección de credits para saber más de como recargarlo.


# Como conectar un Arduino Uno

![Arduino con placa](/imagenes/shield-ethernet-para-arduino-uno-mega.jpg)

Desde luego que sin una tarjeta de red no se puede conectar a la Internet. No se requieren puertos especiales para acceder a la plataforma Ubidots, basta con tener acceso a Internet y navegar. Lo anterior es porque la plataforma tiene APIs seguros que permiten tanto subir como descargar datos de los sensores.

Se pueden utilizar muchos otros shields como el de WiFi o BLuetooth en el caso de este último se requiere que se conecte a algun dispositivo que le permita tener acceso a Internet o aplicación que recolecte y muestre la data como información. Lo anterior como punto final para dar información o generar una nueva acción.


Vamos a modificar el ejemplo que trae por defecto la libreria que acabamos de instalar. Para acceder al ejemplo vamos a abrir la aplicación Arduino IDE, en el menú de **Archivo--> Ejemplos --> UbidotsArduino library** seleccionamos el ejemplo de **UbidotsGetValue**.

Una vez cargado el ejemplo estamos casi listos para cargarlo a nuestro Arduino Uno con placa de ethernet. Este se puede compilar para estar seguros que las librerías requeridas están correctamente instaladas. Si la compilación es exitosa podemos subir el código a la placa para empezar a enviar valores.


Este es el código ejemplo con comentarios en español incluidos por mi persona, el código esta basado en el ejemplo proporcionado por Ubidots.


```c++
/********************************
 * Librerias incluidas
 *******************************/
#include <Ethernet.h>
#include <SPI.h>
#include <UbidotsEthernet.h>

/********************************
 * Constantes y Objetos
 *******************************/
/* Asignar los parametros para Ubidots */
char const * TOKEN = "Assign_your_Ubidots_TOKEN_here"; // El token que se indico en la seccion de plataforma
char const * VARIABLE_LABEL_1 = "temperature"; // Esta etiqueta es unica y es para cada variable
char const * VARIABLE_LABEL_2 = "humidity"; // Esta etiqueta es unica y es para cada variable
char const * VARIABLE_LABEL_3 = "pressure"; // Esta etiqueta es unica y es para cada variable

/* Enter a MAC address para la tarjeta de red del Arduino */
/* Las placas nuevas de ethernet traen una etiqueta con  */
byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };

/* Se asigna una direccion IP estatica en caso que no se tuviera automaticamente */
IPAddress ip(8, 8, 8, 8);

/* Instanciamos Ubidots */
Ubidots client(TOKEN);

/********************************
 * Funciones Principales
 *******************************/
// La seccion de setup en un arduino solo se corre una vez al encenderse la placa
void setup() {
    Serial.begin(9600);
    client.setDebug(true);
    //client.setDeviceLabel("my-new-device");// Se debe indicar el id de mi dispositivo

    /* start the Ethernet connection */
    if (Ethernet.begin(mac) == 0) {
      Serial.println("Failed to configure Ethernet using DHCP");
      /* Try to configure using IP address instead of DHCP */
      Ethernet.begin(mac, ip);
    }
    /* Give the Ethernet shield a second to initialize */
    delay(1000);
}

// Se ejecuta infinitamente hasta que la placa se quede sin electricidad o falle
void loop() {
  /* Lectura de los sensores */
  float value_1 = analogRead(A0);  // Estos son pines de la placa Arduino, se puede utilizar cualquiera de los analogos
  float value_2 = analogRead(A1);  // Estos son pines de la placa Arduino, se puede utilizar cualquiera de los analogos
  float value_3 = analogRead(A2);  // Estos son pines de la placa Arduino, se puede utilizar cualquiera de los analogos
  /* Enviando datos a ubidots */
  client.add(VARIABLE_LABEL_1, value_1);   
  client.add(VARIABLE_LABEL_2, value_2);
  client.add(VARIABLE_LABEL_3, value_3);
  client.sendAll();
  delay(5000);
}
```
Esta liberia te permite subir hasta  **5 variables** por solicitud. No se preocupe si ocupa mas, si decides enviar mas de 5 variables, necesitas abrir un nuevo ejemplo **Archivo -> Ejemplos -> ubidots-arduino-ethernet library** y seleccione el ejemplo **"UbidotsSaveMultiValues"** . No olvide asignar el toquen [TOKEN](http://help.ubidots.com/user-guides/find-your-token-from-your-ubidots-account), e indicar las etiquetas de las variables que requiere, tambien compiar la MAC Address de su tarjeta de red de su Arduino.

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


## Obtener los ultimos valores de un sensor

Para obtener los ultimos valores de un sensor, dirijase a **Archivo -> Ejemplos -> UbidotsArduino library** y selecciona el ejemplo **"UbidotsGetValue"** .

Asigne el [TOKEN](http://help.ubidots.com/user-guides/find-your-token-from-your-ubidots-account), el dispositivo y etiquetas de variable de donde usted desea obtener los datos, no olvide la MAC Address de su tarjeta.

Cargue el codigo, abra el monitor serial para ver los datos .

**NOTE**: Algunas veces el Arduino se queda congelado, se recomienda desconectar el cable y volverlo a conectar.

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

## Manejo del tablero
El manejo del table es muy sensillo he intuitivo, para cuando estamos subiendo datos o requerimos monitorear los valores de los sensores en tiempo real desde alguna localización, colgarlo en una página web o compartirlo con un usuario final o cliente.

Crear tablero ver [imagen](/imagenes/tablero.png)


**Definir que tipo de widget quiero**

Para esto utilizaremos el que mejor se acople a los valores que quiero desplegar y cuantos, hay varias posibilidades como se puede ver en la siguiente [imagen](/imagenes/visualizardata.png)

Una vez seleccionado debo indicar que quiero que me muestre, si un promedio, mayor, menor, igual, el último valor o cuantos se han almacenado. Además se debe indicar si quiero los valores del día, de la semana o del mes. La data se puede descargar de Ubidots si requerimos analizarla diferente. Puede guiarse en la siguiente [imagen](/imagenes/muestre.png)

Por último debe indicar cual dispositivo y variable quiere ver, puede seleccionar de varios dispositivos para el mismo tablero, y diferentes variables. Puedes mezclarlos en la misma vista si es eso lo que seas. Se puede guiar en la siguiente [imagen](/imagenes/dispositivovariable.png)

Ahora con tu tablero creado puedes usar las opciones de compartir o empotrar tu tablero. Se puede guiar en esta [imagen](/imagenes/compartirtablero.png)

# Como conectar un Feather Lora Radio

Como se pudo observar en la presentacion del taller las redes LoraWAN estan desplegadas por todo el mundo, gracias a su versatilidad y bajo consumo de electricidad se perfila como un actor muy importante para desarrollar algunas aplicaciones de IoT.

Para que nuestro Radio Feather M0 RFM95 para **900 Mhz** con chip de SX127x de Lora que en una frecuencia de banda libre (en muchos paises de America Latina la banda 900Mhz es libre debe verificar en el suyo) funcione es requerido tener dos rádios cercanos unos 400 a 600 metros. Lo común es utilizar una pasalera comunitaria de la red LoraWAN que esta soportada en la infraestructura que "The Things Network" facilita a nivel mundial de manera gratuita. Puede verificar si tiene pasarelas cercanas a usted en este [enlace](https://www.thethingsnetwork.org/). 

No se estará realizando completamente la configuración del Gateway que se esta utilizando dada la gran variedad, pero se dejan los enlaces al que se utilizó para el taller, más enlaces a otros que pueden ser adquiridos para realizar pruebas, en caso de que estes cerca de un Gateway de lora solo debes verificar la distancia para que tu antena sea de un tamaño y potencia que te permita acceder a ese Gateway(lora no tiene tanta perdida como wifi atravesando paredes).

Algunas puertas de enlace que TTN promueve pueden verse [aqui](https://www.thethingsnetwork.org/marketplace/products/devices).

El que se utiliza para este laboratorio se puede comprar bajo el modelo RAK 831, tiene un costo aproximado de 300$ y hay muchos manuales de configuración y puesta en marcha.
Pueden verlo en la siguiente 
![imagen](/imagenes/kit7_1024x.jpg).

Algunos manuales disponibles de mucha utilidad para realidad la instalación:
* De TTN [este](https://www.thethingsnetwork.org/docs/gateways/rak831/)
* De RAK [este](https://www.hackster.io/naresh-krish/getting-started-with-the-rak831-lora-gateway-and-rpi3-e3351d)
* En video funciona [este](https://www.youtube.com/watch?v=bea7g5isD0w)

Para el Feather M0 pueden comprarlo en multiples páginas por ejemplo>
* Adafruit [este](https://www.adafruit.com/product/3178)
* Amazon [este](https://www.amazon.com/-/es/165/dp/B01MRY3ETX/ref=sr_1_1?__mk_es_US=%C3%85M%C3%85%C5%BD%C3%95%C3%91&dchild=1&keywords=Feather+M0+with+RFM95+LoRa&qid=1600101003&sr=8-1)
* Mouser [este](https://co.mouser.com/ProductDetail/Adafruit/3178?qs=TlVEbN%2FgKDkhUZkXCJivzw%3D%3D)

A nuestro feather le hemos conectado un dth22 y un acelerometro adxl345. Ilustrados en la siguiente imagenes.

Sensor de Humedad Temperatura DHT22
![DTH22](/imagenes/dht22-pinout_2P1AgF3wPs.png.jpeg)

Acelerometro ADXL345
![Acelerometro ADXL345](/imagenes/acelerometro-3-ejes-adxl345-2g4g8g16g.jpg)

Sugerencia de cableado de los dispositivos al feather m0.

Para el de Humedad Temperatura, en la vista se puede apreciar la antena del Radio de Lora.
![Temperatura-Humedad](/imagenes/weather_dht22_fritzing.png)

Para el Acelerometro
![Acelerometro](/imagenes/feather-and-ADXL345-layout.png)

**Nota imporante nunca conecte el feather radio a un alimentación o bateria sin haber soldado la antenna, esto puede quedar la tarjeta**

Estos pueden cambiar para su conficuración personal, pero debe tener en cuenta lo siguiente:
* El feather M0 solo tiene un conexion de GND o tierra y una de VCC o voltaje, por lo que deberá alambrar y soldar un linea para darle esos conexiones a ambos sensores
* No debe cargar la linea de VCC y GND demasiado, aunque tiene 10 pines analógicos entre más sensores má bateria
* Si alambra más sensores para evitar degradar la señal ocupará un capacitor
* A este fearher le conectaremos una bateria de 3.7V no exceda ese maximo ya que puede causar que la bateria se termine muy rápido


Ahora tenemos claro que estamos usando debemos soldar y cablear todo con cuidado, se pueden utilizar protoboard si el sensor es para pruebas.

Sensor puede verse de esta 
![manera](/imagenes/sensor%20completo.jpeg)

Sensor con bateria de litio 
![asi](/imagenes/sensor%20con%20bateria.jpeg)

Gateway vista 
![uno](/imagenes/gateway%20rak%20831.jpeg)

Gateway vista 
![dos](/imagenes/vista%20gateway.jpeg)









