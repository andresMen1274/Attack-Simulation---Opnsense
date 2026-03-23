# Attack-Simulation---Opnsense
In this project I will be doing a exploit of my Windows 10 virtual machine using remote desktop protocol and a brute force attack.

First I will be installing Sysmon onto my Windows 10 virtual machine. I will be doing this because it provides deeper endpoint visibility. To download Sysmon search Sysmon and download it from the offical Microsoft website. 

<img width="357" height="273" alt="image" src="https://github.com/user-attachments/assets/ec5c63e5-3520-45d6-b4a9-fc996a2d7d32" />

Download it and we will download the olaf configuration. Navigate to github and search olaf sysmon configuration. Scroll down to the sysmonconfig.xml file. Download it as a raw file then save it. Next extract the contents of the compressed sysmon file. Then copy the path of the folder and open powershell with administrator privlages. 

<img width="1121" height="201" alt="image" src="https://github.com/user-attachments/assets/57e317f8-6e8e-4ddb-b452-6b15466a0a28" />

Enter the command seen in the photo below to finish the configuration of Sysmon on the Windows 10 virtual machine.

<img width="856" height="377" alt="image" src="https://github.com/user-attachments/assets/d7d5941e-38cf-46f5-8a1e-141d6a5524cf" />

