# Networking and Reconnaissance
This lab demonstrates the use of commands like nmap and netcat to understand what services are running on our machine, what ports are open, how we can communicate with other machines, and trying to probe further into the services we autonomously run.

NOTE: If you are to recreate this lab, be sure to do it in a closed or virtual environment. Port scanning on public WiFi is illegal, so tread carefully.

<h2>Step 1: Scan your host</h2>
To scan our host machine for open ports, lets run this command:

```bash
nmap -A localhost
```
```nmap```, which is short for Network Mapping, is a Linux package that is essential for network discovery and port scanning. Including the ```-A``` flag is a more aggressive means of port scanning, where it fetches us the target's OS, version, and traceroute. I've replaced the target machine with my machine, ```localhost```, so that I don't infring upon the privacy of others. Let's see what it put out.

<img src="https://i.imgur.com/YtQqXKP.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

As we can see, port 631 is open using the TCP protocol. The service it is running is IPP, or Internet Printing Protocol. This service allows us to remotely send print jobs to a printer in the network. The version, CUPS 2.4, is the OS, and it facilitates the management of print jobs.

Next, let's scan our LAN, replacing ```-A``` with ```-sn```.

<img src="https://i.imgur.com/YPeG8ZG.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

This scan checks for devices in my local area. As my machine is the only one running in the network, it is therefore the only one found to be up.

<h2>Step 2: Experiment with different scan methods</h2>

We can use different flags to accomplish different things when mapping the network. Let's check for TCP connections with the ```-sT``` flag.

<img src="https://i.imgur.com/YO2xMCA.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

Again, we can see port 631 as it is TCP and running a service. Now, how about if we wanted to map the network without alerting anything?

Nmap offers a stealth scan flag, however it requires root privileges. To do this, we will add sudo before our command. To modify the command as a stealth search, we use the ```-sS``` flag.

<img src="https://i.imgur.com/tUvcspm.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

This accomplishes the same workings as the other scans, but stealthily.

Let's try to find open ports utilizing UDP. To do this, we will use the ```-sU``` flag.

<img src="https://i.imgur.com/HMu5ZyI.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

Let's try to find the OS of the target. To do this, we will use the ```-O``` flag.

<img src="https://i.imgur.com/fvxXivT.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

As we can see, the target machine, which is just my machine, is running Linux. It even contains information such as the version and device purpose.

<h2>Step 3: Netcat</h2>

Netcat is a vital networking tool that can connect, listen to, and transfer data from a port. To demonstrate this, let's create two terminals that will be sending messages to each other on a specified port.

<img src="https://i.imgur.com/QGm7ra6.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

We use ```nc``` for Netcat. ```-lnvp``` is a mix of many flags together. The l means listen, the n means to bypass DNS, v means verbose, and p means that you're indicating to netcat that a port will be specified. In this example, I use port 4444.

Then, on another terminal, I run ```nc 127.0.0.1 4444``` to connect to the initiator terminal. After that, the initial terminal will print a message stating that the connection has been made, and now the two terminals can communicate.

<img src="https://i.imgur.com/ewiSxXj.png" height="80%" width="80%" alt="Networking and Reconnaissance"/>

So, why does any of this matter? Well, it's to demonstrate that open or vulnerable ports can be manipulated or utilized to do things that bad actors could want. For example, they could use the nc command to remotely feed personal information to them from a hostmachine. This highlights the importance of network scanning and seeing what ports can be opened, which ones are running potentially unsafe services, and how to best create an environment that allows traffic and blocks unwanted ones.

A common use of Netcat is to banner grab. Banner grabbing is running netcat on a port to gather more information about a service running on an open port. 

Let's say that I wanted to banner grab a service on port 80 (HTTP). To do this, I would netcat into the port.

```bash
nc 192.168.1.1 80
```
The resulting output would look something like this:

```bash
HTTP/1.1 400 Bad Request
Server: Apache/2.4.41 (Ubuntu)
Date: Tue, 27 Aug 2025 12:00:00 GMT
```

This concludes the lab.

<h2>Conclusion</h2>

One of the most vital elements of war is to gather everything you know about your enemy. Running reconnaissance and analysis on any and all network information you gather from an organization is vital towards knowing where to penetrate. Both red and blue team specialists must work to understand everything about ports in order to attack or protect their assets. This lab touches on two vital commands in Linux that cover port mapping on a broad level, but the topics covered are essential towards making for a more scrutinized and protected environment.

