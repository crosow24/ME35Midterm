# ME35Midterm
Hello and welcome to my Repo for my ME35 Midterm!

Heres how everything fits together: The Pico code takes readings from a thermistor, while the openCV script can trigger what unit the user desires to see temperature in by using either red or blue colored items as triggers. These trigger values are uploaded to an Airtable, which can be read via RestAPI by both the Pico and a Python3 script. Both the Pico and the Python3 script may update the Adafruit dashboard. Additionally, the unit may be changed to Kelvin using an I2C accelerometer (see Pico code for details).

Required libraries: requests, json, paho.mqtt.client, time, openCV, network, math, ubinascii, urequests as requests, machine (Pin, UART, I2C, PWM, ADC), mqttAda (see file in repo), wifisecrets (not included for safety reasons), and accelLib
