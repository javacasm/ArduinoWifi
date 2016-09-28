# Video

## Hardware

* Placa Arduino Uno Wifi que nos han enviado los amigos de Bricogeek. Se trata de una Developer Edition, con lo que podría cambiar el diseño final

* Los shields Wifi para Arduino han sido caros hasta ahora y según mi experiencia no demasiado buenos

* Esta placa utiliza para la comunicación Wifi al que es a día de hoy uno de los mayores competidores de Arduino: el [ESP8266](http://www.esp8266.com/)

* La comunicación se hace usando I2C, con lo que podemos seguir usando todas las patillas de Arduino (salvo el A4 y A5, como siempre que se usa I2C)

* El acabado de la placa es de calidad como nos tienen acostumbrados.

* Me gusta que los pines vengan etiquetados en el lateral, lo que facilita mucho el montaje

## Software

* Debemos usar el IDE de Arduino.org (personalmente no me gusta tener que usar 2 IDEs)

* Para usar las comunicaciones sólo tendremos que incluir la librería "ArduinoWifi" y utilizar el objeto Wifi. Veamos cómo:

* Existe un led que nos indica cuando hay comunicación con el wifi
