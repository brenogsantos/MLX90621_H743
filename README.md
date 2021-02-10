# MLX90621_H743

# IR 16x4 MLX90621 + ST NUCLEO-H743
### stm32h743ZIT6 / H743ZI2



# Quick Links
- [MLX90621 Datasheet](https://www.mouser.com/datasheet/2/734/MLX90621-Datasheet-Melexis-961580.pdf)
- [NUCLEO-H743ZI2 Pinout](https://os.mbed.com/platforms/ST-Nucleo-H743ZI/)


# Conteúdo
- [I2C](#i2c)
- [Code](#code)
- [Issues](#issues)



# I2C 

- ***IMPORTANT***
  
  The firmware used in the project was based on the HAL library. I was not able to use the existing functions because apparently the HAL I2C sending functions do not support 4-byte messages. I had to add 2 functions (in [stm32h7xx_hal_i2c.c](https://github.com/brenogsantos/MLX90621_H743/blob/main/Drivers/STM32H7xx_HAL_Driver/Src/stm32h7xx_hal_i2c.c)) and add a 32-bit memory space identifier (in [stm32h7xx_hal_i2c.h](https://github.com/brenogsantos/MLX90621_H743/blob/main/Drivers/STM32H7xx_HAL_Driver/Inc/stm32h7xx_hal_i2c.h)). 

  Function and identifier --> [.txt](https://github.com/brenogsantos/MLX90621_H743/blob/main/Docs/HAL_i2c_h7.txt)

  So every time you generate the STM code you need to add the following:

```
#define I2C_MEMADD_SIZE_32BIT			(0x00000004U)

static HAL_StatusTypeDef I2C_RequestMemoryRead2(I2C_HandleTypeDef *hi2c, uint16_t DevAddress, uint32_t MemAddress, uint16_t MemAddSize, uint32_t Timeout, uint32_t Tickstart)
HAL_StatusTypeDef HAL_I2C_Mem_Read2(I2C_HandleTypeDef *hi2c, uint16_t DevAddress, uint32_t MemAddress, uint16_t MemAddSize, uint8_t *pData, uint16_t Size, uint32_t Timeout)

```
- **Pull up resistors / Capacitor**

  The MCU's I2C was set to standard mode (100kHz), and the pull-up resistors and the SDA-GND capacitor were calculated based on the Texas Instruments I2C [document](https://www.ti.com/lit/an/slva689/slva689.pdf?ts=1612877731180&ref_url=https%253A%252F%252Fwww.google.com%252F).

![alt Text](https://github.com/brenogsantos/MLX90621_H743/blob/main/images/Captura%20de%20tela%202021-02-09%20233455.png)


# Code
  - **Quick Note**
  
I’m still updating and improving the code, in fact, part of the code related to the calculation of To came from a project I found using the STM32L4, if you have created some of the following functions, please contact me.
  
  - **Library**
   
   I created a library (MLX90621_BG) for the sensor, where you can find quick test functions and the standard reading and writing functions. If you want to understand the code, read the sensor datasheet, it makes it a lot easier, especially the step by step on page 20.
   
  - **Output**
  
  The code output uses UART4, using asynchronous mode and a UART_print() function. The output is a 16x4 matrix containing the temperature values in celsius
  

# Issues
  
  - **Some pixels in the output matrix have no consistent value.**
  
  Some values in my output matrix were constant during the execution time, even with the sensor exposed to temperature variations. I believe it is some error related to the state of conservation of the sensor, which, by carelessness, lost 2 pins (I had to make a workaround)
  
  - **Contact me if you find other**
  
  


