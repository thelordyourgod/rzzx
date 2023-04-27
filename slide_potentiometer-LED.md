To control the intensity of a light (e.g., an LED) using a slide potentiometer and an ESP32 with MicroPython, follow these steps:

1. Connect the slide potentiometer and LED to the ESP32:

   For the slide potentiometer:
   - Connect one end of the potentiometer to 3.3V on the ESP32
   - Connect the other end of the potentiometer to GND on the ESP32
   - Connect the middle pin (wiper) of the potentiometer to an ADC pin on the ESP32 (e.g., GPIO34)

   For the LED:
   - Connect the anode (longer leg) of the LED to a PWM-capable GPIO pin on the ESP32 (e.g., GPIO4)
   - Connect the cathode (shorter leg) of the LED to GND through a current-limiting resistor (e.g., 330Ω)

2. Write a MicroPython script to read the potentiometer value and control the LED intensity:

```python
from machine import ADC, Pin, PWM
import time

# Initialize the ADC for the potentiometer
adc = ADC(Pin(34))
adc.atten(ADC.ATTN_11DB)  # Set the voltage range to 0-3.3V

# Initialize the PWM for the LED
led = PWM(Pin(4), freq=1000)  # Set the PWM frequency to 1kHz

# Main loop
while True:
    # Read the raw ADC value (0-4095)
    raw_value = adc.read()
    
    # Convert the raw ADC value to a duty cycle (0-1023)
    duty_cycle = int(raw_value * 1023 / 4095)
    
    # Set the LED intensity based on the duty cycle
    led.duty(duty_cycle)
    
    # Wait before reading again
    time.sleep(0.1)
```

In this example, the ESP32 reads the potentiometer value using ADC and controls the LED intensity using PWM. The script maps the raw ADC value (0-4095) to a duty cycle (0-1023) and sets the LED intensity accordingly. You can adjust the GPIO pin numbers and other parameters in the script to match your specific wiring and requirements.
