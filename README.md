# stm32IAPDemo

This IAP demo put bootloader and application into one project.

## 1.How to build bootloader

uncomment '#define BOOTLOADER_APPLICATION' on main.h

```C
/* USER CODE BEGIN Private defines */

#define BOOTLOADER_APPLICATION

/* USER CODE END Private defines */
```

This will enable ```CheckUpdate``` Function on startup located at main.c

 ```C
/* USER CODE BEGIN 2 */
#ifdef BOOTLOADER_APPLICATION
CheckUpdate();
#endif
LL_USART_EnableIT_RXNE(USART1);
/* USER CODE END 2 */

 ```

## 2.How to build application

Comment '#define BOOTLOADER_APPLICATION' on main.h

```C
/* USER CODE BEGIN Private defines */

//#define BOOTLOADER_APPLICATION

/* USER CODE END Private defines */
```

This will disable ```CheckUpdate``` Function on startup located at main.c, and set set VTOR register to the application address

```C
/* USER CODE BEGIN 1 */
#ifndef BOOTLOADER_APPLICATION
 SCB->VTOR = APPLICATION_ADDRESS;
#endif
/* USER CODE END 1 */
```

## 3.Test

### 1.First, we build the bootloader and download it to the flash, we use the get info command to see what msg income

```BASH
D:\stm32IAPDemo\test>stm32IAPDemo.exe -c COM2 -i
get info
msg from bootloader
press any key to exit...
```

### 2.Second, we use the update command to download the firmware APP1.bin to the flash

```BASH
D:\stm32IAPDemo\test>stm32IAPDemo.exe -c COM2 -u APP1.bin
update firmware...
update firmware done
press any key to exit...
```

 and excute the get info command again

```BASH
D:\stm32IAPDemo\test>stm32IAPDemo.exe -c COM2 -i
get info
msg from application1
press any key to exit...
```

### 3.Third, we use the update command to download the firmware APP2.bin to the flash

```BASH
D:\stm32IAPDemo\test>stm32IAPDemo.exe -c COM2 -u APP2.bin
update firmware...
update firmware done
press any key to exit...
```

 and excute the get info command again

```BASH
D:\stm32IAPDemo\test>stm32IAPDemo.exe -c COM2 -i
D:\stm32IAPDemo\test>stm32IAPDemo.exe -c COM2 -i
get info
msg from application1
press any key to exit...
```

Enjoy!