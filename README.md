# Ducato can speedmeter
The goal of these project is to build a can device thats translate the speed signal +15%. A second goal is to switch on the camera during driving and prevent the sleep mode after 20 minutes of the infotainment system

# Hardware
We use the SammyC21 Board from chip45.com. https://www.chip45.com/products/sammy-c21-1.0_atmel_smart_arm_sam_module_board_samc21_usb_dual_two_can.php?de
Order the board with a pre installed chip45bootSAM Bootloader. It´s super easy to flash the board with this bootloader via USB (windows only ;( )

## Instrument cluster / Kombinstrument pins

![Kombiinstrument](documentation/images/pins.png "Kombiinstrument")
![Kombiinstrument2](documentation/images/IMG_20201023_101301.jpg "Kombiinstrument2")
![Kombiinstrument3](documentation/images/IMG_20201023_101332.jpg "Kombiinstrument3")



### sidenotes for people who run into the same issues like me: 
#### Set the SAMC21 to 50kbit/s
```
#define CONF_CAN0_BTP_BRP 40
#define CONF_CAN0_BTP_TSEG1 13
#define CONF_CAN0_BTP_TSEG2 2
#define CONF_CAN0_BTP_SJW 1

#define CONF_CAN0_DBTP_DBRP 40
#define CONF_CAN0_DBTP_DTSEG1 13
#define CONF_CAN0_DBTP_DTSEG2 2
#define CONF_CAN0_DBTP_DSJW 1
```
