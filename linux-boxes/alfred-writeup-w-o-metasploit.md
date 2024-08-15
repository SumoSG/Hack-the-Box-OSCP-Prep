Alfred is a THM box 

![image](https://github.com/user-attachments/assets/a13253ae-f2f2-4067-beab-119b4877f6a3)

**Reconnaissance**

Run map scan 

We get back the following result.

![image](https://github.com/user-attachments/assets/962f85c2-e284-4a72-a29c-8e9ab3bc8aea)


we have three open ports 

* port 80 Microsoft IIS httpd 7.5
* 3389/tcp open  ssl/ms-wbt-server
* 8080/tcp open  http Jetty  


![image](https://github.com/user-attachments/assets/f34d690d-d1b3-4409-a439-38055eb37d61)

**Enumeration**

Visit the Web application 

![image](https://github.com/user-attachments/assets/a3fa0f46-856d-4932-9cb3-045f0e76a5a0)

While enumerating port 8080, a login page powered by Jenkins was discovered. Different default login credentials were attempted, and a successful login was achieved with admin: admin.

After successfully logging in, navigation through various options was initiated. Upon reaching “Manage Jenkins,” various options were displayed, and when “Script Console” was selected, a location for executing Groovy scripts was found.

![image](https://github.com/user-attachments/assets/8ba03aae-04e1-4b29-9954-2d4e275d1ddb)

![image](https://github.com/user-attachments/assets/809f477d-1b64-437a-958d-73b1f98c6c08)

A Groovy script reverse shell script was searched for on Google and subsequently located. It was employed to obtain a reverse shell.

![image](https://github.com/user-attachments/assets/8f5f713e-5875-4e1c-bcad-413c050d6a08)

![image](https://github.com/user-attachments/assets/da967f3d-e12b-4c2f-b249-7d615a5202e7)


![image](https://github.com/user-attachments/assets/41355bdf-3451-4df2-874c-4e512942d595)


![image](https://github.com/user-attachments/assets/90e237ef-9b31-4c8e-b253-8cd13c3a7a60)

The Groovy reverse shell script was used to establish a reverse shell, and the LPORT was monitored on my attacker machine. A shell of the user “Bruce” was obtained.

![image](https://github.com/user-attachments/assets/c1b7c5c0-f552-4add-986d-617019f7fd9c)

![image](https://github.com/user-attachments/assets/ab06e2f7-39c9-45d9-b937-dba5fb412f12)

The user’s home directory in Windows is the user’s Desktop. Navigating to the Desktop of the user, I was able to find the user flag.

**Privielge escalation**
To make privilege escalation easy, a decision was made to switch to a Meterpreter shell. The process of creating a Meterpreter reverse shell was initiated in order to obtain a reverse shell.

![image](https://github.com/user-attachments/assets/05542ece-76a4-4190-98d9-4e1d5b78f7c5)

After the payload had been generated, the process of transferring the reverse shell from the attacker machine was initiated.

A Python server was created for file transfer from the location of the rev-shell.exe in the attacker machine.
![image](https://github.com/user-attachments/assets/92afa088-ddde-4ce5-837d-83fb451dcf72)

![image](https://github.com/user-attachments/assets/2997b86c-730f-4f53-8031-91162a28b500)

The tshell-name.exe was downloaded from the attacker's machine.

![image](https://github.com/user-attachments/assets/de9727e0-90fd-43b2-9e28-4f14e0595ec9)

The msfconsole was set up to listen for the reverse shell.

![image](https://github.com/user-attachments/assets/2b74841b-2799-4861-aec8-7998512e5d74)

![image](https://github.com/user-attachments/assets/3bd507ba-128d-4dd1-8ef8-bd8790c29e65)




