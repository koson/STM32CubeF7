/**
  @page FatFs_MultiDrives   FatFs with multi drives application
 
  @verbatim
  ******************** (C) COPYRIGHT 2016 STMicroelectronics *******************
  * @file    FatFs/FatFs_MultiDrives/readme.txt 
  * @author  MCD Application Team
  * @brief   Description of the FatFs with multi drives application
  ******************************************************************************
  *
  * @attention
  *
  * Copyright (c) 2017 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  @endverbatim

@par Application Description

How to use STM32Cube firmware with FatFs middleware component as a generic
FAT file system module. This example develops an application that exploits
FatFs features, with multidrive (USB Disk, uSD) configurations.

At the beginning of the main program the HAL_Init() function is called to reset 
all the peripherals, initialize the Flash interface and the systick.
Then the SystemClock_Config() function is used to configure the system clock
(SYSCLK) to run at 216 MHz and provide 50 MHz at the output PLL divided by PLL_Q. 
This frequency permit to reach 25 MHz clock needed for SD operation and in line 
with microSD specification. 
 
The application is based on writing and reading back a text file from two drives,
and it's performed using FatFs APIs to access the FAT volume as described
in the following steps:

 - Link the SDRAM and uSD disk I/O drivers;
 - Register the file system object (mount) to the FatFs module for both drives;
 - Create a FAT file system (format) on both logical drives;
 - Create and Open new text file objects with write access;
 - Write data to the text files;
 - Close the open text files;
 - Open text file objects with read access;
 - Read back data from the text files;
 - Check on read data from text file;
 - Close the open text files;
 - Unlink the SDRAM and uSD disk I/O drivers.

It is worth noting that the application manages any error occurred during the 
access to FAT volume, when using FatFs APIs. Otherwise, user can check if the
written text file is available on the uSD card.

It is possible to fine tune needed FatFs features by modifying defines values 
in FatFs configuration file �ffconf.h� available under the project includes 
directory, in a way to fit the application requirements. In this case, two drives
are used, so the Max drive number is set to: _VOLUMES = 2 in �ffconf.h� file.
         
STM32 Eval board's LEDs can be used to monitor the application status:
  - LED1 is ON when the application runs successfully.
  - LED3 is ON when any error occurs. 


@note Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.
      
@note The application needs to ensure that the SysTick time base is always set to 1 millisecond
      to have correct HAL operation.

For more details about FatFs implementation on STM32Cube, please refer to UM1721 "Developing Applications 
on STM32Cube with FatFs".


@par Keywords

FatFS, RAMDisk, SDRAM, SD Card, FAT, File system 

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
 
  - FatFs/FatFs_MultiDrives/Inc/stm32f7xx_hal_conf.h    HAL configuration file
  - FatFs/FatFs_MultiDrives/Inc/stm32f7xx_it.h          Interrupt handlers header file
  - FatFs/FatFs_MultiDrives/Inc/main.h                  Main program header file
  - FatFs/FatFs_MultiDrives/Inc/ffconf.h                FAT file system module configuration file   
  - FatFs/FatFs_MultiDrives/Src/stm32f7xx_it.c          Interrupt handlers
  - FatFs/FatFs_MultiDrives/Src/main.c                  Main program
  - FatFs/FatFs_MultiDrives/Src/system_stm32f7xx.c      STM32F7xx system clock configuration file
         
 
@par Hardware and Software environment

  - This application runs on STM32F756xx/STM32F746xx devices.
    
  - This application has been tested with STMicroelectronics STM327x6G_EVAL 
    evaluation boards and can be easily tailored to any other supported device 
    and development board.  

  - STM327x6G_EVAL Set-up
   - Connect a uSD Card to the MSD connector (CN16).
    - Ensure that JP24 is in position 2-3 to use LED1.
    - Ensure that JP23 is in position 2-3 to use LED3. 

@par How to use it ? 

In order to make the program work, you must do the following :
 - Open your preferred toolchain 
 - Rebuild all files and load your image into target memory
 - Run the application


 */
