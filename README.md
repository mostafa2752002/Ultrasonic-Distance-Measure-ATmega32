# Ultrasonic Distance Measurement System (ATmega32)

## 📌 Project Overview
This project implements a high-precision distance measurement system using the **HC-SR04 Ultrasonic Sensor** and the **ATmega32** microcontroller. The system calculates distance based on the time-of-flight of sound waves, utilizing the hardware **Input Capture Unit (ICU)** to ensure accurate pulse-width measurement.

The real-time distance is displayed in centimeters on a **16x4 LCD**.

---

## 🛠️ System Features
- **High Precision Timing:** Uses the ATmega32 internal ICU to capture the Echo signal edges.
- **Dynamic Sensing:** Continuous distance measurement with a range of 2cm to 400cm.
- **Interrupt-Driven:** Measurement is handled via callback functions, minimizing CPU overhead in the main loop.
- **Layered Architecture:** Clear separation between Microcontroller Abstraction (MCAL) and Hardware Abstraction (HAL).

---

## 🏗️ Software Architecture
The system is built using a **Layered Architecture** to promote modularity and ease of hardware replacement:

1.  **MCAL (Microcontroller Abstraction Layer):**
    * **GPIO Driver:** Standard digital I/O control for the Trigger pin and LCD pins.
    * **ICU Driver:** Configured at `F_CPU/8` to detect rising/falling edges and measure the "High Time" of the Echo pulse.
2.  **HAL (Hardware Abstraction Layer):**
    * **Ultrasonic Driver:** Manages the Trigger pulse and calculates distance using physics constants ($v = 340 m/s$).
    * **LCD Driver (8-bit mode):** Interface for the 16x4 display.
3.  **App Layer:**
    * **Application Logic:** Initializes drivers and updates the display with the latest sensor data.

---

## 📏 Measurement Logic
The distance is calculated using the formula:
$$Distance (cm) = \frac{Sound Speed (34000 cm/s) \times Time (sec)}{2}$$

The ICU is configured to:
1. Detect a **Rising Edge** (Start of Echo).
2. Reset the timer and switch to **Falling Edge** detection.
3. Capture the timer value on the Falling Edge to determine the total High Time.

---

## 🔌 Hardware Configuration
- **Microcontroller:** ATmega32 (**8.0 MHz Clock**)
- **Sensor:** HC-SR04 Ultrasonic Sensor
- **Display:** 16x4 LCD (8-bit Mode)
- **ICU Configuration:** Frequency = `F_CPU/8`, Initial Trigger = **Rising Edge**

### Pin Mapping
- **Ultrasonic Trigger:** (Defined in Driver, e.g., PD7)
- **Ultrasonic Echo:** PD6 (Fixed ICU Pin for ATmega32)
- **LCD Control:** RS → PB0 | E → PB1 | RW → GND
- **LCD Data:** Port A (all 8 pins)
