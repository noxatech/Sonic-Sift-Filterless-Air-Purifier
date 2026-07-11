# Noxa Corp: Sonic-Sift V1.0 🌪️
**Filterless, Acoustic Agglomeration-Based Ultrasonic Air Purification Reactor**

Sonic-Sift is an experimental system architecture designed to replace traditional HEPA filters. It utilizes high-frequency sound waves, fluid dynamics, and the principle of inertia to physically separate and isolate airborne micro-particles without the need for a physical filter barrier.

The system intakes ambient air, subjects it to 40KHz ultrasonic standing waves to force particle collisions and agglomeration, and then utilizes inertial separation to divert the clean air while dumping the heavy particulate matter into a collection bin. The entire system architecture and dynamic calibration are managed by an ESP32-S3 microcontroller.

---

## 🏗️ System Architecture (Physical Layers)

The device features a vertical cylindrical structure composed of three main modules:

1.  **Top Crown Module (Main Control & Emitter):** This 140mm diameter tower houses the system's brain (ESP32-S3) along with the BME280 and SDS011 laser dust sensors. A 40mm main air intake shaft runs through the center. Exactly 10 units of 40KHz Ultrasonic Piezo Transmitters are symmetrically arrayed around this central shaft, firing downwards.
2.  **Middle Hall (Acoustic Reaction Chamber):** A 100mm diameter transparent acrylic/polycarbonate tube. This is the main reaction tunnel where particles collide and agglomerate at the speed of sound.
3.  **Bottom Module (Reflection & Exhaust Bypass):** The foundational base of the system. It features a **35-degree passive conical funnel (reflector)** perfectly aligned to bounce the descending ultrasonic waves back upwards. Beneath the acoustic zone, two 5015 Blower Fans exhaust the purified air, while the heavy, agglomerated dust particles fall straight down into the central collection bin.

---

## 🧮 Physics & Core Equations

The core operational principle of the Sonic-Sift relies on three fundamental physical phenomena:

### 1. Acoustic Standing Waves
The 40KHz sound waves emitted downwards from the 10 piezo transmitters hit the 35-degree passive conical reflector at the bottom and bounce back. When the incident and reflected waves meet inside the reaction tube, they create **Standing Waves**.

The ambient temperature ($T$, in Celsius) is constantly monitored via the BME280 sensor to dynamically calculate the speed of sound ($c$):
$$c \approx 331.3 \sqrt{1 + \frac{T}{273.15}}$$

Based on this real-time variable, the ESP32-S3 calculates the required wavelength ($\lambda$) to maintain resonance:
$$\lambda = \frac{c}{f}$$
Where $f = 40,000 \text{ Hz}$. Suspended particles are forced to gather at the nodes (points of zero acoustic pressure) of the standing wave. To achieve perfect resonance, the acoustic path length ($L$) must be an integer multiple of half the wavelength:
$$L = n \frac{\lambda}{2}$$

### 2. Acoustic Agglomeration
Micro-particles (PM2.5, PM10) suspended in the air gain kinetic energy in the intense 40KHz acoustic field. Due to the orthokinetic effect, particles of different masses vibrate with different amplitudes, causing them to collide, adhere to one another, and grow in mass.

The entrainment coefficient ($\mu_p$), which represents the ratio of the particle vibration velocity to the gas vibration velocity, is expressed as:
$$\mu_p = \frac{1}{\sqrt{1 + (\omega \tau)^2}}$$
*(Where $\omega$ is the angular frequency and $\tau$ is the particle relaxation time).* As the particles agglomerate, their increased mass makes them highly susceptible to gravity and downward airflow, initiating precipitation.

### 3. Inertial Separation
As the heavy, agglomerated dust clusters travel downwards at high speed, the airflow is abruptly forced to make a 90-degree turn towards the exhaust bypass slots (leading to the blower fans) at the bottom of the system.

The probability of particles separating from the airflow streamline and dropping into the collection bin is governed by the Stokes Number ($Stk$):
$$Stk = \frac{\rho_p d_p^2 U}{18 \mu L_c}$$
Agglomerated heavy dust clusters with $Stk \gg 1$ possess too much inertia to negotiate the sharp aerodynamic turn, causing them to shoot straight down into the dead zone (dust collection bin).

---

## 🌬️ Aerodynamics & Fluid Dynamics Design

The hardware chassis is strictly modeled according to fluid dynamics principles to eliminate internal air resistance, turbulence, and back-pressure.

### Top Module: Parabolic Funnel (Coanda Effect)
The outer surface of the top control module is not flat; it features a curved, inward-sloping **parabolic funnel** (wormhole geometry) inspired by advanced impeller intakes.
* **Purpose:** To direct the dirty ambient air straight into the 40mm central shaft while actively preventing Boundary Layer Separation. This funnel accelerates the ambient air into the intake via the Venturi effect.

### Bottom Module: NACA Airfoil Exhaust Bypass Geometry
Ensuring that the clean air exiting the acoustic storm does not create vortices is critical for system efficiency and silent operation.
* **Design:** The 50x15mm exhaust slots leading from the lower chassis to the 5015 Blower Fans are not standard rectangular cuts. The inner walls of these channels utilize a teardrop-shaped **NACA Airfoil** geometry.
* **Purpose:** As the air mass negotiates the 90-degree turn to be expelled, the Drag Coefficient is minimized, and the aerodynamic back-pressure on the blower fans is entirely neutralized.

***

**Noxa Corp - Push The Boundaries.**
