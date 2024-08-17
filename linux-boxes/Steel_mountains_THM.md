Steel Mountain is a THM machine 

![image](https://github.com/user-attachments/assets/c9a4dba0-4117-4d65-9e7f-3f8dd916f518)

First thing first, we run a quick initial nmap scan to see which ports are open and which services are running on those ports.

nmap -sC -sV 10.10.224.110 -oA initial  

* -sC: run default nmap scripts
* -sV: detect service version
* -O: detect OS [not allowed to run on this machine- I got an error]
* -oA: output all formats and store in file initial

We get back the following result showing that below ports are open:

* Port 80: running Microsoft IIS httpd 8.5 HTTP method risky :Trace
* Port 135: msrpc
* Port 139: netbios-ssn
* Port 445:  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
* Port 3389: ssl/ms-wbt-server?
* Port 8080 : open  http               HttpFileServer httpd 2.3 http-server-header: HFS 2.3





![image](https://github.com/user-attachments/assets/66f63413-c1bd-4ba1-9f8d-703c42515757)

Before we start investigating these ports, let’s run more comprehensive nmap scans in the background to make sure we cover all bases.

Let’s run an nmap scan that covers all ports.

nmap -sC -sV -O -p- -oA full $IP


We get back the following result. No other ports are open.



Similarly, we run an nmap scan with the -sU flag enabled to run a UDP scan.

nmap -sU -O -p- -oA udp $IP

