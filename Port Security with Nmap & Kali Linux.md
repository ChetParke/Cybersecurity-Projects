# Port Security using Nmap & Kali Linux

Within this lab I will be using Nmap and Kali Purple to secure vulnerable service ports. Service ports are crucial for communication between hosts, so having them open is essential for routine network operations. However, 
mismanaged service ports that support vulnerable operations can easily become attack vectors for malicous threat actors. This lab will deomstrate how to close unnessacarily open service ports for device and network hardening.

Devices used 
- Windows Server 2022 Domain Controller / Domain Member Server
- Kali Purple 2023 Linux Workstation

### Discover Unnecessary Open Ports

As mentioned previously open ports that are no longer of any use to the host are simply providing potential
entry points to attackers. We first need to discover these ports before we can secure them.

First we use the XAMPP Control Panel to initialize some open ports for test purposes through our Windows Server.
<img width="775" height="480" alt="image" src="https://github.com/user-attachments/assets/c3e1b320-668a-4fa0-92a8-a5e1ed61340d" />


Now we will hop into Kali Purple to discover the ports.

Within the Linux terminal we type the following command "nmap -Pn 192.168.0.2"
<img width="622" height="467" alt="image" src="https://github.com/user-attachments/assets/58982de6-148c-466e-9bbd-6de58b3444f8" />

This allows up to scan all the open port for the associated IP address.

We notice that these is a mySQL database and a webserver that should not be running or have an open port.
Lets issue another -A command on the ports that should not be open in nmap to find out more.

<img width="1189" height="827" alt="image" src="https://github.com/user-attachments/assets/2287f466-5403-4b33-9ab5-5176af0778f2" />

We notice that port 80 and 443 are running an Apache server, and 3306 is running a MariaDB. All of these services and ports are unnecessarily opened and need to be close to ensure proper security.

### Close Unnecessary Open Ports

Now that we have fully identified the open ports that pose a potential security risk we need to close them. 
We will be using Windows Defender Firewall to close the ports 80, 443, and 3306.

To do so we will make a new inbound rule for the asociated ports.

<img width="937" height="764" alt="image" src="https://github.com/user-attachments/assets/73339b6d-b1cd-4bd4-8d63-eb2021791def" />

Now i will set the action rule for the ports to "Block the connection", I will apply this to the Domain, private, and public options as well.

<img width="939" height="770" alt="image" src="https://github.com/user-attachments/assets/2e65e624-0c24-4c05-9751-1c7ba8f76747" />

Finally we can observe the new rule we created within our Windows Defender Firewall.

<img width="945" height="140" alt="image" src="https://github.com/user-attachments/assets/cc7a1a34-f095-4087-b0a4-980b2bbfd63c" />

Let hop back over into our Kali Linux and once again utalize nmap to see the new rule in action.

I rerun the initial terminal command of "nmap -Pn 192.168.0.2" to confirm.

<img width="596" height="314" alt="image" src="https://github.com/user-attachments/assets/92015d10-9627-4325-adc3-f70394aca999" />

And as expected the ports we wanted to block are no longer open, our new rule is working.


### Conclusion

Unnecessary open service ports pose a significant security risk by expanding a systems attack surface. Open ports provide
potential entry points for exploitation, service enumeration, and lateral movement by threat actors.

This lab demonstarates how Nmap can be used to efficiently identify active ports and running services on a host, providing clear visibility
into the system's exposed network footprint. Windows Defender Firewall was then used to enforce host-based controls by blocking unnecessary inbound traffic, and Nmap was re-used to validate that the security chages were effective. Together, thse tools illustrate a practical workflow for discovery, remediation, and verification of port-level security.



