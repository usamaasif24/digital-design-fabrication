# Exercise 1: Electrical Circuits

**Digital Design & Fabrication**  
Carl von Ossietzky Universität Oldenburg · May 2026

---

## Task 1.1 – Simple LED Circuit

A green LED was connected in series with a current-limiting resistor between a 5 V supply and ground. The circuit was tested with three resistor values: 220 Ω, 1 kΩ, and 4.7 kΩ. Voltage across the resistor (V1) and across the LED (V_LED) were measured for each.

<p align="center">
  <img src="task1_1_circuit.jpg" width="500">
</p>
<p align="center"><em>Fig 1: Simple LED circuit with 220 Ω resistor, Vcc = 5 V</em></p>

| R1 | V1 | V_LED |
|----|----|-------|
| 220 Ω | 2.10 V | 2.76 V |
| 1000 Ω | 2.53 V | 2.45 V |
| 4700 Ω | 2.71 V | 2.29 V |

**Observations:** As R1 increases, the voltage drop across the resistor (V1) increases while V_LED decreases. The sum V1 + V_LED ≈ 5 V at all times, confirming Kirchhoff's Voltage Law. The LED forward voltage remains relatively stable (~2.3–2.8 V) regardless of resistor value, while the current decreases significantly with higher resistance — causing the LED to dim. Changing R1 does affect the LED brightness: at 220 Ω it glows brightly, at 4.7 kΩ it is very dim but still on.

**What went wrong:** During initial setup the LED did not light up. After inspection it was found that the LED had been inserted in the wrong orientation — reversed polarity. Flipping the LED in the breadboard resolved the issue immediately. This highlighted the importance of checking component polarity before applying power.

---

## Task 1.2 – Switchable LED Circuit

A toggle switch (S1) was added in series with the circuit. The switch was tested in both orientations.

<p align="center">
  <img src="task1_2_switch.jpg" width="500">
</p>
<p align="center"><em>Fig 2: Switchable LED circuit with toggle switch S1</em></p>

**Observations:** When the switch is open, the circuit is broken and no current flows — the LED is off. When closed, the LED lights at full brightness. Reversing the switch orientation produced identical behaviour, confirming that a mechanical SPST switch is non-polarised. The switch works correctly regardless of which terminal faces the supply or ground.

---

## Task 1.3 – Dimmable LED Circuit

A 1 kΩ potentiometer (R2) was added in series, with its wiper as the variable output. R1 was set to 100 Ω. Voltages were measured at three potentiometer positions.

<p align="center">
  <img src="task1_3_potentiometer.jpg" width="500">
</p>
<p align="center"><em>Fig 3a: Dimmable LED circuit with potentiometer</em></p>

<p align="center">
  <img src="task1_3_notes.jpg" width="500">
</p>
<p align="center"><em>Fig 3b: Lab notebook measurements</em></p>

https://github.com/user-attachments/assets/a8bee591-fca0-44c0-987e-4fc35da2f613

<p align="center"><em>Video 1: Demonstration of the dimmable LED circuit with potentiometer.</em></p>

| Position | V_LED | V2 |
|----------|-------|----|
| Full brightness | 2.97 V | 2.99 V |
| Dimmed | 2.88 V | 4.55 V |
| OFF | 0.37 V | 4.58 V |

**Observations:** The potentiometer acts as a variable voltage divider. As its resistance increases, V2 rises and V_LED falls. When V_LED drops below the LED forward voltage threshold (~2 V), the LED turns off completely. The relationship is inverse and continuous — rotating the wiper provides smooth, analogue brightness control. V2 + V_LED ≈ 5 V at all positions, confirming KVL.

**What went wrong:** With R1 = 100 Ω the potentiometer regulation range was very narrow — the LED transitioned from full brightness to off with only a very small rotation of the wiper. As suggested in the manual, R1 was replaced with 220 Ω which provided a noticeably wider and smoother dimming range.

---

## Task 2.1 – Switchable LED Strip

An N-channel MOSFET (IRLZ44N) was used to switch a 12 V LED strip. The control side runs on 5 V from a USB module; the load side on 12 V. A 10 kΩ pull-down resistor holds the gate low when the switch is open, and a 100 Ω gate resistor limits transient current. Both sides share a common ground.

<p align="center">
  <img src="task2_1_strip_a.jpg" width="500">
</p>
<p align="center"><em>Fig 4a: MOSFET switchable LED strip circuit</em></p>

<p align="center">
  <img src="task2_1_strip_b.jpg" width="500">
</p>
<p align="center"><em>Fig 4b: Full breadboard layout with XIAO MCU</em></p>

**Observations:** When S1 is open, the pull-down resistor holds V_GS at 0 V — the MOSFET is off and the strip is dark. When S1 is closed, 5 V is applied to the gate, raising V_GS above the threshold (~2 V). The MOSFET conducts and the strip lights fully. The switch controls V_GS, not the load voltage directly. This demonstrates the key advantage of a transistor switch: a small 5 V gate signal controls a high-current 12 V load without direct electrical connection between the two voltage domains.

---

## Task 2.2 – Dimmable LED Strip (PWM)

The switch was replaced with a PWM signal generator powered from the USB module. Two parameters were investigated: duty cycle and switching frequency.

### Part A — Duty Cycle (f = 90 Hz)

<p align="center">
  <img src="task2_2_duty_02.jpg" width="500">
</p>
<p align="center"><em>Fig 5a: D = 2% — barely visible</em></p>

<p align="center">
  <img src="task2_2_duty_15.jpg" width="500">
</p>
<p align="center"><em>Fig 5b: D = 15% — dim</em></p>

<p align="center">
  <img src="task2_2_duty_40.jpg" width="500">
</p>
<p align="center"><em>Fig 5c: D = 40% — medium brightness</em></p>

<p align="center">
  <img src="task2_2_duty_60.jpg" width="500">
</p>
<p align="center"><em>Fig 5d: D = 60% — bright</em></p>

<p align="center">
  <img src="task2_2_duty_100.jpg" width="500">
</p>
<p align="center"><em>Fig 5e: D = 100% — full brightness</em></p>

| Duty Cycle | V_LSD |
|-----------|-------|
| 2% | 0.03 V |
| 15% | 2.28 V |
| 40% | 2.77 V |
| 75% | 2.90 V |
| 100% | 2.99 V |

**Observations:** Brightness increases proportionally with duty cycle. At 2% the strip is barely visible; at 100% it reaches full brightness. The MOSFET switches on and off rapidly — the human eye integrates the pulses into a steady perceived intensity equal to the duty cycle percentage of full brightness. Compared to the potentiometer in Task 1.3, PWM is more energy efficient as no power is wasted as heat in a resistor.

### Part B — Switching Frequency (D = 50%)

https://github.com/user-attachments/assets/e2ec74df-cd70-4559-a760-28ac156d2e80

<p align="center"><em>Video 2: Demonstration of PWM duty cycle control on LED strip.</em></p>

| Frequency | Observation |
|-----------|-------------|
| 5 Hz | Clearly visible slow flashing — eye resolves individual pulses |
| 25 Hz | Fast flicker, still clearly uncomfortable |
| 45 Hz | Barely perceptible — near flicker fusion threshold |
| 100 Hz | No flicker — smooth steady glow at 50% brightness |

**Observations:** At low frequencies the eye resolves individual on/off pulses as visible flicker. As frequency increases past approximately 45–50 Hz — the human visual flicker fusion threshold — the brain can no longer distinguish individual pulses and perceives a continuous steady light. At 100 Hz the strip appears completely smooth. Frequency does not affect brightness — only duty cycle does.

**What went wrong:** At 2% duty cycle the strip looked completely off at first. We double checked all connections but everything was fine — the strip was just so dim it was almost invisible in normal room lighting. Also at 5 Hz the flicker was much stronger than expected and was quite uncomfortable to look at directly.

---

---

# Exercise 2: Introduction to Arduino

**Digital Design & Fabrication**  
Carl von Ossietzky Universität Oldenburg · May 2026

---

## Overview

In this exercise a functional alarm clock was built using the Arduino Uno. The system was built component by component — starting with a buzzer, then adding an LCD screen, a real-time clock module, and finally push buttons to control the alarm. Each sub-circuit was tested individually before combining everything into the final alarm clock.

---

## Sub-circuit 1 – Connecting the Buzzer

The buzzer was connected to digital pin 12 of the Arduino Uno through a 220 Ω current-limiting resistor. The `buzzer_test.ino` sketch was uploaded and the pin was toggled between HIGH and LOW states with different delay values to produce various sounds.

<p align="center">
  <img src="sub1_buzzer.jpg" width="500">
</p>
<p align="center"><em>Fig 6: Buzzer connected to Arduino Uno via 220 Ω resistor on breadboard</em></p>

https://github.com/usamaasif24/digital-design-fabrication/raw/main/video_buzzer.mp4

<p align="center"><em>Video 3: Buzzer producing different tones by changing delay values in code</em></p>

**Observations:** When the code was first uploaded the buzzer produced a continuous beeping tone. By reducing the delay between HIGH and LOW states the pitch of the sound increased noticeably — shorter delays produced a higher and more rapid tone, while longer delays produced slower and deeper beeps. Changing the sequence of HIGH/LOW patterns also changed the rhythm of the sound, which showed how digital output pins can directly control audio behaviour.

**What went wrong:** The buzzer did not make any sound at first. After checking the wiring we found that the positive leg of the buzzer was connected to the wrong pin. Moving it to pin 12 as specified in the code fixed it immediately. We also noticed that without the resistor the buzzer was very loud — the resistor helped reduce the current and bring it to a more reasonable volume.

---

## Sub-circuit 2 – Connecting the LCD Screen

The LCD is a 2x16 character display that communicates over the I2C protocol using only 4 wires — VCC, GND, SDA (A4) and SCL (A5). The `I2C_scanner.ino` sketch was run first to find the LCD address, which came back as `0x27`. The `LCD_test.ino` sketch was then used to print text to the screen.

<p align="center">
  <img src="sub2_lcd.jpg" width="500">
</p>
<p align="center"><em>Fig 7: Arduino Uno connected to I2C LCD screen displaying "Hello! LCD Working"</em></p>

https://github.com/usamaasif24/digital-design-fabrication/raw/main/video_lcd.mp4

<p align="center"><em>Video 4: LCD screen displaying test message after successful I2C configuration</em></p>

**Observations:** Once the correct I2C address was set in the code the display responded immediately and showed the test message "Hello! LCD Working" across two lines. The I2C protocol made the wiring simple — only 4 wires were needed compared to the 8+ wires a parallel LCD connection would require. The display was clear and readable with good contrast.

**What went wrong:** The LCD showed nothing when the code was first uploaded. Running the I2C scanner revealed the address was `0x27` and not the `0x20` we had initially assumed. Updating the address in the sketch fixed the display straight away.

---

## Sub-circuit 3 – Real Time Clock (RTC)

The RTC module was added to the existing I2C bus alongside the LCD, sharing the same SDA and SCL lines. Running the I2C scanner with both devices connected now reported two addresses — `0x27` for the LCD and `0x68` for the RTC. The `RTC_LCD_test.ino` sketch was used to read the current time from the RTC and display it live on the LCD.

<p align="center">
  <img src="sub3_rtc.jpg" width="500">
</p>
<p align="center"><em>Fig 8: RTC module connected alongside LCD — screen displaying live time: 12:05:25</em></p>

**Observations:** Once both the LCD and RTC were connected on the same I2C bus the scanner correctly detected two separate devices. The RTC started displaying the live time on the LCD and it updated every second as expected. The RTC module has its own coin cell battery which allows it to keep accurate time even when the Arduino is powered off.

**What went wrong:** After connecting the RTC the LCD stopped showing anything for a moment. It turned out the RTC library needed to be told to set the compile time on first upload. We also had the SDA and SCL wires swapped at one point which caused the scanner to find no devices at all.

---

## Sub-circuit 4 – Push Button

The push buttons were connected to digital input pins of the Arduino using the built-in pull-up resistors, declared in the setup function with `pinMode(pin, INPUT_PULLUP)`. The `Button.h` library was used to handle debouncing automatically.

**Observations:** The pull-up resistor approach worked cleanly — without it the pin would float between HIGH and LOW randomly when the button was not pressed, causing false triggers. We checked which pairs of legs were connected on the button using the multimeter continuity mode before wiring.

**What went wrong:** The button registered multiple presses for a single physical press at first. The `Button.h` library handled this automatically once we switched to using it instead of raw `digitalRead()`.

---

## Final Task – Alarm Clock

All four sub-circuits were combined into a single functional alarm clock. The system shows the current time on the LCD from the RTC, allows the alarm time to be set using push buttons, triggers the buzzer when the alarm time is reached, and lets the user turn the alarm off — all without changing the code.

<p align="center">
  <img src="sub4_alarm.jpg" width="500">
</p>
<p align="center"><em>Fig 9: Complete alarm clock — LCD showing "Time: 12:44:56" and "Alarm is ON"</em></p>

**Observations:** The final alarm clock worked as intended. The LCD displayed the current time on the first line and the alarm status on the second line. Multiple coloured push buttons were used — one to increment the alarm hour, one for the minutes, and one to dismiss the alarm when it triggered. The buzzer produced a repeating alert tone when the set alarm time was reached.

**What went wrong:** When all components were connected together for the first time the LCD showed garbled random characters. After debugging we found a loose jumper wire on the breadboard that was causing noise on the I2C line. Re-seating the connection fixed the display. There was also an issue where the alarm would trigger immediately after power-on — this was fixed by adding a short startup delay in the code.

---

## Components Used

- Arduino Uno
- 16x2 I2C LCD Display — address `0x27`
- DS1307 RTC Module — address `0x68`
- Passive buzzer + 220 Ω resistor
- Push buttons x4 (coloured)
- Breadboard + jumper wires
- Libraries: `LiquidCrystal_I2C.h`, `RTClib.h`, `Button.h`
