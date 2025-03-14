/**
  @page ADC_TripleModeInterleaved  Use ADC1, ADC2 and ADC3 in Triple interleaved mode and DMA mode2 with 6Msps

  @verbatim
  ******************************************************************************
  * @file    ADC/ADC_TripleModeInterleaved/readme.txt 
  * @author  MCD Application Team
  * @brief   Description of the Triple interleaved mode and DMA mode2 Example
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2016 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  @endverbatim

@par Example Description 

How to use the ADC peripheral to convert a regular channel in Triple 
interleaved mode.

The ADC1, ADC2 and ADC3 are configured to convert ADC_CHANNEL_12, with conversion 
triggered by software.

The Triple interleaved delay is configured to 5 ADC clk cycles (ADC_TWOSAMPLINGDELAY_5CYCLES).

In Triple ADC mode, three DMA requests are generated: 
- On the first request, both ADC2 and ADC1 data are transferred (ADC2 data take 
  the upper half-word and ADC1 data take the lower half-word). 
- On the second request, both ADC1 and ADC3 data are transferred (ADC1 data take
  the upper half-word and ADC3 data take the lower half-word).
- On the third request, both ADC3 and ADC2 data are transferred (ADC3 data take 
  the upper half-word and ADC2 data take the lower half-word) and so on.

On each DMA request (two data items are available) two half-words representing 
two ADC-converted data items are transferred as a word.

A DMA request is generated each time 2 data items are available :
1st request: ADC_CDR[31:0] = (ADC2_DR[15:0] << 16) | ADC1_DR[15:0] (step1)
2nd request: ADC_CDR[31:0] = (ADC1_DR[15:0] << 16) | ADC3_DR[15:0] (step2)
3rd request: ADC_CDR[31:0] = (ADC3_DR[15:0] << 16) | ADC2_DR[15:0] (step3)
4th request: ADC_CDR[31:0] = (ADC2_DR[15:0] << 16) | ADC1_DR[15:0] (step1) and so on.

The ADC1, ADC2 and ADC3 are configured to convert ADC Channel 12.

In this example, the system clock is 216MHz, APB2 = 108MHz and ADC clock = APB2/4. 
Since ADCCLK = 27 MHz and Conversion rate = 5 cycles
==> Conversion Time = 27M/5cyc = 5.4Msps.

The ADC measure is realized on PC.02, so you need to connect this pin to a power supply (do not forget to connect the power supply 
GND to the EVAL board GND).

STM32 board's LEDs can be used to monitor the transfer status:
 - LED1 is ON when the conversion is complete.
 - LED3 is ON when there are an error in initialization. 

User should monitor aADCTripleConvertedValue variable to get the converted value.
  
@note Refer to "simulation.xls" file to have the diagram simulation of the example and to get the formula
      with an example showing how to calculate to calculate ADC sampling rate.

@note Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.
      
@note The application need to ensure that the SysTick time base is always set to 1 millisecond
      to have correct HAL operation.

@par Keywords

Analog, ADC, Analog to Digital, Triple mode, Interleaved, Continuous conversion, Software Trigger, DMA, Measurement,

@Note�If the user code size exceeds the DTCM-RAM size or starts from internal cacheable memories (SRAM1 and SRAM2),that is shared between several processors,
 �����then it is highly recommended to enable the CPU cache and maintain its coherence at application level.
������The address and the size of cacheable buffers (shared between CPU and other masters)  must be properly updated to be aligned to cache line size (32 bytes).

@Note It is recommended to enable the cache and maintain its coherence, but depending on the use case
����� It is also possible to configure the MPU as "Write through", to guarantee the write access coherence.
������In that case, the MPU must be configured as Cacheable/Bufferable/Not Shareable.
������Even though the user must manage the cache coherence for read accesses.
������Please refer to the AN4838 �Managing memory protection unit (MPU) in STM32 MCUs�
������Please refer to the AN4839 �Level 1 cache on STM32F7 Series�

@par Directory contents 

  - ADC/ADC_TripleModeInterleaved/Inc/stm32f7xx_hal_conf.h    HAL configuration file
  - ADC/ADC_TripleModeInterleaved/Inc/stm32f7xx_it.h          DMA interrupt handlers header file
  - ADC/ADC_TripleModeInterleaved/Inc/main.h                  Header for main.c module  
  - ADC/ADC_TripleModeInterleaved/Src/stm32f7xx_it.c          DMA interrupt handlers
  - ADC/ADC_TripleModeInterleaved/Src/main.c                  Main program
  - ADC/ADC_TripleModeInterleaved/Src/stm32f7xx_hal_msp.c     HAL MSP file
  - ADC/ADC_TripleModeInterleaved/Src/system_stm32f7xx.c      STM32F7xx system source file

@par Hardware and Software environment 

  - This example runs on STM32F756xx/STM32F746xx devices.

  - This example has been tested with STM327x6G-EVAL revB evaluation board and can be
    easily tailored to any other supported device and development board. 
    
  - STM327x6G-EVAL revB Set-up   
        - Connect PC2 to external power supply.
        - To use LED1, ensure that JP24 is in position 2-3
        - To use LED3, ensure that JP23 is in position 2-3

@par How to use it ? 

In order to make the program work, you must do the following :
 - Open your preferred toolchain 
 - Rebuild all files and load your image into target memory
 - Run the example


 */
