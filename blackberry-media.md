How to access Blackberry Z10 media files with OpenBSD
-----------------------------------------------------

### Install the required package
	$ sudo pkg_add samba

### Prepare Blackberry Z10
* System Settings -> Network Connections -> Wi-Fi: `Off`
* System Settings -> Storage and Access
* USB Connections: `Connect to Mac`
* Access using Wi-Fi: `On`
* `Password`
* Identification on Network
* Device Name 
* Username
* Access using Wi-Fi: `Off`

### Connect
* Plug USB cable
* Configure the interface with dhcp:
	`$ sudo dhclient cdce0`
* Access the device share:
	`$ smbclient -R bcast -U <username%<password> //<device-name>/media`
* Access the SD card:
	`$ smbclient -R bcast -U <username%<password> //<device-name>/removable_SDCARD`
