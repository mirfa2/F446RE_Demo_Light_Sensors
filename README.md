# STM32 Zero-CPU Overhead Data Acquisition & Control

A high-efficiency, non-blocking embedded system demonstrating continuous multi-channel sensor data acquisition and dynamic actuator control. This project utilizes hardware-level peripheral linking (Timers, ADC, UART, and DMA) on the STM32F446RE to handle the entire data pipeline autonomously, leaving the main CPU core completely free for higher-level application logic.



🚀 Project Overview

This demo features two primary independent hardware streams:

1.  Continuous Actuator Control: A breathing LED fading in a perfect sine wave.

2.  Multi-Channel Sensor Acquisition: Dual photoresistors sampling differential light data at a strict, timer-controlled frequency and streaming the formatted data to a PC.

By aggressively utilizing Direct Memory Access (DMA) and hardware timers, the system completely avoids blocking delays (HAL_Delay) and high-frequency interrupt overhead.

<img width="1884" height="2741" alt="20260526_232439" src="https://github.com/user-attachments/assets/521c9fea-1683-4c0c-8fbe-de7ea305b5fd" />


⚙️ Hardware Setup

  -  Microcontroller: STM32 Nucleo-F446RE

  -  Sensors: 2x Photoresistors (LDRs) configured in voltage dividers with $10\text{k}\Omega$ resistors

  -  Actuator: 2x Standard LED with $220\Omega$ current-limiting resistors

Pin Mapping:

  - PA8: Timer 1 PWM Output to LED1 (Blue)
  
  - PA7: Timer 1 Complementary Output to LED2 (Red)

  - PA0 & PA1: ADC1 Channel 0 and 1 inputs from the photoresistor voltage dividers

  - PA2 / PA3: USART2 TX/RX (Connected to ST-LINK Virtual COM Port)



🧠 Software Architecture

This project is built using C and the STM32 HAL drivers within STM32CubeIDE. The architecture solves common real-time embedded bottlenecks through three key implementations:

1. Hardware-Driven PWM Sine Wave (Look-Up Table)
   
Calculating trigonometry in real-time is computationally expensive. Instead, a pre-computed 512-point Look-Up Table (LUT) is stored in memory. A hardware timer triggers a DMA stream in Circular Mode to continuously feed these values into the PWM Capture/Compare Register (CCR). The result is a mathematically smooth breathing LED that requires zero CPU cycles to maintain.

2. Timer-Triggered ADC with Ping-Pong Buffering

To prevent the ADC from overwhelming the data pipeline in free-running mode, a dedicated hardware timer triggers conversions at a strict $100\text{Hz}$.
  - Data is written via DMA into a 40-element batch buffer (holding 20 samples per channel).
  - The system uses Half-Transfer and Full-Transfer DMA callbacks to implement a Ping-Pong buffer scheme.
  - This ensures the CPU can safely format and transmit the first half of the data while the ADC hardware seamlessly fills the second half, entirely eliminating race conditions and data corruption.

3. Non-Blocking UART Transmission
   
Once the ADC buffer triggers a callback, the CPU rapidly formats the raw 12-bit integer pairs into an ASCII comma-separated string. The transmission is then handed off to the UART DMA controller, preventing the CPU from waiting on the relatively slow baud rate of the serial connection.



📊 Data Visualization

The STM32 transmits data in the following comma-separated ASCII format at 115200 baud:

[Sensor 1 Value], [Sensor 2 Value] \r\n

TeraTerm: Connect to the assigned COM port to view the raw data stream.

STM32CubeMonitor / Node-RED: The system is optimized for real-time visualization. You can route the serial string into a CSV parsing node to plot the dynamic response of both sensors simultaneously on a digital dashboard.
