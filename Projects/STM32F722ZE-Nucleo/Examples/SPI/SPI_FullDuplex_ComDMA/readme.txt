/**
  @page SPI_FullDuplex_ComDMA SPI Full Duplex DMA example

  @verbatim
  ******************************************************************************
  * @file    SPI/SPI_FullDuplex_ComDMA/readme.txt 
  * @author  MCD Application Team
  * @brief   Description of the SPI Full Duplex DMA example.
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

Data buffer transmission/reception between two boards via SPI using DMA.

   _________________________                        _________________________
  |           ______________|                      |______________           |
  |          |SPI1          |                      |          SPI1    |      |
  |          |              |                      |                  |      |
  |          |     CLK(PB3) |______________________|(PB3)CLK  CN7.15  |      |
  |          |              |                      |                  |      |
  |          |     MISO(PA6)|______________________|(PA6)MISO CN7.D12 |      |
  |          |              |                      |                  |      |
  |          |     MOSI(PA7)|______________________|(PA7)MOSI CN7.D11 |      |
  |          |              |                      |                  |      |
  |          |______________|                      |__________________|      |
  |      __                 |                      |                         |
  |     |__|                |                      |                         |
  |     USER                |                      |                         |
  |                      GND|______________________|GND                      |
  |                         |                      |                         |
  |_STM32F7 Master _________|                      |_STM32F7 Slave __________|
  
HAL architecture allows user to easily change code to move to Polling or IT 
mode. To see others communication modes please check following examples:
SPI\SPI_FullDuplex_ComPolling

At the beginning of the main program the HAL_Init() function is called to reset 
all the peripherals, initialize the Flash interface and the systick.
Then the SystemClock_Config() function is used to configure the system
clock (SYSCLK) to run at 216 MHz.

The SPI peripheral configuration is ensured by the HAL_SPI_Init() function.
This later is calling the HAL_SPI_MspInit()function which core is implementing
the configuration of the needed SPI resources according to the used hardware (CLOCK, 
GPIO, DMA and NVIC). You may update this function to change SPI configuration.

The SPI communication is then initiated.
The HAL_SPI_TransmitReceive_DMA() function allows the reception and the 
transmission of a predefined data buffer at the same time (Full Duplex Mode). 
The user can choose between Master and Slave through "#define MASTER_BOARD"
in the "main.c" file.
If the Master board is used, the "#define MASTER_BOARD" must be uncommented.
If the Slave board is used the "#define MASTER_BOARD" must be commented.

For this example the aTxBuffer is predefined and the aRxBuffer size is same as aTxBuffer.

In a first step after the user press the User push-button, SPI Master starts the
communication by sending aTxBuffer and receiving aRxBuffer through 
HAL_SPI_TransmitReceive_DMA(), at the same time SPI Slave transmits aTxBuffer 
and receives aRxBuffer through HAL_SPI_TransmitReceive_DMA(). 
The callback functions (HAL_SPI_TxRxCpltCallback and HAL_SPI_ErrorCallbackand) update 
the variable wTransferState used in the main function to check the transfer status.
Finally, aRxBuffer and aTxBuffer are compared through Buffercmp() in order to 
check buffers correctness.  

STM32F722ZE-Nucleo's LEDs can be used to monitor the transfer status:
 - LED1 toggles quickly on master board waiting User push-button to be pressed.
 - LED1 turns ON when the transmission process is complete.
 - LED2 turns ON when the reception process is complete.
 - LED3 turns ON when there is an error in transmission/reception process.  

@note SPIx instance used and associated resources can be updated in "main.h"
      file depending hardware configuration used.

@note You need to perform a reset on Slave board, then perform it on Master board
      to have the correct behaviour of this example.
      
@note Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.
      
@note The application need to ensure that the SysTick time base is always set to 1 millisecond
      to have correct HAL operation.

@par Keywords

Connectivity, SPI, Full-duplex, DMA, Transmission, Reception, Master, Slave, MISO, MOSI

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

  - SPI/SPI_FullDuplex_ComDMA/Inc/stm32f7xx_hal_conf.h   HAL configuration file
  - SPI/SPI_FullDuplex_ComDMA/Inc/stm32f7xx_it.h         Interrupt handlers header file
  - SPI/SPI_FullDuplex_ComDMA/Inc/main.h                 Header for main.c module  
  - SPI/SPI_FullDuplex_ComDMA/Src/stm32f7xx_it.c         Interrupt handlers
  - SPI/SPI_FullDuplex_ComDMA/Src/main.c                 Main program
  - SPI/SPI_FullDuplex_ComDMA/Src/system_stm32f7xx.c     stm32f7xx system source file
  - SPI/SPI_FullDuplex_ComDMA/Src/stm32f7xx_hal_msp.c    HAL MSP file

@par Hardware and Software environment

  - This example runs on STM32F722xx/STM32F723xx/STM32F732xx/STM32F733xx devices.

  - This example has been tested with STM32F722ZE-Nucleo board and can be
    easily tailored to any other supported device and development board.

  - STM32F722ZE-Nucleo Set-up
    - Connect Master board PB3 to Slave Board PB3 (CN7, pin 15)
    - Connect Master board PA6 to Slave Board PA6 (Arduino D12)
    - Connect Master board PA7 to Slave Board PA7 (Arduino D11)
    - Connect Master board GND to Slave Board GND

@par How to use it ? 

In order to make the program work, you must do the following:
 - Open your preferred toolchain 
 - Rebuild all files and load your image into target memory
    o Uncomment "#define MASTER_BOARD" and load the project in Master Board
    o Comment "#define MASTER_BOARD" and load the project in Slave Board
 - Run the example


 */
 