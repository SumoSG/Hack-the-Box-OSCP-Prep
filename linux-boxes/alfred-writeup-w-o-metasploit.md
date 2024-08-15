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






