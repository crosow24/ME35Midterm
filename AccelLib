from machine import Pin, I2C
import struct, time

LSM = 0x6A

# maybe this goes here

rate = {'done': 0b0000,'12.5':0b0001,'26':0b0010,'52':0b0011,'104':0b0100,'208':0b0101,'416':0b0110,'833':0b0111,'1.66k':0b1000,'3.33k':0b1001,'6.66k':0b1010,'1.6':0b1011}
anti_alias = {'400':0b00,'200':0b01,'100': 0b10, '50':0b11}
XL_range = {'2g':0b00,'4g':0b10,'8g':0b11, '16g':0b01}
G_range = {'250':0b00, '500':0b01,'1000':0b10,'2000':0b11}
G_125_fullscale = 0

XLfactor = (0.061, 0.488, 0.122, 0.244)
Gfactor = (8.75, 17.50, 35.0, 70.0)

class accelerometer:
    def __init__(self, bus=1, scl=3, sda=2, freq=100000):
        self.bus = bus
        self.scl = scl
        self.sda = sda
        self.freq = freq
        self.i2c = I2C(id=bus, scl=Pin(scl), sda=Pin(sda), freq=freq)

        self.ID = self.i2c.readfrom_mem(LSM, 0x0F, 1)
        self.ID = struct.unpack('<b',self.ID)[0]

    def read_a(self):
        XL = (rate['208']<<4) + (XL_range['4g']<<2) + anti_alias['400'] # some kind of setup
        self.i2c.writeto_mem(LSM, 0x10, struct.pack('>b',XL)) # enable accel
        accel = self.i2c.readfrom_mem(LSM, 0x28, 6)   # read the i2c channel
        accel = struct.unpack('<hhh',accel)

        return accel

    def read_g(self):
        # 58 = high performance - 1000 dps
        G = (rate['1.66k']<<4) + (G_range['1000']<<2) + (G_125_fullscale <<1) + 0 # some kind of setup
        self.i2c.writeto_mem(LSM, 0x11, struct.pack('>b',G)) # enable gyro
        gyro = self.i2c.readfrom_mem(LSM, 0x22, 6)
        gyro = struct.unpack('<hhh',gyro)

        return gyro

    def calabrate_to_meter_per_sec(self):
        #Z-axis flat on table to run this
        vals = self.read_a()
        conversion = (vals[0])/9.8

        return conversion
