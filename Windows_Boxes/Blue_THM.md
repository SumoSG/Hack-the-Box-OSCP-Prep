
Blue is a Try hack me machine
![image](https://github.com/user-attachments/assets/2f77bd75-7f2d-4c6b-b277-7735d402cc25)

**Reconnaissance**

First thing first, we run a quick initial nmap scan to see which ports are open and which services are running on those ports.
*sudo nmap -sC -sV 10.10.232.47 -O -oA initial  -oN tcp_scan.nmap*
![image](https://github.com/user-attachments/assets/3571879c-685f-4d45-bf8f-964fa1fd79bd)

* -sC: run default nmap scripts
* sV: detect service version
* -O: detect OS
* -oA: output all formats and store in file nmap/initial

  Port 135: Windows Remote procedure call
  Port 139: netbios-ssn
  Port445 : Windows 7 professional 7601 Service Pack 
