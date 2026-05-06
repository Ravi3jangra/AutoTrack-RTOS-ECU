
# AutoTrack RTOS ECU 🚄⚙️
**Preemptive Railway Routing & Telemetry System using STM32F4 & FreeRTOS**

## 📌 Project Overview
To solve the critical safety challenges of latency and hardware-blocking in automated railway networks, I architected and developed a deterministic **Real-Time Railway ECU (Embedded Control Unit)** from scratch. 

The system is engineered to independently manage three parallel operations: 
1. High-speed train authentication (RFID)
2. Physical track routing (Servo Motors)
3. Remote telemetry (GSM SMS Alerts)

By implementing a **FreeRTOS** preemptive multitasking environment, the system completely eliminates legacy 'superloop' blocking delays, guaranteeing 100% fault-tolerance and zero-latency response to approaching trains.

## 🏗️ System Architecture
* **The Brain:** STM32F4 Series MCU (ARM Cortex-M4 @ 84MHz)
* **OS Layer:** FreeRTOS (Priority-based Task Scheduling & Context Switching)
* **Hardware Abstraction:** STM32 HAL Drivers

### Sub-Systems & Protocols
* **Authentication Engine (SPI):** Configured Full-Duplex SPI to interface with an MFRC522 RFID scanner, ensuring instant ID verification of moving trains.
* **Routing Mechanism (Hardware Timers):** Programmed STM32 hardware timers (TIM2), mathematically calculating the clock prescaler and auto-reload registers to generate precise 50Hz PWM signals for the servo-actuated track switches.
* **Telemetry Gateway (UART):** Utilized hardware USART1 (115200 baud) to push asynchronous string-based AT commands to a SIM800L module for control-room alerts.

## 🔌 Hardware Pinout & Wiring
| Component | STM32 Pin | Protocol / Function |
| :--- | :--- | :--- |
| **MFRC522 (RFID)** | PA4 (CS), PA5 (SCK), PA6 (MISO), PA7 (MOSI), PB0 (RST) | SPI1 |
| **SIM800L (GSM)** | PA9 (TX), PA10 (RX) | USART1 |
| **Servo Motor 1** | PA0 | TIM2_CH1 (PWM) |
| **Servo Motor 2** | PA1 | TIM2_CH2 (PWM) |
| **IR Sensors (1,2,3)** | PB13, PB14, PB15 | GPIO Input |
| **Signal LEDs** | PC2 (Green/GO), PC3 (Red/STOP) | GPIO Output |

> ⚠️ **Hardware Note:** The SIM800L and Servo Motors draw significant peak current. Do not power them directly from the STM32 board. Use an external buck-converter (e.g., LM2596) and ensure a common ground with the MCU.

## 🛠️ Software Toolchain
* **IDE:** STM32CubeIDE (Eclipse-based, ARM GCC Compiler)
* **Hardware Configurator:** STM32CubeMX (Pin Muxing, Clock Tree, Middleware setup)
* **Debugging:** ST-LINK GDB Hardware Debugger

## 🚀 How to Build & Run
1. Clone this repository: `git clone https://github.com/Ravi3jangra/AutoTrack-RTOS-ECU.git`
2. Open **STM32CubeIDE** and select `File > Import > Existing Projects into Workspace`.
3. Select the cloned folder and click Finish.
4. Click on the **Build (Hammer)** icon to compile the `.elf` binary.
5. Connect your STM32 Nucleo/Discovery board via USB.
6. Click **Run (Green Play Button)** to flash the firmware onto the MCU.

## 💡 Key Learnings & Skills Demonstrated
* Bare-metal to HAL Transition & Clock Tree Configuration.
* Eliminating blocking delays using RTOS Context Switching (`osDelay`).
* Concurrent handling of synchronous (SPI) and asynchronous (UART) hardware buses.

---
*Designed & Developed by [Ravi Jangra](https://linkedin.com/in/your-linkedin-profile)*
