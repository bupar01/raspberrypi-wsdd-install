# WSDD Installation procedure for Raspberry Pi
Web Service Discovery host daemon (WSDD) allows linux machines to show up in Windows Network.
## Why would you need WSDD
Since Windows 10 updates in 2021, and may be shortly before that, samba servers are no longer showing up in Windows Explorer's Network window.
The python3 [wsdd script](https://github.com/christgau/wsdd) by Steffen Christgau published in GitHub implemented a Web Service Discovery host daemon provides a workaround for this problem until samba implements an internal solution.
## How do you install it in Raspberry Pi OS
1. make sure python3 is installed and where it is
	`which python3`
1. install python3 if not present
    `sudo apt install python3`

The next series of commands do the following:
1. download wsdd.py from github
1. move file to /usr/local/bin and make executable
1. create service and enter into editor


{

	wget https://raw.githubusercontent.com/christgau/wsdd/master/src/wsdd.py
	sudo mv wsdd.py /usr/local/bin/wsdd.py && sudo chmod a+x /usr/local/bin/wsdd.py
	sudo systemctl edit --force --full wsdd.service

}

Enter next block into the editor, edit location of python3 obtained in step 1 above, and save:

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


5. enable  & start service

	`sudo systemctl enable wsdd.service && sudo systemctl start wsdd.service`
