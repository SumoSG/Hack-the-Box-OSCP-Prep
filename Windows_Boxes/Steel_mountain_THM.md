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


**Enumeration**
I always start off with enumerating HTTP first. In this case both 80 and 8080 are open so we’ll start there.

Ports 80 & 8080

Visit the site on the browser.

![image](https://github.com/user-attachments/assets/12aec968-4a0e-4c18-8c4d-7302df33f566)


![image](https://github.com/user-attachments/assets/6de5a185-9e60-4c41-bc2a-eec9d4a12a2c)

View the source code to see if we can find any other information. We see that the photo on the port 80 is Employee of the month Bill Harper 

![image](https://github.com/user-attachments/assets/9b04f2a8-adab-4d60-bdee-0baaae09a824)

What is the CVE number to exploit this file server?

We know the name of the file version, and that it is running version 2.3. We can therefore use the searchsploit command (or look at exploitdb’s website). I entered the following command:

searchsploit HFS -w

This gave me the following results. Note I used the -w flag to get a link to exploit-db.

![image](https://github.com/user-attachments/assets/98505663-dd1b-4c86-a8b3-56c8941464a9)

Of you look carefully you can see that there are are at least two exploits for version 2.3.x. Let’s try the Remote Command Execution (1) at the following link:

https://www.exploit-db.com/exploits/34668?source=post_page-----90af67ec1af6--------------------------------

Use Metasploit to get an initial shell. What is the user flag?

Startup Metasploit by running msfconsole.

The great thing about Metasploit is that we can search on CVE number. Let’s do this by running:

search 2014–6287
![image](https://github.com/user-attachments/assets/13289ada-5d7e-4f73-a02d-614f25f0dd5b)

![image](https://github.com/user-attachments/assets/1fd2fdb3-c23a-4710-a8e3-a9b7360fdea3)

We can follow this by entering use 0. This select the module.

To see the different options we need to adjust we can run show options.




