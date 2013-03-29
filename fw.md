  sudo cu -l /dev/tty00 -s 115200

Press 's' during memory test to enter BIOS setup.
Enable PXE boot

  (9) 9600 baud (2) 19200 baud (3) 38400 baud (5) 57600 baud *1* 115200 baud
  *C* CHS mode (L) LBA mode (W) HDD wait (V) HDD slave *U* UDMA enable
  (M) MFGPT workaround
  (P) late PCI init
  *R* Serial console enable 
  *E* PXE boot enable 
  (X) Xmodem upload 
  (Q) Quit
  
  Save changes Y/N ? y

  (I)nstall, (U)pgrade or (S)hell? i
  Terminal type? [vt220]
  System hostname? (short form, e.g. 'foo') fw
  Which one do you wish to configure? (or 'done') [vr0]
  IPv4 address for vr0? (or 'dhcp' or 'none') [dhcp]
  IPv6 address for vr0? (or 'rtsol' or 'none') [none]
  Which one do you wish to configure? (or 'done') [done]
  DNS nameservers? (IP address list or 'none') [none]
  Password for root account? (will not echo)
  Password for root account? (again)
  Start sshd(8) by default? [yes]
  Start ntpd(8) by default? [no] yes
  NTP server? (hostname or 'default') [default]
  Do you expect to run the X Window System? [no]
  Change the default console to com0? [no] yes
  Which one should com0 use? (or 'done') [115200]
  Setup a user? (enter a lower-case loginname, or 'no') [no] u
  Full user name for u? [u]
  Password for u account? (will not echo)
  Password for u account? (again)
  Since you set up a user, disable sshd(8) logins to root? [yes]
  Which disk is the root disk? ('?' for details) [wd0]
  Use DUIDs rather than device names in fstab? [yes]
  Use (W)hole disk, use the (O)penBSD area, or (E)dit the MBR? [OpenBSD] w
  Use (A)uto layout, (E)dit auto layout, or create (C)ustom layout? [a]
  Location of sets? (disk ftp http or 'done') [http]
  HTTP/FTP proxy URL? (e.g. 'http://proxy:8080', or 'none') [none]
  Server? (hostname or 'done') 192.168.1.1
  Server directory? [pub/OpenBSD/5.2/i386]
      [X] bsd           [X] etc52.tgz     [ ] xbase52.tgz   [ ] xserv52.tgz
      [X] bsd.rd        [X] comp52.tgz    [ ] xetc52.tgz
      [ ] bsd.mp        [X] man52.tgz     [ ] xshare52.tgz
      [X] base52.tgz    [X] game52.tgz    [ ] xfont52.tgz
  Set name(s)? (or 'abort' or 'done') [done]
  Location of sets? (disk ftp http or 'done') [done]
  What timezone are you in? ('?' for list) [Canada/Mountain] Europe/Zurich
  # reboot

Press 's' during memory test to enter BIOS setup.
Now disable PXE boot

  (9) 9600 baud (2) 19200 baud (3) 38400 baud (5) 57600 baud *1* 115200 baud
  *C* CHS mode (L) LBA mode (W) HDD wait (V) HDD slave *U* UDMA enable
  (M) MFGPT workaround
  (P) late PCI init
  *R* Serial console enable 
  (E) PXE boot enable 
  (X) Xmodem upload 
  (Q) Quit

  Save changes Y/N ? y
