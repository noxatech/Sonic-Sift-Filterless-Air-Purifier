# Noxa Corp: Sonic-Sift V1.0 🌪️
**Filterless Acoustic Agglomeration-Based Ultrasonic Air Purification Reactor**

**Status:** R&D Phase / Minimum Viable Product (MVP) Development  
**Lead Architect:** Alp Arslan Sayin | Noxa Corp (Nucleus Of Xperimental Applications)

## 📑 Abstract
Sonic-Sift is a proprietary, experimental system architecture designed to fundamentally disrupt traditional HEPA-based air filtration. Instead of forcing air through a physical mesh that eventually clogs, Sonic-Sift utilizes high-frequency acoustic standing waves, fluid dynamics, and the principle of inertia to physically separate and isolate airborne micro-particles (PM2.5, PM10). 

The system creates an "acoustic storm" that forces sub-micron particles to collide and agglomerate at the speed of sound. Once heavy, these clusters are separated from the clean air stream via sharp aerodynamic bypass routes. The entire reactor is dynamically calibrated and managed by an ESP32-S3 neural node.

---

## ⚙️ Hardware Bill of Materials (BOM)
To achieve industrial-grade acoustic pressure and precise environmental monitoring, the system integrates the following core hardware components:

| Component Category | Part / Specification | Function within Reactor |
| :--- | :--- | :--- |
| **Main Processing Unit** | ESP32-S3 (N16R8) Dual-Core | System brain, real-time wave calibration, and UI management. |
| **Acoustic Array** | 10x 40KHz Ultrasonic Piezos | Generates the high-pressure acoustic standing waves. |
| **Particulate Sensor** | SDS011 Laser Dust Sensor | High-precision laser scattering to detect PM2.5 and PM10 levels. |
| **Environmental Sensor** | BME280 | Reads ambient Temperature and Humidity for Speed of Sound ($c$) calibration. |
| **Power Distribution** | LM2596 & XL6009 DC-DC | Steps down/up the 12V 5A main input to strictly regulated voltages. |
| **Signal Amplification** | TC4427 MOSFET Driver | Amplifies the ESP32's PWM signals to safely drive the 10-piezo array. |
| **Airflow Mechanics** | 2x 5015 Blower Fans (Snail) | Pulls the purified air through the aerodynamic bypass slots. |
| **Human-Machine Interface**| 1.28" Round LCD + EC11 Rotary | Displays real-time reactor telemetry; Rotary allows menu navigation. |

---

## 🏗️ System Architecture (Physical Layers)

The device features a highly specialized, 3-stage vertical cylindrical structure:

1.  **Top Crown Module (The Brain & Emitter):** A 140mm diameter command tower that houses the ESP32-S3, BME280, and the SDS011 laser sensor. A 40mm main air intake shaft runs through the absolute center. Exactly 10 units of 40KHz Ultrasonic Transmitters are symmetrically arrayed in a tight ring around this shaft, secured by 1.5mm custom retaining hooks, firing sound waves straight down into the reactor.
2.  **Middle Hall (Acoustic Reaction Chamber):** A 100mm diameter transparent acrylic/polycarbonate tube. This is the isolated reaction tunnel where the orthokinetic effect takes place.
3.  **Bottom Module (Reflection & Exhaust Bypass):** The foundational base of the system. It features a **35-degree passive conical funnel (reflector)** perfectly aligned to bounce the descending ultrasonic waves back upwards to create standing waves. Beneath the acoustic zone, two 5015 Blower Fans exhaust the clean air, while the heavy dust falls straight down into a removable electronic maintenance floor and collection bin.

---

## 💻 Software Architecture (Asymmetric Dual-Core Processing)
Generating a flawless, uninterrupted 40KHz PWM signal while simultaneously parsing laser sensor data and rendering UI graphics requires strict hardware-level multitasking. Sonic-Sift utilizes FreeRTOS to divide operations asymmetrically across the ESP32-S3's dual cores:

* **Core 0 (Environmental & UI Node):** Handles the I2C communications for the BME280, serial UART polling for the SDS011, and SPI rendering for the 1.28" LCD screen.
* **Core 1 (Acoustic Engine):** Exclusively dedicated to generating the 40KHz high-resolution hardware timer (PWM) signal. It runs without OS interruption to prevent acoustic jitter and phase shifts in the standing waves.

---

## 🧮 Physics & Core Equations

The reactor's operation is governed by three primary physical phenomena:

### 1. Dynamic Acoustic Standing Waves
The 40KHz sound waves emitted downwards hit the 35-degree passive conical reflector and bounce back. To ensure particle entrapment, **Standing Waves** must be formed and maintained despite environmental changes.

The ambient temperature ($T$, in Celsius) is polled via the BME280 to dynamically calculate the true speed of sound ($c$):
$$c \approx 331.3 \sqrt{1 + \frac{T}{273.15}}$$

Based on this real-time variable, the ESP32-S3 calculates the required wavelength ($\lambda$) to maintain resonance:
$$\lambda = \frac{c}{f}$$
*(Where $f = 40,000 \text{ Hz}$).* Suspended particles are forced by acoustic radiation pressure to gather at the nodes (points of zero pressure). For perfect resonance, the acoustic path length ($L$) matches:
$$L = n \frac{\lambda}{2}$$

### 2. Acoustic Agglomeration (Orthokinetic Effect)
Micro-particles suspended in the chamber gain immense kinetic energy. Because particles of different masses vibrate with varying amplitudes in the sound field, they collide and fuse (agglomerate).

The entrainment coefficient ($\mu_p$), representing the ratio of particle vibration velocity to gas vibration velocity, is:
$$\mu_p = \frac{1}{\sqrt{1 + (\omega \tau)^2}}$$
*(Where $\omega$ is the angular frequency and $\tau$ is the particle relaxation time).* As $\mu_p$ shifts, collisions exponentially increase the mass of the dust clusters.

### 3. Inertial Separation (Impaction)
As the agglomerated dust clusters travel downwards, the airflow is abruptly forced to make a sharp 90-degree turn into the exhaust bypass slots.

The probability of particles separating from the airflow and dropping into the collection bin is governed by the Stokes Number ($Stk$):
$$Stk = \frac{\rho_p d_p^2 U}{18 \mu L_c}$$
Clusters with $Stk \gg 1$ possess too much inertia to negotiate the sharp aerodynamic turn, shooting straight down into the dead zone while clean air escapes sideways.

---

## 🌬️ Fluid Dynamics & Aerodynamic Chassis Design

The physical 3D casing is strictly modeled to eliminate internal air resistance, turbulence, and back-pressure.

### Coanda Effect Intake (Parabolic Funnel)
The top outer surface features a curved, inward-sloping **parabolic funnel** (wormhole geometry). 
* **Purpose:** To direct dirty ambient air straight into the 40mm central shaft while actively preventing Boundary Layer Separation. It accelerates the ambient air into the intake via the Venturi effect, reducing the load on the exhaust fans.

### NACA Airfoil Exhaust Bypass
Ensuring the clean air exits the acoustic storm without creating vortices is critical for system efficiency.
* **Design:** The 50x15mm exhaust slots leading from the lower chassis to the Snail Fans utilize a teardrop-shaped **NACA Airfoil** internal geometry.
* **Purpose:** As the air mass negotiates the 90-degree turn, the Drag Coefficient ($C_d$) is minimized, completely neutralizing the aerodynamic back-pressure on the 5015 blowers.

***
**Developed by Noxa Corp - Push The Boundaries.**
