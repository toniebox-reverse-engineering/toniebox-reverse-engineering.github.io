# Power
## SD
```
from machine import Pin
p_sdpower = Pin('GP3', mode=Pin.OUT)
p_sdpower(0) # Power on SD
```
## RFID, Audio, Acceleration Sensor
```
from machine import Pin
p_power = Pin('GP6', mode=Pin.OUT)
p_power(1)
```

# SD Access
```
from machine import SD
import os
sd = machine.SD(pins=('GP10', 'GP11', 'GP9')) # SD must be powered on!
os.mount(sd, '/sd')
os.listdir('/sd')
```

# LEDs
```
from machine import Pin
#p_led_r = Pin('', mode=Pin.OUT) # TODO is analog pin 19 (TCK)
p_led_g = Pin('GP25', mode=Pin.OUT)
p_led_b = Pin('GP24', mode=Pin.OUT) # Power Pin must be on

p_led_g(1) # Switch LED on
p_led_g(0) # Switch LED off
```

# Charger
```
from machine import Pin
p_charger = Pin('GP17', mode=Pin.IN)
p_charger.value() # charger connected?
```

# Battery state
```
from machine import ADC
adc = ADC()
p_battery = adc.channel(pin='GP5')
p_battery.value() # value see https://github.com/toniebox-reverse-engineering/toniebox/wiki/Battery-pack-&-power-supply#adc-voltage-map
```

# Ear buttons
from machine import Pin
```
p_ear_big = machine.Pin('GP2', mode=Pin.IN)
p_ear_sml = machine.Pin('GP4', mode=Pin.IN)
p_ear_big.value()
p_ear_sml.value()
```

# Disable Heartbeat
```
import wipy
wipy.heartbeat(False) 
```