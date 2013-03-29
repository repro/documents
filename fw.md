Setup
-----

	sudo cu -l /dev/tty00 -s 115200

Press 's' during memory test to enter BIOS setup.
Enable PXE boot.

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
	Setup a user? (enter a lower-case loginname, or 'no') [no] <user>
	Full user name for <user>? [<user>]
	Password for <user> account? (will not echo)
	Password for <user> account? (again)
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
Disable PXE boot

	(9) 9600 baud (2) 19200 baud (3) 38400 baud (5) 57600 baud *1* 115200 baud
	*C* CHS mode (L) LBA mode (W) HDD wait (V) HDD slave *U* UDMA enable
	(M) MFGPT workaround
	(P) late PCI init
	*R* Serial console enable 
	(E) PXE boot enable 
	(X) Xmodem upload 
	(Q) Quit

	Save changes Y/N ? y

### Setup PKG
	echo "installpath = http://mirror.switch.ch/ftp/pub/OpenBSD/`uname -r`/packages/i386/" > /etc/pkg.conf

### Enable IP forwarding
	vi /etc/sysctl.conf
	net.inet.ip.forwarding=1
	sysctl net.inet.ip.forwarding=1

### Setup the interfaces
	echo "inet 192.168.1.1 255.255.255.0" > /etc/hostname.vr1
	echo "inet 10.0.0.1 255.255.255.0" > /etc/hostname.ath0
	chmod 640 /etc/hostname.ath0 /etc/hostname.vr1
	echo "nwid <nwid> wpa wpakey <password> media OFDM54 mediaopt hostap wpaprotos wpa2 mode 11a" >> /etc/hostname.ath0

### Setup DHCP client
	cp /etc/dhclient.conf /etc/dhclient.conf`date +%Y%m%d`
	echo "initial-interval 1;" > /etc/dhclient.conf
	echo "request subnet-mask, broadcast-address, routers;" >> /etc/dhclient.conf
	#echo "request subnet-mask, broadcast-address, routers, domain-name-servers;" >> /etc/dhclient.conf

### Configure packet filter
	cp /etc/pf.conf /etc/pf.conf`date +%Y%m%d`
	vi /etc/pf.conf
	pfctl -f /etc/pf.conf
	
pf.conf

	set skip on lo

	anchor "ftp-proxy/*"
	pass in quick on vr1 inet proto tcp to port ftp divert-to 127.0.0.1 port 8021

	block log

	pass out log on vr0 proto {udp,tcp} from any to any port {domain,www,https,smtp,ftp,ntp,whois,8000,8080}
	pass out log on vr0 proto {udp,tcp} from vr1:network to any port {domain,ssh,www,https,smtp,submission,pop3s,ntp,whois,nntp,8000,8080,17} nat-to (vr0)
	pass in  log on vr1 proto {udp,tcp} from vr1:network to any port {domain,ssh,www,https,smtp,submission,pop3s,ntp,whois,nntp,nfs,sunrpc,8000,8080,17}
	pass in log on vr1 proto {udp,tcp} from vr1:network to vr1:network
	#pass in log on vr0 proto {udp,tcp} from any to vr0 port {www}

	pass in log on ath0 proto {udp,tcp} from ath0:network to any port {domain,www,https,smtp,pop3s}
	#pass in log on ath0 proto {udp,tcp} from ath0:network to ath0:network
	pass out log on vr0 proto {udp,tcp} from ath0:network to any port {domain,www,https,smtp,pop3s} nat-to (vr0)
	
### Setup FTP proxy
	echo "ftpproxy_flags=" >> /etc/rc.conf.local

### Configure dhcpd
	cp /etc/dhcpd.conf /etc/dhcpd.conf`date +%Y%m%d`
	vi /etc/dhcpd.conf
	echo "dhcpd_flags=\"vr1 ath0\"" >> /etc/rc.conf.local

dhcpd.conf

	subnet 192.168.1.0 netmask 255.255.255.0 {
		option routers 192.168.1.1;
		option domain-name "localdomain";
		option domain-name-servers 192.168.1.1;
		range 192.168.1.32 192.168.1.127;
		
		host static-client {
			hardware ethernet 00:26:ab:f7:61:3e;
			fixed-address 192.168.1.200;
		}
	}
	
	subnet 10.0.0.0 netmask 255.255.255.0 {
		option routers 10.0.0.1;
		option domain-name-servers 10.0.0.1;
		range 10.0.0.32 10.0.0.127;
	}

### Configure named
	cp /var/named/etc/named.conf /var/named/etc/named.conf`date +%Y%m%d`
	vi /var/named/master/localdomain
	vi /var/named/etc/named.conf
	echo "named_flags=" >> /etc/rc.conf.local

localdomain

	$ORIGIN localdomain.
	$TTL 6h
	
	@	IN	SOA	localdomain. root.localdomain. (
				1       ; serial
				1h      ; refresh
				30m     ; retry
				7d      ; expiration
				1h )    ; minimum

			NS      localdomain.
			A       192.168.1.1
	fw              A       192.168.1.1
	printhost       A       192.168.1.200
	printer         CNAME   printhost

named.conf

	acl clients {
		192.168.1.0/24;
		10.0.0.0/24;
	};

	options {
		listen-on { 192.168.1.1; 10.0.0.1; };
		allow-notify { none; };
		allow-transfer { none; };
		allow-query { clients; };
		allow-query-cache { clients; };
		allow-recursion { clients; };
	};
	
	zone "localdomain" {
		type master;
		file "master/localdomain";
		allow-transfer { localhost; };
	};

### Configure hosts file
	vi /etc/hosts

###  Reboot
	reboot
	

Useful commands
---------------

### Restart network
	. /etc/netstart
	
