# 2.2-Desplegar-Temperatura
```python

import machine
import utime
from ssd1306 import SSD1306_I2C

sensor_temp = machine.ADC(4)

i2c = machine.I2C(0, scl=machine.Pin(21), sda=machine.Pin(20))
oled = SSD1306_I2C(128, 64, i2c)

def obtener_temperatura():
    lectura = sensor_temp.read_u16() * 3.3 / (65535)
    temperatura = 27 - (lectura - 0.706) / 0.001721
    return temperatura

def mostrar_imagen_temperatura(temperatura):
    if temperatura < 10:
        imagen = bytearray(b'\x00\x00\x00\x00\x00\x00\x00\x00')
    elif 10 <= temperatura < 30:
        imagen = bytearray(b'\xff\xff\xff\xff\xff\xff\xff\xff')
    else:
        imagen = bytearray(b'\x00\xff\x00\xff\x00\xff\x00\xff')
    oled.fill(0)
    oled.framebuf.blit_buffer(imagen, 0, 0, 8, 1)
    oled.show()

while True:
    temp = obtener_temperatura()
    print('Temperatura actual: {} °C'.format(temp))
    mostrar_imagen_temperatura(temp)
    utime.sleep(5)  ```
