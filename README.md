# STM32_IAP
学习一下IAP固件升级。芯片用的是STM32L071RBT6。所以片上资源带有128K的FLASH,还有6K的EEPROM，20K的SRAM。

<img src=".\2_Images\flash.png" alt="flash" style="zoom:50%;" />

用stm32cubemx创建了两个工程：Bootloader和app。

|            | Start Address | End Address |
| ---------- | ------------- | ----------- |
| Bootloader | 0x0800 0000   | 0x0800 2FFF |
| APP        | 0x0800 3000   | 0x0800 FFFF |
| APP_OTA    | 0x0801 0000   | 0x0801 FFFF |

按照以上的地址分配ROM空间。这样的话，Bootloader程序不能超过12K,APP程序不超过52K，APP_OTA不超过64K。

上图可以看到EEPROM的地址范围是：**0x0808 0000 ~~0x0808 17FF**

所以EEPROM一共是6K。现在的想法是，把升级的标志位放在EEPROM里边，boot程序读到升级的标志位，就把APP_OTA的内容写进APP的地址范围里。