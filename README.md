# STM32 Zero-CPU Overhead Data Acquisition & Control

A high-efficiency, non-blocking embedded system demonstrating continuous multi-channel sensor data acquisition and dynamic actuator control. This project utilizes hardware-level peripheral linking (Timers, ADC, UART, and DMA) on the STM32F446RE to handle the entire data pipeline autonomously, leaving the main CPU core completely free for higher-level application logic.


🚀 Project Overview

This demo features two primary independent hardware streams:

1.  Continuous Actuator Control: A breathing LED fading in a perfect sine wave.

2.  Multi-Channel Sensor Acquisition: Dual photoresistors sampling differential light data at a strict, timer-controlled frequency and streaming the formatted data to a PC.

By aggressively utilizing Direct Memory Access (DMA) and hardware timers, the system completely avoids blocking delays (HAL_Delay) and high-frequency interrupt overhead.


⚙️ Hardware Setup

  -  Microcontroller: STM32 Nucleo-F446RE

  -  Sensors: 2x Photoresistors (LDRs) configured in voltage dividers with $10\text{k}\Omega$ resistors

  -  Actuator: 2x Standard LED with $220\Omega$ current-limiting resistors

Pin Mapping:

  - PA8: Timer 1 PWM Output to LED1 (Blue)
  
  - PA7: Timer 1 Complementary Output to LED2 (Red)

  - PA0 & PA1: ADC1 Channel 0 and 1 inputs from the photoresistor voltage dividers

  - PA2 / PA3: USART2 TX/RX (Connected to ST-LINK Virtual COM Port)
