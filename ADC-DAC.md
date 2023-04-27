In MicroPython on ESP32, you can use the built-in `machine` module to work with ADC (Analog-to-Digital Converter) and DAC (Digital-to-Analog Converter) functionalities.

**ADC (Analog-to-Digital Converter):**

ADC is used to measure an analog voltage and convert it into a digital value. The ESP32 has a built-in 12-bit ADC with a configurable voltage range of 0V - 1.1V, 0V - 2.2V, 0V - 3.3V, or 0V - 4.4V.

Here's an example of using ADC to read the voltage from a potentiometer:

1. Connect the potentiometer to the ESP32:
   - Connect one end of the potentiometer to 3.3V on the ESP32
   - Connect the other end of the potentiometer to GND on the ESP32
   - Connect the middle pin (wiper) of the potentiometer to an ADC pin on the ESP32 (e.g., GPIO34)

2. Write a MicroPython script to read the voltage:

```python
from machine import ADC, Pin
import time

# Initialize the ADC
adc = ADC(Pin(34))
adc.atten(ADC.ATTN_11DB)  # Set the voltage range to 0-3.3V

# Main loop
while True:
    # Read the raw ADC value (0-4095)
    raw_value = adc.read()
    
    # Calculate the voltage
    voltage = (raw_value / 4095) * 3.3
    
    # Print the voltage
    print("Voltage: {:.2f}V".format(voltage))
    
    # Wait before reading again
    time.sleep(1)
```

**DAC (Digital-to-Analog Converter):**

DAC is used to generate an analog voltage from a digital value. The ESP32 has a built-in 8-bit DAC on two pins: GPIO25 and GPIO26.

Here's an example of using DAC to generate a sine wave:

1. Connect an oscilloscope or a speaker to the DAC pin (e.g., GPIO25) and GND on the ESP32.

2. Write a MicroPython script to generate the sine wave:

```python
from machine import DAC, Pin
import time
import math

# Initialize the DAC
dac = DAC(Pin(25))

# Parameters for the sine wave
amplitude = 127
offset = 128
frequency = 1  # Hz
sampling_rate = 100  # samples per second

# Main loop
while True:
    for i in range(int(sampling_rate / frequency)):
        # Calculate the sine value
        value = int(amplitude * math.sin(2 * math.pi * frequency * i / sampling_rate) + offset)
        
        # Write the value to the DAC
        dac.write(value)
        
        # Wait for the next sample
        time.sleep(1 / sampling_rate)
```

This example generates a 1 Hz sine wave with an amplitude of 127 (out of 255) and an offset of 128. You can change the frequency, amplitude, and offset to suit your requirements.

Remember to adjust the GPIO pin numbers in both examples according to your specific wiring.
