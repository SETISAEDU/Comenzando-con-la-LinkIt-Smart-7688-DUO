# Comenzando-con-la-LinkIt-Smart-7688-DUO

Esta tarjeta de desarrollo nos permite desarrollar multiples tareas a la vez gracias a su Unidad de Procesamiento MT7688 de Mediatek y realiizar tareas de bajo nivel por parte del Microcontrolador ATmega32u4 con lo cual podremos cargar sketchs de Arduino. Con esta tarjeta ganamos dos entornos de desarrollo tanto de Linux y Arduino. La LinkIt Smart 7688 DUO cuenta con soporte de varios lenagujes de programación como lo que son Python, Node.JS y C.


# Lineas de códigos para el entorno de LINUX (Open WRT)

Para generar un nuevo archivo digitamos el comando `nano` luego ingresamos el nombre del archivo seguido de la extensión del lenguaje de programación a utilizar por ejemplo en python `nano programa1.py`

Luego de editar el archivo lo guardamos y lo ejecutamos con el comando de ejecución de Python, quedando de la siguiente manera `python programa1.py`

# Esquema de conexión de unos LEDs

Conectamos unos Leds a los pines GPIO 21 GPIO 20 y GPIO 43. Como se muestra en la siguiente imagen:

![LinkIt Smart 7688 DUO Diagram](https://raw.githubusercontent.com/SETISAEDU/Comenzando-con-la-LinkIt-Smart-7688-DUO/master/Diagrama_LinkIt_Parte%20II.png)


# Parpadeo de un LED

Con este código haremos el parpadeo de un LED conectador el pin GPIO 43

```
import mraa
import time

print "***** Demo Blink SETISA-EDU sobre SOC MT7688 *****"
T= input("Ingrese Tiempo: ")


Led1=mraa.Gpio(43)

Led1.dir(mraa.DIR_OUT)

while True:
	Led1.write(1)
	time.sleep(T)
	Led1.write(0)
	time.sleep(T)

```
# PWM en LinkIt Smart 7688 DUO

Con este código haremos generar un señal de PWM en los pines GPIO 21 y GPIO 20

```
import mraa
import time

def my_range(start, end, step):
    while start <= end:
        yield start
        start += step

print "***** Demo Blink con PWM SETISA-EDU sobre SOC MT7688 *****"
T= input("Ingrese Tiempo: ")


Led1=mraa.Pwm(21)
Led2=mraa.Pwm(20)

Led1.period_ms(2)
Led2.period_ms(2)

Led1.enable(True)
Led2.enable(True)


while True:
	for x in my_range(0,1,0.01):
		Led1.write(x)
		Led2.write(1-x)
		time.sleep(T)

	for x in my_range(0,1,0.01):
		Led1.write(1-x)
		Led2.write(x)
		time.sleep(T)

```

# Utilizando la MT7688 y el ATmega32u4

Cargamos el sketch de Arduino `Blink_32u4.ino` que se encuentra en este repositorio

Luego en el entorno de Linux cargamos el código de python

```
import serial
import time

s = None

def setup():
    # open serial COM port to /dev/ttyS0, which maps to UART0(D0/D1)
    # the baudrate is set to 57600 and should be the same as the one
    # specified in the Arduino sketch uploaded to ATMega32U4.
    global s
    s = serial.Serial("/dev/ttyS0", 57600)


def loop():
    # send "1" to the Arduino sketch on ATMega32U4.
    # the sketch will turn on the LED attached to D13 on the board
    s.write("1")
    time.sleep(1)
    # send "0" to the sketch to turn off the LED
    s.write("0")
    time.sleep(1)


if __name__ == '__main__':
    setup()
    while True:
        loop()

```

Y lo ejecutamos `python blink7688SERIAL.py`
