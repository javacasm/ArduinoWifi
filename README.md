# Arduino UNO Wifi

He tenido el placer de probar una placa [Arduino UNO Wifi](http://www.arduino.org/products/boards/arduino-uno-wifi) que nos ha enviado Bricogeek para que la podamos probar.


En un [curso de nuevas tecnologías](https://github.com/javacasm/EscuelaArte) para algunos profesores la [Escuela de Arte de Granada](http://www.escuelaartegranada.com/) estábamos tratando las comunicaciones y tras trabajar con [Bluetooth](https://github.com/javacasm/EscuelaArte/blob/master/bluetooth.md), vamos a trabajar con el wifi.

## ¿Arduino Wifi?

La placa [Arduino UNO Wifi](http://www.arduino.org/products/boards/arduino-uno-wifi) no es otra cosa que una placa Arduino UNO con un [ESP8266](http://www.esp8266.com/) integrado que le proporciona la conectividad Wifi y que están conectados entre si por medio I2C con lo que tenemos disponible todos  los pines.

![1](./images/ArduinoWifi_back.jpg)
![1](./images/ArduinoWifi_front.jpg)     

## ¿Wifi?

Como hemos dicho la placa incluye un ESP8266 que nos permite tanto crear una red Wifi como conectarnos a una dada. [¿Cómo configurar el wifi?](http://www.arduino.org/learning/getting-started/getting-started-with-arduino-uno-wifi)

### Características

    Microcontrolador: ATMega328 @ 16 MHz
    Módulo Wifi ESP8266EX @ 80 MHz 802.11 b/g/n 2.4 GHz
    Voltaje de funcionamiento (pines I/O): 5V
    Alimentación: 7 a 12 V
    Memoria Flash: 32 Kb
    SRAM: 2Kb
    Pines I/O: 20
    Salidas PWM: 6
    Consumo: 93 mA
    Pines analógicos: 6
    EEPROM: 1Kb

## Contenido de la caja

![1](./images/CajaArduinoWifi.jpg)
![1](./images/ArduinoWifi_base.jpg)  
![1](./images/CajaArduinoWifi_back.jpg)  
![1](./images/Contenido.jpg)

## Uso

Descargamos el [IDE de arduino.org](http://www.arduino.org/downloads) (la numeración es engañosa y no quiere decir que sea más avanzado que la versión 1.6.9 de arduino.cc)

A partir de aquí podemos programar la placa de la forma standard.

## Código

Como siempre tenemos disponibles ejemplos

### Webserver

Se trata de poder ver el valor de las entradas analógica desde un navegador web conectándolos a la ip del Arduino UNO Wifi

    #include <Wire.h>
    #include <ArduinoWiFi.h>

    /* Nos conectamos a la IP de la placa arduino (192.168.240.1 por defecto) http://<IP>/arduino/webserver/ or http://<hostname>.local/arduino/webserver/

    http://labs.arduino.org/WebServer

    */
    void setup() {
        Wifi.begin();
        Wifi.println("WebServer Server arrancado");
    }
    void loop() {

        while(Wifi.available()){
          process(Wifi);
        }
      delay(50);
    }
    void process(WifiData client) {
      // Leemos el comando (petición)
      String command = client.readStringUntil('/');

      if (command == "webserver") {
        WebServer(client);
      }
    }
    void WebServer(WifiData client) {
              client.println("HTTP/1.1 200 OK");
              client.println("Content-Type: text/html");
              client.println("Connection: close");  
              client.println("Refresh: 20");  // refresh the page automatically every  sec
              client.println();      
              client.println("<html>");
              client.println("<head> <title>UNO WIFI Example</title> </head>");
              client.print("<body>");

              for (int analogChannel = 0; analogChannel < 4; analogChannel++) {
                int sensorReading = analogRead(analogChannel);
                client.print("analog input ");
                client.print(analogChannel);
                client.print(" is ");
                client.print(sensorReading);
                client.print("<br/>");
              }

              client.print("</body>");
              client.println("</html>");
              client.print(DELIMITER); // very important to end the communication !!!          
    }

### Webserver con capacidad de controlar pines



    #include <Wire.h>
    #include <ArduinoWiFi.h>
    /*
    on your borwser, you type http://<IP>/arduino/webserver/ or http://<hostname>.local/arduino/webserver/

    http://labs.arduino.org/WebServerBlink

    */
    void setup() {
        pinMode(13,OUTPUT);
        Wifi.begin();
        Wifi.println("WebServer Server is up");
    }
    void loop() {

        while(Wifi.available()){
          process(Wifi);
        }
      delay(50);
    }

    void process(WifiData client) {
      // read the command
      String command = client.readStringUntil('/');

      // is "digital" command?
      if (command == "webserver") {
        WebServer(client);
      }

      if (command == "digital") {
        digitalCommand(client);
      }
    }

    void WebServer(WifiData client) {

              client.println("HTTP/1.1 200 OK");
              client.println("Content-Type: text/html");
              client.println();
              client.println("<html>");

              client.println("<head> </head>");
              client.print("<body>");

              client.print("Click<input type=button onClick=\"var w=window.open('/arduino/digital/13/1','_parent');w.close();\"value='ON'>pin13 ON<br>");
              client.print("Click<input type=button onClick=\"var w=window.open('/arduino/digital/13/0','_parent');w.close();\"value='OFF'>pin13 OFF<br>");

              client.print("</body>");
              client.println("</html>");
              client.print(DELIMITER); // very important to end the communication !!!

    }

    void digitalCommand(WifiData client) {
      int pin, value;

      // Read pin number
      pin = client.parseInt();

      // If the next character is a '/' it means we have an URL
      // with a value like: "/digital/13/1"
      if (client.read() == '/') {
        value = client.parseInt();
        digitalWrite(pin, value);
      }

      // Send feedback to client
      client.print(F("Pin D"));
      client.print(pin);
      client.print(F(" set to "));
      client.print(value);
      client.print(EOL);

    }



## Opinión

Hasta hace poco las placas wifi para arduino eran caras (entre los 60€ y 70€), consumían mucho y no funcionaban demasiado bien. Tengo en casa varias y ninguna de ellas es realmente operativa.

Teníamos la opción de la Arduino Yun, pero a un precio rondando los 70€.

La arduino UNO Wifi me ha parecido una placa muy interesante, con buenas prestaciones, un precio muy ajustado y que facilita totalmente acceso wifi.
