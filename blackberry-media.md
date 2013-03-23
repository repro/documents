How to access Blackberry Z10 media files with OpenBSD
-----------------------------------------------------

### Install the required package (once)
	$ sudo pkg_add samba

### Prepare Blackberry Z10 (once)
* System Settings -> Storage and Access
** USB Connections: Connect to Mac
* Access using WI-FI: on
** Set Storage Access Password
* Identification on Network
** Device Name 
** Username
* Access using WI-FI: off

### Connect
* Plug USB cable
* Configure the interface with dhcp:
	$ sudo dhclient cdce0
* Login to the device:
	$ smbclient -R bcast -U <username%<password> //<device-name>/media
