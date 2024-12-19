# DHT22-CON-LCD
**Propósito**

La práctica tiene como finalidad aprender a manejar la placa ESP32 junto con el sensor de temperatura y humedad DHT22, además de incorporar una pantalla LCD para visualizar las lecturas obtenidas. Este ejercicio introduce al estudiante a la programación de sensores y al diseño de una interfaz de usuario básica mediante una pantalla LCD.
________________________________________
**Materiales y equipo necesario**

1.	Plataforma Wokwi: Simulador de circuitos electrónicos que permite probar el código sin necesidad de hardware físico
2.	Placa ESP32: Microcontrolador económico con conectividad Wi-Fi y Bluetooth.
3.	Sensor DHT22: Sensor digital diseñado para medir temperatura y humedad.
4.	Pantalla LCD 20x4 con I2C: Pantalla de cristal líquido controlada mediante protocolo I2C, ideal para presentar datos al usuario.
________________________________________

**Conexiones principales**
•	Sensor DHT22: Conecta el pin de señal del sensor al GPIO 15 de la ESP32.
•	Pantalla LCD: Usa el protocolo I2C con la dirección 0x27 para conectar la pantalla a la ESP32. Los pines SDA y SCL son utilizados para la comunicación.

![](https://github.com/marcorea97/DHT22-CON-LCD/blob/main/CAPTURA%20MODO%20CONEXION%20LCD.png)

________________________________________

**Descripción del programa**

El código tiene como objetivo leer datos de temperatura y humedad desde el sensor DHT22 y mostrarlos en una pantalla LCD. Paralelamente, los valores también se envían al monitor serie para su visualización en texto.
Código fuente

#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>

#define I2C_ADDR 0x27
#define LCD_COLUMNS 20
#define LCD_LINES 4
const int DHT_PIN = 15;

DHTesp dhtSensor;
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {
    Serial.begin(115200);
    dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
    lcd.init();
    lcd.backlight();
}

void loop() {
    TempAndHumidity data = dhtSensor.getTempAndHumidity();
    Serial.println("Temp: " + String(data.temperature, 1) + "°C");
    Serial.println("Humidity: " + String(data.humidity, 1) + "%");

    lcd.setCursor(0, 0);
    lcd.print(" Temp: " + String(data.temperature, 1) + "\xDF" + "C ");
    lcd.setCursor(0, 1);
    lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
    lcd.print("Wokwi Online IoT");

    delay(1000);
}
________________________________________

**Análisis del código**

1.	Librerías incluidas:
DHTesp para interactuar con el sensor DHT22.
LiquidCrystal_I2C para manejar la pantalla LCD.
2.	Configuración inicial (setup):
Inicializa el sensor DHT22 en el GPIO 15.
Prepara la pantalla LCD y activa su retroiluminación.
4.	Lectura y visualización (loop):
Obtiene datos de temperatura y humedad con getTempAndHumidity().
Envía los valores al monitor serie.
Muestra los datos en la pantalla LCD usando lcd.print().
5.	Intervalo de actualización:
Usa delay(1000) para pausar un segundo entre lecturas.
________________________________________
**Resultados**
•	Temp: 24.0°C  
•	Humidity: 40.0%  

![](https://github.com/marcorea97/DHT22-CON-LCD/blob/main/CAPTURA%20%20LCD%20TEMP%20Y%20HUM.png)
________________________________________
**Conclusión**
Esta actividad permite integrar hardware y software para desarrollar un sistema básico de monitoreo ambiental en tiempo real. Usar la simulación en Wokwi facilita la validación del código y la revisión de los resultados sin necesidad de hardware físico. Este enfoque práctico refuerza el conocimiento sobre sensores, comunicación serial y el manejo de pantallas LCD con la ESP32.
