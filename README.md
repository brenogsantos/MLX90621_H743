# MLX90621_H743

# IR 16x4 MLX90621 + ST NUCLEO-H743
### stm32h743ZIT6 / H743ZI2



# Quick Links
- [MLX90621 Datasheet](https://www.mouser.com/datasheet/2/734/MLX90621-Datasheet-Melexis-961580.pdf)
- [NUCLEO-H743ZI2 Pinout](https://os.mbed.com/platforms/ST-Nucleo-H743ZI/)


# Conteúdo
- [I2C](#i2c)
- [



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



#
