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
  <img src="Exercise_2/sub1_buzzer.jpg" width="500">
</p>
<p align="center"><em>Fig 1: Buzzer connected to Arduino Uno via 220 Ω resistor on breadboard</em></p>

https://github.com/usamaasif24/digital-design-fabrication/raw/main/Exercise_2/video_buzzer.mp4

<p align="center"><em>Video 1: Buzzer producing different tones by changing delay values in code</em></p>

**Observations:** When the code was first uploaded the buzzer produced a continuous beeping tone. By reducing the delay between HIGH and LOW states the pitch of the sound increased noticeably — shorter delays produced a higher and more rapid tone, while longer delays produced slower and deeper beeps. Changing the sequence of HIGH/LOW patterns also changed the rhythm of the sound, which showed how digital output pins can directly control audio behaviour.

**What went wrong:** The buzzer did not make any sound at first. After checking the wiring we found that the positive leg of the buzzer was connected to the wrong pin. Moving it to pin 12 as specified in the code fixed it immediately. We also noticed that without the resistor the buzzer was very loud — the resistor helped reduce the current and bring it to a more reasonable volume.

---

## Sub-circuit 2 – Connecting the LCD Screen

The LCD is a 2x16 character display that communicates over the I2C protocol using only 4 wires — VCC, GND, SDA (A4) and SCL (A5). The `I2C_scanner.ino` sketch was run first to find the LCD address, which came back as `0x27`. The `LCD_test.ino` sketch was then used to print text to the screen.

<p align="center">
  <img src="Exercise_2/sub2_lcd.jpg" width="500">
</p>
<p align="center"><em>Fig 2: Arduino Uno connected to I2C LCD screen displaying "Hello! LCD Working"</em></p>

https://github.com/usamaasif24/digital-design-fabrication/raw/main/Exercise_2/video_lcd.mp4

<p align="center"><em>Video 2: LCD screen displaying test message after successful I2C configuration</em></p>

**Observations:** Once the correct I2C address was set in the code the display responded immediately and showed the test message "Hello! LCD Working" across two lines. The I2C protocol made the wiring simple — only 4 wires were needed compared to the 8+ wires a parallel LCD connection would require. The display was clear and readable with good contrast.

**What went wrong:** The LCD showed nothing when the code was first uploaded. Running the I2C scanner revealed the address was `0x27` and not the `0x20` we had initially assumed and put in the code. Updating the address in the sketch fixed the display straight away. This showed why running the I2C scanner first is important before assuming the device address.

---

## Sub-circuit 3 – Real Time Clock (RTC)

The RTC module was added to the existing I2C bus alongside the LCD, sharing the same SDA and SCL lines. Running the I2C scanner with both devices connected now reported two addresses — `0x27` for the LCD and `0x68` for the RTC. The `RTC_LCD_test.ino` sketch was used to read the current time from the RTC and display it live on the LCD.

<p align="center">
  <img src="Exercise_2/sub3_rtc.jpg" width="500">
</p>
<p align="center"><em>Fig 3: RTC module connected alongside LCD — screen displaying live time: 12:05:25</em></p>

**Observations:** Once both the LCD and RTC were connected on the same I2C bus the scanner correctly detected two separate devices. The RTC started displaying the live time on the LCD and it updated every second as expected. The RTC module has its own coin cell battery which allows it to keep accurate time even when the Arduino is powered off — when we powered it back on the time was still correct and had continued counting.

**What went wrong:** After connecting the RTC the LCD stopped showing anything for a moment. We thought something had broken but it turned out the RTC library needed to be told to set the compile time on first upload. After doing that the time appeared correctly on screen. We also had to be careful with the I2C wiring — at one point we had the SDA and SCL wires swapped which caused the scanner to find no devices at all.

---

## Sub-circuit 4 – Push Button

The push buttons were connected to digital input pins of the Arduino using the built-in pull-up resistors, declared in the setup function with `pinMode(pin, INPUT_PULLUP)`. With this setup the pin reads HIGH when the button is not pressed and LOW when pressed. The `Button.h` library was used to handle debouncing automatically.

**Observations:** The pull-up resistor approach worked cleanly — without it the pin would float between HIGH and LOW randomly when the button was not pressed, causing false triggers. With `INPUT_PULLUP` the reading was stable and reliable. We checked which pairs of legs were connected on the button using the multimeter continuity mode before wiring, as the manual recommended.

**What went wrong:** The button registered multiple presses for a single physical press at first — this is called bouncing and is a common issue with mechanical switches. The `Button.h` library handled this automatically once we switched to using it instead of raw `digitalRead()`.

---

## Final Task – Alarm Clock

All four sub-circuits were combined into a single functional alarm clock. The system shows the current time on the LCD from the RTC, allows the alarm time to be set using push buttons, triggers the buzzer when the alarm time is reached, and lets the user turn the alarm off — all without changing the code.

<p align="center">
  <img src="Exercise_2/sub4_alarm.jpg" width="500">
</p>
<p align="center"><em>Fig 4: Complete alarm clock — LCD showing "Time: 12:44:56" and "Alarm is ON"</em></p>

**Observations:** The final alarm clock worked as intended. The LCD displayed the current time on the first line and the alarm status on the second line. Multiple coloured push buttons were used — one to increment the alarm hour, one for the minutes, and one to dismiss the alarm when it triggered. The buzzer produced a repeating alert tone when the set alarm time was reached. The RTC kept accurate time throughout and the alarm triggered reliably at the correct moment.

**What went wrong:** When all components were connected together for the first time the LCD showed garbled random characters. After debugging we found a loose jumper wire on the breadboard that was causing noise on the I2C line. Re-seating the connection fixed the display. There was also an issue where the alarm would sometimes trigger immediately after power-on before the time was properly initialised — this was fixed by adding a short startup delay in the code before the alarm check begins.

---

## Components Used

- Arduino Uno
- 16x2 I2C LCD Display — address `0x27`
- DS1307 RTC Module — address `0x68`
- Passive buzzer + 220 Ω resistor
- Push buttons x4 (coloured)
- Breadboard + jumper wires
- Libraries: `LiquidCrystal_I2C.h`, `RTClib.h`, `Button.h`
