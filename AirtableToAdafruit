import requests
import json
import paho.mqtt.client as mqtt
import time


def whenCalled(topic, msg):
    global wcdata
    print((topic.decode(), msg.decode()))
    # wcdata = (topic.decode(), msg.decode())
    wcdata = msg.decode()
    led.on()
    time.sleep(0.5)
    led.off()
def GetIt(url,headers):
    reply = requests.get(url,headers)
    if reply.status_code == 200:
        return(reply.json()[element])
    else:
        print(reply.status_code)
        return('Error')
    
mqttc = mqtt.Client()
mqttc.username_pw_set("REDACTED", password="REDACTED")
mqttc.connect("io.adafruit.com")
id="REDACTED"
key = "REDACTED"
url = f"REDACTED"
headers = {'Authorization': 'Bearer ' + str(key),'Content-Type':'application/json'}
#fullTime = GetIt(url,headers)
#print(fullTime)
#VALUE="Red"
#data = {'value':VALUE}
#reply = requests.post(url,headers=headers,json=data)
#print(reply.status_code)

while True:
    response = requests.request("GET", url, headers=headers)
    records=response.json()
    print(response)
    print(records)
    color=records['fields']['Status']
    print(color)
    mqttc.subscribe('crosow24/feeds/unit')
    if color == "Red":
        msg="Celsuis"
        #fred.publish('REDACTED',msg)
        mqttc.publish('REDACTED',msg)
    if color == "Blue":
        msg="Fahrenheit"
        #fred.publish('REDACTED',msg)
        mqttc.publish('REDACTED',msg)
    time.sleep(10)
        
