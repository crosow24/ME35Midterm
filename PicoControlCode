#Pico Control Code
import network
import time
import math
import ubinascii
import urequests as requests
from machine import Pin, UART, I2C, PWM, ADC
import mqttAda as mqtt
from wifisecrets import Tufts_Wireless as wifi
import accelLib as accel

def connect_wifi(wifi):
    station = network.WLAN(network.STA_IF)
    station.active(True)
    mac = ubinascii.hexlify(network.WLAN().config('mac'),':').decode()
    print("MAC " + mac)

    station.connect(wifi['ssid'],wifi['pass'])
    while not station.isconnected():
        time.sleep(1)
    print('Connection successful')
    print(station.ifconfig())
def whenCalled(topic, msg):
    global wcdata
    print((topic.decode(), msg.decode()))
    # wcdata = (topic.decode(), msg.decode())
    wcdata = msg.decode()
    led.on()
    time.sleep(0.5)
    led.off()

led12 = machine.Pin(18, machine.Pin.OUT)
led11 = machine.Pin(19, machine.Pin.OUT)
led10 = machine.Pin(20, machine.Pin.OUT)
led9 = machine.Pin(21, machine.Pin.OUT)
led8 = machine.Pin(13, machine.Pin.OUT)
led7 = machine.Pin(22, machine.Pin.OUT)
led6 = machine.Pin(14, machine.Pin.OUT)
led5 = machine.Pin(26, machine.Pin.OUT)
led4 = machine.Pin(27, machine.Pin.OUT)
led3 = machine.Pin(15, machine.Pin.OUT)
led2 = machine.Pin(16, machine.Pin.OUT)
led1 = machine.Pin(17, machine.Pin.OUT)
ledF = machine.Pin(2, machine.Pin.OUT)
ledC = machine.Pin(4, machine.Pin.OUT)
ledK = machine.Pin(6, machine.Pin.OUT)
thermistor = ADC(Pin(28))

ledArray=[led1, led2, led3, led4, led5, led6, led7, led8, led9, led10, led11, led12]


for x in ledArray:
    x.on()
    time.sleep(.1)
    x.off()
connect_wifi(wifi)
fred = mqtt.MQTTClient('PicoTemp', 'io.adafruit.com', keepalive=60)
fred.connect()
fred.set_callback(whenCalled)
fred2 = mqtt.MQTTClient('PicoUnit', 'io.adafruit.com', keepalive=60)
fred2.connect()
fred2.set_callback(whenCalled)
id="REDACTED"
key = "REDACTED"
url = f"REDACTED"
headers = {'Authorization': 'Bearer ' + str(key),'Content-Type':'application/json'}

acc = accel.accelerometer(0,1,0)

modeF=True
modeK=False
modeC=False
modeChange=True #save prev value and compare
PrevColor="None"
while True:
    gyroVector=acc.read_g()
    gyroVectorX=gyroVector[0]
    response = requests.request("GET", url, headers=headers)
    records=response.json()
    color=records['fields']['Status']
    print(color)
    RawADC=thermistor.read_u16()
    #print(RawADC)
    Voltage=3.3*(RawADC/65535)
    #print(Voltage)
    ConvADC=(2161.5/Voltage)
    TempK = 1 / ((1/363)+(1/4000)*math.log(ConvADC/50));
    TempC = TempK-273.15
    TempF = TempC*(9/5)+32
    if color != PrevColor:
        for t in ledArray:
            t.off()
    if abs(gyroVectorX) > 120:
        color="None"
        gyroVector=acc.read_g()
        gyroVectorX=gyroVector[0]
        ledF.off()
        ledK.on()
        ledC.off()
        tempBenchmark=round((int(round(TempK, -1)))/30)
        for i in ledArray[:tempBenchmark]:
            i.on()
        msg=str(TempK)
        msg2="Kelvin"
        fred.publish('REDACTED',msg)
        fred2.publish('REDACTED',msg2)
        time.sleep(4)
        for t in ledArray:
            t.off()
    elif color == "Red":
        modeF=False
        modeK=False
        modeC=True
    elif color == "Blue":
        modeF=True
        modeK=False
        modeC=False
    if modeF:
        ledF.on()
        ledK.off()
        ledC.off()
        tempBenchmark=int((round(TempF, -1)/10)+2)
        for y in ledArray[:tempBenchmark]:
            y.on()
        msg=str(TempF)
        msg2="Fahrenheit"
        fred.publish('REDACTED',msg)
        fred2.publish('REDACTED',msg2)
    elif modeC:
        ledF.off()
        ledK.off()
        ledC.on()
        tempBenchmark=int((round(TempC, -1)/10)+4)
        for z in ledArray[:tempBenchmark]:
            z.on()
        msg=str(TempC)
        msg2="Celsuis"
        fred.publish('REDACTED',msg)
        fred2.publish('REDACTED',msg2)
    prevColor=color
        
    time.sleep(10)
