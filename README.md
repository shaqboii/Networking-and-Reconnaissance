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

