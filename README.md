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


#### Set SamC21 to receive all can messages:
```
hal/include/hpl_can.h
struct can_filter {
	uint32_t idStart;   /* Message identifier */
	uint32_t idEnd;   /* Message identifier */
	uint32_t mask; /* The mask applied to the id */
	
};

hpl/can/hpl_can.c
/**
 * \brief Set CAN to the specified mode
 */
int32_t _can_async_set_filter(struct _can_async_device *const dev, uint8_t index, enum can_format fmt, struct can_filter *filter)
{
.....
		sf->S0.val       = filter->mask;
		sf->S0.bit.SFID1 = filter->idEnd;
		sf->S0.bit.SFID2 = filter->idStart;
.....
	} else if (fmt == CAN_FMT_EXTID) {
		if (filter == NULL) {
			ef->F0.val = 0;
			return ERR_NONE;
		}
		ef->F0.val      = filter->idStart;
.....
}



main.c
	// receive any data on CAN0
	filter.idStart   = 0x000;
	filter.idEnd   = 0x7FF;
	filter.mask = 0xFFF;
	can_async_set_filter(&CAN_0, 0, CAN_FMT_STDID, &filter);
```
