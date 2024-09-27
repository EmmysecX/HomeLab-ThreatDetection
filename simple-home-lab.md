# Setting Up a Simple Home Lab for Threat Detection and Monitoring

As part of my cybersecurity learning journey, I have used **TryHackMe** to gain hands-on experience in labs. However, I wanted to challenge myself by working on personal projects, starting with building a simple home lab. I came across a tweet by **0xEH (@efamharris)** and an insightful article on his page about setting up a home lab. This led me to the YouTuber **MyDFIR**, whose step-by-step videos I found incredibly helpful. Combining both resources, I embarked on building my home lab.

I opted for **VMware** (which I prefer over VirtualBox) and set up **Kali Linux** as my attack machine, with **Windows 10** acting as my defense machine. After configuring the network, I embarked on setting up the tools needed for threat detection and monitoring.

## Setting Up Splunk and Sysmon

The first tool I installed was **Splunk**, a widely-used SIEM (Security Information and Event Management) tool. While the installation on the virtual machine took a while, I ensured everything was fully set up before proceeding with any attacks, as this would allow me to monitor the logs efficiently later on.

**Splunk installation successful**

Next, I installed **Sysmon** to capture telemetry on the Windows machine. With everything in place, I was ready to simulate an attack.

**Sysmon successfully installed**  
**Sysmon running on my defense machine**

## Day 2: Simulating the Attack

To begin, I scanned the target Windows machine for open ports using **Nmap**. Initially, I couldn't find any open ports, specifically **port 3389 (RDP)**. After troubleshooting and realizing the RDP connection was disabled on my Windows system, I enabled it and reran the scan, which successfully revealed the open port.

**Nmap Scan showing open ports**

I proceeded to create a **payload** on my attack machine using **Metasploit**. To avoid any interruptions during the process, I temporarily deactivated **Windows Defender** on my target machine.

**Payload created**

Next, I launched the attack using **msfconsole**. After configuring all the necessary settings, the handler was activated, waiting for the test machine to download the malware.

**Reverse TCP handler activated**

Then I set up an **HTTP server** to allow the Windows machine to download the malware.

Once ready, I went over to the Windows machine, downloaded the malware via a web browser, and executed it. My browser blocked the download at first, but I bypassed the warning by clicking on "Download insecure file."

**HTTP site for the sample malware**

After executing the malware, I opened the command prompt to check if there was a connection to my **Kali** machine. And there it was — an established connection between the attack and defense machines!

**Command prompt showing established connection between the two machines**

Then I headed back to my **Kali Linux** to see if there was a connection and, guess what? There was! I was able to create a **reverse TCP shell** on the attack machine, check the IP address, and retrieve other information about the defense machine remotely.

## Monitoring the Attack with Splunk

Returning to my **Kali Linux**, I confirmed that the **reverse TCP shell** was successfully created. I could view the **IP address** and other information about the Windows defense machine remotely.

**Meterpreter shell**

The primary goal of this lab was to detect and monitor the simulated threat. I used **Splunk** to generate the telemetry and analyze the logs.

In **Splunk**, I was able to see the attack carried out via **RDP port 3389**, with the default **Meterpreter port 4444**.

**Splunk Log of the RDP Attack via Port 3389**

The logs also displayed the **IP addresses** of both machines involved in the communication.

**Splunk log showing the IP address of both machines**

I queried the name of the malware in **Splunk**, revealing **13 events**. Expanding on the first event, I could see the **parent image**, the **malware name**, and the **parent ID**, which could be useful for further analysis.

**EventCode on Splunk**  
**Event 1 information**

## Reflecting as a Cybersecurity Analyst

As a cybersecurity analyst, I have to ask myself: **How can I effectively detect malicious activity like this, and what actions should I take to mitigate such threats?**

### Solution:

The key lies in **proactive monitoring** with tools like **Splunk** and **Sysmon**. I should regularly analyze logs for any unusual activities, such as unexplained **RDP connections** or **unauthorized file downloads**. Setting up **automated alerts** for critical events is also essential, along with routine system scans using tools like **Nmap**. Additionally, keeping security measures like **Windows Defender** active at all times, except during controlled testing, is vital for safeguarding against potential threats.

## Conclusion: The Importance of Hands-On Learning

This project gave me invaluable experience in detecting and monitoring threats in a controlled environment. It equipped me with the skills to set up a basic home lab for real-world simulation and strengthened my understanding of incident response. Through this process, I learned the significance of careful network setup, effective tool usage, and the importance of monitoring systems closely for any suspicious activity.

And hey, as a **blue teamer**, I’ve got to admit — defending against attacks is a lot more fun when I’m the one in control of the malware! 😄

**More projects to come...**
