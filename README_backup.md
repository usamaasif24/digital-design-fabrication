# Exercise 1: Electrical Circuits

**Digital Design & Fabrication**  
Carl von Ossietzky Universität Oldenburg · May 2026

---

## Task 1.1 – Simple LED Circuit

A green LED was connected in series with a current-limiting resistor between a 5 V supply and ground. The circuit was tested with three resistor values: 220 Ω, 1 kΩ, and 4.7 kΩ. Voltage across the resistor (V1) and across the LED (V_LED) were measured for each.

![Task 1.1 Circuit](images/task1_1_circuit.jpg)

| R1 | V1 | V_LED |
|----|----|-------|
| 220 Ω | 2.10 V | 2.76 V |
| 1000 Ω | 2.53 V | 2.45 V |
| 4700 Ω | 2.71 V | 2.29 V |

As R1 increases, the voltage drop across the resistor increases and V_LED decreases. The sum V1 + V_LED ≈ 5 V at all times, confirming Kirchhoff's Voltage Law. The LED forward voltage remains relatively stable while current — and brightness — decreases with higher resistance.

---

## Task 1.2 – Switchable LED Circuit

A toggle switch (S1) was added in series with the circuit. The switch was tested in both orientations.

![Task 1.2 Circuit](images/task1_2_switch.jpg)

When the switch is open the circuit is broken and the LED is off. When closed, the LED lights at full brightness. Reversing the switch produced identical behaviour, confirming it is a non-polarised SPST switch — orientation has no effect.

---

## Task 1.3 – Dimmable LED Circuit

A 1 kΩ potentiometer (R2) was added in series, with its wiper as the variable output. R1 was set to 100 Ω. Voltages were measured at three potentiometer positions.

![Task 1.3 Circuit](images/task1_3_potentiometer.jpg)
![Lab Notes](images/task1_3_notes.jpg)

| Position | V_LED | V2 |
|----------|-------|----|
| Full brightness | 2.97 V | 2.99 V |
| Dimmed | 2.88 V | 4.55 V |
| OFF | 0.37 V | 4.58 V |

The potentiometer acts as a variable voltage divider. As resistance increases, V2 rises and V_LED falls. When V_LED drops below the LED forward voltage threshold (~2 V), the LED turns off. The relationship is inverse and continuous, providing smooth analogue brightness control.

---

## Task 2.1 – Switchable LED Strip

An N-channel MOSFET (IRLZ44N) was used to switch a 12 V LED strip. The control side runs on 5 V from a USB module; the load side on 12 V. A 10 kΩ pull-down resistor holds the gate low when the switch is open, and a 100 Ω gate resistor limits transient current. Both sides share a common ground.

![Task 2.1 Circuit A](images/task2_1_strip_a.jpg)
![Task 2.1 Circuit B](images/task2_1_strip_b.jpg)

When S1 is open, the pull-down holds V_GS at 0 V — the MOSFET is off and the strip is dark. When S1 is closed, 5 V is applied to the gate, raising V_GS above the threshold (~2 V). The MOSFET conducts and the strip lights fully. The switch controls V_GS, not the load voltage directly. This demonstrates the key advantage of a transistor switch: a small 5 V signal controls a high-current 12 V load.

---

## Task 2.2 – Dimmable LED Strip (PWM)

The switch was replaced with a PWM signal generator powered from the USB module. Two parameters were investigated: duty cycle and switching frequency.

### Part A — Duty Cycle (f = 90 Hz)

![D = 2%](images/task2_2_duty_02.jpg)
*D = 2% — barely visible*

![D = 15%](images/task2_2_duty_15.jpg)
*D = 15% — dim*

![D = 40%](images/task2_2_duty_40.jpg)
*D = 40% — medium brightness*

![D = 60%](images/task2_2_duty_60.jpg)
*D = 60% — bright*

![D = 100%](images/task2_2_duty_100.jpg)
*D = 100% — full brightness*

| Duty Cycle | V_LSD |
|-----------|-------|
| 2% | 0.03 V |
| 15% | 2.28 V |
| 40% | 2.77 V |
| 75% | 2.90 V |
| 100% | 2.99 V |

Brightness increases proportionally with duty cycle. The MOSFET switches on and off rapidly — the eye integrates the pulses into a steady perceived intensity. Unlike the potentiometer, PWM wastes no energy as heat in a resistor, making it significantly more efficient for high-power loads.

### Part B — Switching Frequency (D = 50%)

| Frequency | Observation |
|-----------|-------------|
| 5 Hz | Clearly visible slow flashing |
| 25 Hz | Fast flicker, still uncomfortable |
| 45 Hz | Barely perceptible — near threshold |
| 100 Hz | No flicker — smooth steady glow |

At low frequencies the eye resolves individual on/off pulses as flicker. Above approximately 45–50 Hz — the human visual flicker fusion threshold — the brain averages the pulses into a continuous steady light. At 100 Hz the strip appears completely smooth at 50% brightness.
