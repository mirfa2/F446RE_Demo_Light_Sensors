# STM32 Zero-CPU Overhead Data Acquisition & Control

A high-efficiency, non-blocking embedded system demonstrating continuous multi-channel sensor data acquisition and dynamic actuator control. This project utilizes hardware-level peripheral linking (Timers, ADC, UART, and DMA) on the STM32F446RE to handle the entire data pipeline autonomously, leaving the main CPU core completely free for higher-level application logic.

🚀 Project Overview
This demo features two primary independent hardware streams:

1.  Continuous Actuator Control: A breathing LED fading in a perfect sine wave.

2.  Multi-Channel Sensor Acquisition: Dual photoresistors sampling differential light data at a strict, timer-controlled frequency and streaming the formatted data to a PC.

By aggressively utilizing Direct Memory Access (DMA) and hardware timers, the system completely avoids blocking delays (HAL_Delay) and high-frequency interrupt overhead.

⚙️ Hardware Setup
Microcontroller: STM32 Nucleo-F446RE
Sensors: 2x Photoresistors (LDRs) configured in voltage dividers with $10\text{k}\Omega$ resistors
Actuator: 1x Standard LED with a $220\Omega$ current-limiting resistor
Pin Mapping:PA5 (Example): PWM Output to LED
PA0 & PA1 (Example): ADC1 Channel 0 and 1 inputs from the photoresistor voltage dividers
PA2 / PA3: USART2 TX/RX (Connected to ST-LINK Virtual COM Port)
