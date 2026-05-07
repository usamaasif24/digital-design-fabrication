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

| Position | V_LED | V2 |
|----------|-------|----|
| Full brightness | 2.97 V | 2.99 V |
| Dimmed | 2.88 V | 4.55 V |
| OFF | 0.37 V | 4.58 V |

**Observations:** The potentiometer acts as a variable voltage divider. As its resistance increases, V2 rises and V_LED falls. When V_LED drops below the LED forward voltage threshold (~2 V), the LED turns off completely. The relationship is inverse and continuous — rotating the wiper provides smooth, analogue brightness control. V2 + V_LED ≈ 5 V at all positions, confirming KVL.


https://github.com/user-attachments/assets/a8bee591-fca0-44c0-987e-4fc35da2f613


<p align="center"><em>Video 1: Demonstration of the dimmable LED circuit with potentiometer.</em></p>

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

| Frequency | Observation |
|-----------|-------------|
| 5 Hz | Clearly visible slow flashing — eye resolves individual pulses |
| 25 Hz | Fast flicker, still clearly uncomfortable |
| 45 Hz | Barely perceptible — near flicker fusion threshold |
| 100 Hz | No flicker — smooth steady glow at 50% brightness |

**Observations:** At low frequencies the eye resolves individual on/off pulses as visible flicker. As frequency increases past approximately 45–50 Hz — the human visual flicker fusion threshold — the brain can no longer distinguish individual pulses and perceives a continuous steady light. At 100 Hz the strip appears completely smooth. Frequency does not affect brightness — only duty cycle does.





