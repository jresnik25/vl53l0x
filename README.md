# Trabajo Práctico Final: Sistemas Embebidos – Sensor VL53L0X con Raspberry Pi Pico (TinyGo)

## Descripción

En este trabajo utilizamos una **Raspberry Pi Pico** para:

1. Comunicarse mediante **I²C** con un sensor **VL53L0X**.
2. El sensor lee distancia/proximidad en milimetros (mm).
3. Se encienden hasta **3 LEDs** según la distancia medida.

La programación se realizo en el lenguaje **TinyGo**.

## Estructura del proyecto (carpetas)

### `prueba_comunicacion_i2c`

Se utilizó como prueba de que la comunicación I²C entre sensor y microcontrolador se estaban realizando bien. Fue el primer testeo para chequear la inicialización y conexiones.


### `pico-vl53l0x-read`

Carpeta principal del proyecto. Contiene dos archivos:
* `main.go`: el programa principal.
* `vl53l0x/`: driver para el sensor (lectura y configuración).

## LEDs

La distancia medida se observa en el terminal, pero además se utilizan tres diodos LED que sirven como indicación visual general de la distancia que se está midiendo. Estos parámetros pueden modificarse fácilmente.
Las dos conexiones de cada LED son:

* ánodo → a una resistencia ~330 Ω → distintos GPIO
* cátodo → GND (todos a tierra)
* Los pines usados son:

| LED  | GPIO Pico  |
| ---- | ---------  |
| LED1 | GPIO14     |
| LED2 | GPIO15     |
| LED3 | GPIO16     |

La lógica de encendido de los LED depende de la distancia y es la siguiente:

* Distancia < 100 mm o si hay error de lectura o un valor inválido → todos apagados 
* Distancia ≥ 100 mm → LED1
* Distancia ≥ 300 mm → LED1 + LED2
* Distancia ≥ 500 mm → LED1 + LED2 + LED3

## Conexiones del sensor VL53L0X

| VL53L0X       | Raspberry Pi Pico |
| ------------- | ----------------- |
| VIN / VCC     | 3V3               |
| GND           | GND               |
| SDA           | GPIO4             |
| SCL           | GPIO5             |

> El módulo VL53L0X tiene resistencias pull-up en SDA/SCL (por eso podemos conectar directo)

Para correr el proyecto, una vez conectado el mictrocontrolador, abrimos la terminal y desde la carpeta principal (`pico-vl53l0x-read`), se ejecuta la linea:
```powershell
tinygo flash -target pico -monitor .\main.go
```
De esta forma, además de compilar el programa, se imprime la distancia en el terminal. En este caso, las mediciones se realizan cada 1 segundo (también se puede modificar).

