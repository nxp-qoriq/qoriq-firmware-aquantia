### Steps to flash Aquantia Ethernet-PHYs firmware from U-Boot

##### What you will find here:

- Software to program PHY firmware **aq_programming_xxx.bin**

- Ethernet-phy firmware binary (**.cld files**)

- Steps to upgrade

  

##### For a list of available ethernet-phy firmware files, check out the below table.

|   Board type    | PHY model | Firmware version | System interface |                           Filename                           |
| :-------------: | :-------: | :--------------: | :--------------: | :----------------------------------------------------------: |
| **LX2160A-RDB** |  AQR107   |      v3.5.E      |       XFI        | AQR-G2_v3.5.E-AQR_Freescale_LX2RDB-XFI_ID24377_VER965.cld |
|                 |           |      v3.5.E      |     USXGMII      | AQR-G2_v3.5.E-AQR_Freescale_LX2RDB-USXGMII_ID24109_VER965.cld |
| **LX2160A-QDS** |  AQR405   |      v2.C.8      |       XFI        |  AQ28nm_v2.C.8-AQR_Freescale_LX2QDS-XFI_ID24724_VER1019.cld  |
|                 |           |      v2.C.8      |     USXGMII      | AQR405:AQ28nm_v2.C.8-AQR_Freescale_LX2QDS-USXGMII_ID24191_VER997.cld |
|    **T2080**    |  AQ1202   |     v1.37.10     |                  |       Firmware_1.37.10_011014_Freescale_T2080PCIe.cld        |
|  **T1024-RDB**  |  AQR105   |     v2.0.B3      |    2.5G SGMII    |        AQ28nm-FW_2.0.B3_Freescale_T1024RDB_120514.cld        |
|                 |           |     v2.0.B9      |       XFI        |        AQ28nm-FW_2.0.B9_Freescale_T1024RDB_012115.cld        |
| **LS1028A-QDS** |  AQR112   |      v4.3.C      |                  |     AQR-G3_v4.3.C-AQR_NXP_SPF-30842_ID24155_VER1198.cld      |
|                 |  AQR412C  |      v4.3.C      |                  |   AQR-G3_v4.3.C-AQR_NXP_SPF-30841_MUSX_ID40019_VER1198.cld   |



##### Aquantia Firmware Flashing utility

There are different aq_programming binaries working with specific U-boot versions.  The ones based on ATF (ARM Trusted Firmware) are different than the older ones based on PPA. Please find below a list of applications that must be used along with a U-boot/LSDK release.

| LSDK version | U-boot type |            Filename             |
| :----------: | :---------: | :-----------------------------: |
|     1906     |     TFA     | aq_programming_atf_lsdk1906.bin |
|     1903     |     TFA     | aq_programming_atf_lsdk1903.bin |
|    Older     |     PPA     |     aq_programming_ppa.bin      |



##### Steps to flash firmware on RDB board

1. Download the proper aq_programming and ethernet-phy firmware to TFTP location or storage media.

2. Check MACs connected to an Aquantia PHY (example on LX2160A-RDB; notice that there are two phys)

   *=> mdio list*

   *FSL_MDIO0:*

   *0 - Cortina CS4223 <--> DPMAC2@xlaui4*

   *1 - AR8035 <--> DPMAC17@rgmii-id*

   *2 - AR8035 <--> DPMAC18@rgmii-id*

   ***4 - Aquantia AQR107 <--> DPMAC3@xgmii***

   ***5 - Aquantia AQR107 <--> DPMAC4@xgmii****

3. Execute aq_programming_xxx.bin to flash ethernet-phy firmware (aq_programming_atf_lsdk1906.bin is used in the following example)

   The usage is the following: ***go 0x80300000 <ETHDEV_NAME> <firmware_address> <firmware_size>***

   Below are two examples on how to flash firmware on a LX2160A-RDB board on both PHYs.

   **USB example:**

   *mdio list;*

   *usb start;*

   *fatload usb 0:1 0x80300000 aq_programming_atf_lsdk1906.bin;*

   *fatload usb 0:1 0x82000000 AQR-G2_v3.5.E-AQR_Freescale_LX2RDB-XFI_ID24377_VER965.cld;*

   *go 0x80300000 DPMAC3@xgmii 0x82000000 $filesize;*

   *go 0x80300000 DPMAC4@xgmii 0x82000000 $filesize;*

   **TFTP example:**

   *mdio list;*

   *tftp 0x80300000 aq_programming_atf_lsdk1906.bin;*

   *tftp 0x82000000 AQR-G2_v3.5.E-AQR_Freescale_LX2RDB-XFI_ID24377_VER965.cld;*

   *go 0x80300000 DPMAC3@xgmii 0x82000000 $filesize;*

   *go 0x80300000 DPMAC4@xgmii 0x82000000 $filesize;*

4. Notes

   The LX2160A-RDB boards are shipped with USXGMII firmware.

