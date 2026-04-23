# Scenario Data Heist Write-Up:Hackviser

- The scenario is this:
- We have noticed that there are some employees in our company who are not
 sufficiently aware of cybersecurity issues. It has been observed that 
these employees have uploaded files containing company data to a website
 called "Exif Viewer" in order to view metadata information.
- The site claims that it does not store the uploaded files. However, we 
have to make sure that this is the case. If any of our company's files 
have been compromised in this process, we need to determine what 
information may be at risk. You are expected to guide us through this 
critical process. We rely on your expertise.
- Good luck!

- Looks interesting
- I just have a website at the moment
- It looks like this
- And in real life I saw websites like this website so this ctf looks very relastic.That’s good

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image.png)

- I wanna test the website
- Yeah it is really working
- I think I can upload something in the input field to be able to get a shell or to be able to reach the database
- We are gonna see what happens

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%201.png)

# 1-What is the path where the files are stored on the server?

- I wanna start with a nmap scan to understand the structure
- Firstly I wanna find the ip address of the website
- I’ll find it with ping command

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%202.png)

- I found the ip address of the website
- It is 172.20.25.136
- Let’s run a nmap command

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%203.png)

- 22 ssh and 80 http ports are open
- Right now I’m not able to use 22 ssh port because I don’t have any username and password information
- Let’s make a gobuster scan to find other directories in the website

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%204.png)

- I couldn’t find any useful information from this gobuster scan
- Let’s make another gobuster scan with another wordlist
- Right now I started a comprenshive gobuster scan because the first one was a little bit easy
- But from this gobuster scan I couldn’t find anything too
- I couldn’t find something useful with this gobuster scans
- Let’s try to find manual
- I tried to find for robots.txt,sitemap.txt,uploads,images,temp,files,static but none of them worked
- I tried to upload a .php file to the system
- It gives me informatipn but still says unsuported file type

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%205.png)

- Hmmmm
- I tried to bypass the unsupported file type.I used burpsuite and I bypassed it but I still couldn’t get any useful information
- I tried to find the path for 2 or 3 hours but I was not successful
- I always tried to make some local file inclusion and then I thought I can find the path
- But no.I was wrong.
- Then I relaized something
- I thought
- This exiftool is running command on the linux so if there is an exploit I can run command on linux so maybe a remote code execution
- Then if you look to the first line you can see the version of the exiftool
- So easy
- Let’s look to the exploit db

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%206.png)

- Our version is 12.23
- And this exploit is for 12.23
- Lets use it
- I searched the exploit with searchsploit and I downloaded it

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%207.png)

- I learned how to use the exploit.This is too much important
- If I use this command the exploit creates a  image.jpg file and it runs pwd command because I specified it

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%208.png)

- Then if I upload that image.jpg file to the system then I can make lots of things because the payload is gonna work
- Then we can see the anwer on the screen
- For example

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%209.png)

- We want to find the path of downloaded files to the system
- Then we have to use this command

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2010.png)

- Now we can upload the image.jpg file to the system
- Then we can see the path

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2011.png)

- Yeah we can see the path obviously
- If we upload a file to the this website that file goes to the this path
- The answer found for the first question

# 2-What is the e-mail address and password of an employee of the company "waltersltd" from the data contained in the stored files?

- Before start to second question let’s take a reverse shell because I cannot create new image.jpg file everytime because it is not useful
- I tried to get a reverse shell
- I took help from gemini to get a reverse shell
- Firstly I started a listener with netcat

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2012.png)

- Firstly I tried to create that payload normally but it didn’t work then gemini told me to make this with base64 encoding
- Then I made those operations

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2013.png)

- Then I created a payload with the exploit

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2014.png)

- I uploaded the image.jpg file to the system and
- Now I have a shell in my computer

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2015.png)

- I started to investigate in the shell

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2016.png)

- As we can see from the picture above there are a .csv file named users
- I think the answer of the question is in that file
- Let’s read it

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2017.png)

- The answer is obvious

# 3-What is the invoice number of an invoice found in the stored files?

- Let’s contiune to investigate

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2018.png)

- There is a pdf file named invoice
- I think the answer of the question that I’m looking for in this file
- But I cannot open the .pdf file in here
- Because of that I’ll create a http server in here then I’ll read the pdf in my computer
- I started the http server

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2019.png)

- Then I connected it in my computer
- So we can read the invoice number

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2020.png)

# 4-What is the database connection address of the files contained in the stored files?

- Let’s continue to the invesitgation
- There is a file named database.go

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2021.png)

- Let’s open that file maybe we can find the answer of the question
- Yeah I found the answer of the question

![image.png](Scenario%20Data%20Heist%20Write-Up%20Hackviser/image%2022.png)

- The ctf is over.