# WSDD Installation procedure for Raspberry Pi
Web Service Discovery host daemon (WSDD) allows linux machines to show up in Windows Network.
## Why would you need WSDD
Since Windows 10 updates since 2021 and may be shortly before that, samba servers are no longer showing up in Windows Explorer's Network window.
The python3 [wsdd script](https://github.com/christgau/wsdd) by Steffen Christgau published in GitHub implemented a Web Service Discovery host daemon provides a workaround for this problem until samba implements an internal solution.
## How do you install it in Raspberry Pi OS
1. make sure python3 is installed and where it is, install python3 if not present (apt install python3) 
	which python3
1. download wsdd.py from github
1. move file to /usr/local/bin and make executable
1. create service and enter into editor


	`wget https://raw.githubusercontent.com/christgau/wsdd/master/src/wsdd.py`
	`mv wsdd.py /usr/local/bin/wsdd.py && chmod a+x /usr/local/bin/wsdd.py`
	`systemctl edit --force --full wsdd.service`


{
	[Unit]
	Description=WSDD Service
	Wants=network.target
	After=network.target
	 
	[Service]
	ExecStartPre=/bin/sleep 5
	ExecStart=/usr/bin/python3 /usr/local/bin/wsdd.py
	Restart=always
	RestartSec=1
	 
	[Install]
	WantedBy=multi-user.target
}

1. enable service
1. start service

	`systemctl enable wsdd.service`
	`systemctl start wsdd.service`
