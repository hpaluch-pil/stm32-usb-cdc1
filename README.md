# STM32 NUCLEO-F767ZI USB CDC Example

Work in progress.

Currently crashes on USB connect:
```
stm32-usb-cdc1 Debug [Ac6 STM32 Debugging]	
	stm32-usb-cdc1.elf	
		Thread #1 (Suspended : Breakpoint)	
			Error_Handler() at main.c:277 0x80039f2	
			HAL_PCD_ResetCallback() at usbd_conf.c:219 0x8003dae	
			HAL_PCD_IRQHandler() at stm32f7xx_hal_pcd.c:1,222 0x8001038	
			OTG_FS_IRQHandler() at stm32f7xx_it.c:208 0x8003b78	
			<signal handler called>() at 0xfffffff9	
			HAL_Delay() at stm32f7xx_hal.c:366 0x80005d2	
			main() at main.c:106 0x8003b20	
	openocd	
	gdb	
```

because speed is `3` which is unknown value...

```c
void HAL_PCD_ResetCallback(PCD_HandleTypeDef *hpcd)
#endif /* USE_HAL_PCD_REGISTER_CALLBACKS */
{ 
  USBD_SpeedTypeDef speed = USBD_SPEED_FULL;

  if ( hpcd->Init.speed == PCD_SPEED_HIGH)
  {
    speed = USBD_SPEED_HIGH;
  }
  else if ( hpcd->Init.speed == PCD_SPEED_FULL)
  {
    speed = USBD_SPEED_FULL;
  }
  else
  { // speed is 3 - unsupported value...
    Error_Handler();
  }
    /* Set Speed. */
  USBD_LL_SetSpeed((USBD_HandleTypeDef*)hpcd->pData, speed);

  /* Reset Device. */
  USBD_LL_Reset((USBD_HandleTypeDef*)hpcd->pData);
}
```
