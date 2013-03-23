Access Blackberry Z10 media files with OpenBSD (Ethernet over USB)

	# pkg_add sharity-light
	# ifconfig cdce0 inet <local-ip-address> 255.255.255.253
	# shlight //<blackberry-device-name>/media /mnt -U BlackBerry -P <password>
