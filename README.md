# SKZ_app_utils
This is a repo for application utilities for Microchip switch KSZ8567 and other KSZxxxx

# I. regs_bin utility
## 1. Description  
   This utility is used to read and write KSZ switch registers

## 2. Usage
	regs_bin <device_name> <operation>
		+ device_name: is a device file name of the control interface, for example 0-005f for an I2C device on bus 0, slave address 0x5f
		+ operation: is one of the following options
			r <reg> [count]                        : read count bytes from register at address reg, count = 1 by default
			rb <reg>                               : read 8-bit data at address reg
			rw <reg>                               : read 16-bit data at address reg
			rd <reg>                               : read 32-bit data at address reg
			w <reg> <val> [val]                    : write 8-bit data specified by val options to at address reg
			wb <reg> <val>                         : write 8-bit data specified by val to address reg
			ww <reg> <val>                         : write 16-bit data specified by val to address reg
			wd <reg> <val>                         : write 32-bit data specified by val to address reg

# II. web_gui utility
## 1. Description
	This utility is a web interface to configure KSZ switch

## 2. Web setup procedure
	2.1) Install lighttpd web server
		$ sudo apt-get install lighttpd

	2.2) Configure document-root
		Modify /etc/lighttpd/lighttpd.conf in the target as below
		[BEFORE]
			server.document-root        = "/var/www/htlm"
		[AFTER]
			server.document-root        = "/var/www"

	2.3) Configure lighttpd
		$sudo lighttpd-enable-mod cgi
		$sudo /etc/init.d/lighttpd force-reload


	2.4) Copy web_gui to target
		$sudo cp web_gui/swcfg /usr/sbin/
		$sudo chmod +x /usr/sbin/swcfg

		$sudo mkdir /var/www/cgi-bin
		$sudo cp web_gui/config.py /var/www/cgi-bin/
		$sudo cp web_gui/user_auth.py /var/www/cgi-bin/
		$sudo chmod +x /var/www/cgi-bin/*

		$sudo cp web_gui/index.html /var/www
		$sudo vim /etc/sudoers
		Add following line next to root
			# www-data ALL= NOPASSWD: /usr/bin/python, /usr/sbin/regs_bin, /usr/sbin/mdio-tool, /usr/sbin/swcfg

	2.5) Change file mode
		$ sudo chmod -R 777 /sys/bus/i2c/devices/0-005f/*

	2.6) To access web gui
			- From a remote computer, open web browser and enter target_board_IP:80
			- Or, from target board, open web browser and enter "localhost"
