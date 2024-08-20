
![image](https://github.com/user-attachments/assets/09cf167b-ac16-45f5-9afc-5a66232cba1d)

**Reconnaissance**

I run four quad nmap scans 
*nmap -sS -v $IP --top-ports 1000
**rustscan -a $IP --ulimit 5000
***nmap -sU -v --top-ports 5 10.10.10.95
nmap -p- --min-rate 10000 -T4 10.10.10.95

![image](https://github.com/user-attachments/assets/5a46ac1f-83d8-45c7-967a-9d3ea37adf4a)

 nmap -sS -v $IP --top-ports 1000 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-20 21:00 +08
Initiating Ping Scan at 21:00
Scanning 10.10.10.95 [4 ports]
Completed Ping Scan at 21:00, 0.06s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 21:00
Completed Parallel DNS resolution of 1 host. at 21:00, 0.01s elapsed
Initiating SYN Stealth Scan at 21:00
Scanning 10.10.10.95 [1000 ports]
Discovered open port 8080/tcp on 10.10.10.95                                                                        
Completed SYN Stealth Scan at 21:00, 6.03s elapsed (1000 total ports)                                               
Nmap scan report for 10.10.10.95                                                                                    
Host is up (0.045s latency).                                                                                        
Not shown: 999 filtered tcp ports (no-response)                                                                     
PORT     STATE SERVICE                                                                                              
8080/tcp open  http-proxy                                     
                                                             
We have only one open port 
Port 8080: running http-proxy 
![image](https://github.com/user-attachments/assets/c17e1523-7520-4938-9e9d-9406b2262860)

![image](https://github.com/user-attachments/assets/b25868a2-2de4-48f2-a1e5-1537de6294ab)

The gobuster, nikto and droopescan scans kept timing out. The web server seems to be not able to handle the requests that these tools were sending.



![image](https://github.com/user-attachments/assets/5a828589-b52e-4680-b3f3-a8436e85fc3f)


![image](https://github.com/user-attachments/assets/af633c62-4676-4cee-8f75-39341a35076e)


