# STM32 NUCLEO-F767ZI USB CDC Example

It finally connects to Host and provides Virtual COM port (for `User USB` connector on Nucleo board).

* red LED D3 lights on fatal error
* blue LED D2 is blinking (to signal that CPU is running)
* Thi CDC Transmit/Receive are currently No-operation (planned
  control of green LED D1 in near future).


WARNING! The Code Generated by `CubeMX 5.3.0` is buggy (at least
for firmware `STM32Cube_FW_F7_V1.15.0`) - you must
apply this fix to generated code:

```diff
--- a/Src/usbd_conf.c
+++ b/Src/usbd_conf.c
@@ -206,11 +206,11 @@ void HAL_PCD_ResetCallback(PCD_HandleTypeDef *hpcd)
 { 
   USBD_SpeedTypeDef speed = USBD_SPEED_FULL;
 
-  if ( hpcd->Init.speed == PCD_SPEED_HIGH)
+  if ( hpcd->Init.speed == USB_OTG_SPEED_HIGH)
   {
     speed = USBD_SPEED_HIGH;
   }
-  else if ( hpcd->Init.speed == PCD_SPEED_FULL)
+  else if ( hpcd->Init.speed == USB_OTG_SPEED_FULL)
   {
     speed = USBD_SPEED_FULL;
   }
```

Also in my case I need to replace:
* `__packed="__attribute__((__packed__))"`
with
* `__packed="__attribute__((packed))"`
to avoid build errors (in C/C++ Build -> Settings -> MCU GCC
Compiler -> Preprocessor).

NOTE: To avoid problems please use this directory structure:
* `/home/ansible/projects/stm32-usb-cdc1/` - path of this project
* `/home/ansible/STM32Cube/Repository/STM32Cube_FW_F7_V1.15.0/` - path
  of firmware package `STM32Cube_FW_F7_V1.15.0`

Used Software:
* OS: Debian Linux 9.9/amd64
* CubeMX 5.3.0, installer: `en.STM32CubeMX_v5-3-0.zip`
* Firmware: `STM32Cube_FW_F7_V1.15.0`
* IDE: SW4STM32 v2.9 for Linux/64-bit, installer: `install_sw4stm32_linux_64bits-v2.9.run`

