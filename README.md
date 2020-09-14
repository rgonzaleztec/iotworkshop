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

No se requiere más equipo conectado solo requerimos un cable para conectar nuestro dispositivo al computador para configurarlo. Verifique que su placa ethernet pudo conectarse a la red proporcionando electricidad a la placa con el adaptador de pared para su Arduino o mediante el cable conectado al puerto del computador. Para verificar si la placa esta funcionando la misma tiene leds de luz que deberán estar parpadeando.

Si no tiene instalado o no ha trabajado con Arduino IDE debe seguir este [enlace](https://help.ubidots.com/en/articles/2033398-setting-up-the-arduino-ide-for-ubidots).

Si ya tiene instalado y configurado su Arduino IDE, debe instalar la libreria de Ethernet de Ubidots descarguela de [aquí](https://github.com/ubidots/ubidots-arduino-ethernet/archive/master.zip).

El archivo zip descargado se debe instalar dentro del IDE de Arduino en Sketch -> Include Library -> Add .ZIP Library. Luego reinicie el Arduino IDE para que se recargue la librería que acabos de subir.

Vamos a modificar el ejemplo que trae por defecto la libreria que acabamos de instalar.


# Sobre la Plataforma
Ubidots proporciona todo lo necesario para que usted pueda subir o bajar datos de los dispositivos, recuerte que estamos utilizando una cuenta STEM, por lo que solo podemos incluir tres dispositivos de manera perpetua. Si se desean más dispositivos o crear aplicaciones para producción lo recomendable es utilizar una cuenta industrial.

Ubidots nos da acceso a Tokens con los que podemos subir o bajar información, estos son los que aseguran que estamos accediendo a la cuenta correcta.
Cada dispositivo tiene su propio identificador, cada variable también por lo que es sencillo acceder para descargar información. Para subir información basta con tener el token y la url para Industria o STEM según el tipo de cuenta que se tenga.




# Como conectar un Arduino Uno

![Arduino con placa](/imagenes/shield-ethernet-para-arduino-uno-mega.jpg)

Desde luego que sin una tarjeta de red no se puede conectar a la Internet. No se requieren puertos especiales para acceder a la plataforma Ubidots, basta con tener acceso a Internet y navegar. Lo anterior es porque la plataforma tiene APIs seguros que permiten tanto subir como descargar datos de los sensores.

Se pueden utilizar muchos otros shields como el de WiFi o BLuetooth en el caso de este último se requiere que se conecte a algun dispositivo que le permita tener acceso a Internet o aplicación que recolecte y muestre la data como información. Lo anterior como punto final para dar información o generar una nueva acción.

# Como conectar un feather Lora


