# Abdullah Ali Alhakami
# Twitter : @Alhakami1_
# Email : Aalhakami.26@gmail.com

![image](https://user-images.githubusercontent.com/99384019/165411019-c0a95b48-5d47-458c-a004-1063dbab4e89.png)

![image](https://user-images.githubusercontent.com/99384019/157856624-2338eee7-2f0f-4713-87fb-b5365ee4dc78.png)

**************************************************************************************************************************************************************************

The challenge provide us with a Windows disk image .. let's go ahead and open it with FTK imager .

![image](https://user-images.githubusercontent.com/99384019/165411393-c741e97f-0054-4f5c-9af3-d752f903961f.png)


Now Let's start answering the questions . 

**************************************************************************************************************************************************************************

#1 What is the MD5 hash value of the suspect disk?

There are two ways to solve this question , i'll show you it both to you .

First approach : 

The challenge provide us with a txt file that has the information when the disk was acquire .. let's go and check the text file :

![image](https://user-images.githubusercontent.com/99384019/165412066-5a85b665-0366-4f23-a0dd-0592d54e8f19.png)

We can observe that computer hash = 9471e69c95d8909ae60ddff30d50ffa1

Second approach : 

We can use a feature in FTK imager that will calculate for us the MD5 hash .. 

![image](https://user-images.githubusercontent.com/99384019/165412716-776824bb-4b59-484a-846d-dd571d71e77c.png)

![image](https://user-images.githubusercontent.com/99384019/165412802-65f92aab-575b-48e6-a8be-ce2a7a6839b2.png)


ANSWER : 9471e69c95d8909ae60ddff30d50ffa1


**************************************************************************************************************************************************************************

#2 What phrase did the suspect search for on 2021-04-29 18:17:38 UTC? (three words, two spaces in between)

To get the search history of the chrome browser we should know how chrome browser store the data ? 

![image](https://user-images.githubusercontent.com/99384019/165420460-0007b722-aba3-49c6-ba76-e2dace8aa987.png)

path : C:\Users\<username>\AppData\Local\Google\Chrome\User Data\Default ---> DB file 

![image](https://user-images.githubusercontent.com/99384019/165420530-22a32da5-3776-450d-8801-aa537c3b11e2.png)


Let's export the DB file and start analysis using DB Browser (SQLite) : 

![image](https://user-images.githubusercontent.com/99384019/165420648-886b78ab-4cd5-431f-88da-54ddc6dc8b38.png)

How do i know the time ? Good question ! Using DCode tool or any online stie that will convert the epoch to human readable time .. 

![image](https://user-images.githubusercontent.com/99384019/165421048-1cc03340-56b8-4d0b-a45e-8eb63dae9289.png)

Answer : password cracking lists


**************************************************************************************************************************************************************************

#3 What is the IPv4 address of the FTP server the suspect connected to?

While analyzing the disk we figure out there is a folder in this path \Users\John Doe\AppData\Roaming\FileZilla\ named recentservers.xml
let's see the xml file

![image](https://user-images.githubusercontent.com/99384019/165662744-a5ef7dd5-cfc3-420b-a419-43c6644d78f1.png)



ANSWER : 192.168.1.20

**************************************************************************************************************************************************************************

#4 What date and time was a password list deleted in UTC? (YYYY-MM-DD HH:MM:SS UTC)

Let's check the $RecycleBin folder and export the information file for the password list , then we will use Eric Zimmerman Tool RBCmd :

![image](https://user-images.githubusercontent.com/99384019/165428528-fb5204f8-4186-4821-9d66-322ad5184d01.png)

ANSWER : 2021-04-29 18:22:17 UTC 

**************************************************************************************************************************************************************************


#5 How many times was Tor Browser ran on the suspect's computer? (number only)

Let's check the Prefetch Files for Tor Browser .. Path : %SystemRoot%\Prefetch\

There is only a prefetch file for the Setup & THERE IS NO PREFETCH FILE FOR EXECUTING THE TOR BROWSER

![image](https://user-images.githubusercontent.com/99384019/165429244-cda13b16-0a17-4a2e-85ac-044b272670ae.png)

ANSWER : 0


**************************************************************************************************************************************************************************

#6 What is the suspect's email address?

While analyzing the history of the chrome browser we found out his email address : 

![image](https://user-images.githubusercontent.com/99384019/165622682-12600fef-b325-4cde-8d94-f12d711041e2.png)

Answer : dreammaker82@protonmail.com

**************************************************************************************************************************************************************************

#7 What is the FQDN did the suspect port scan?

What does FQDN stand for ? Fully qualified domain name 

Okay after analyzing the disk we notice that he use nmap tool for scanning (CHECK HIS DESKTOP) , So let's go and check nmap file (Because nmap store the history of the scan search in db file) , BUT we colud not find it ! 
WHY ? 
After a basic analysis on the uninstall programs we found out that the suspect uninstalled the nmap tool therefor the files of nmap has been deleted .

![image](https://user-images.githubusercontent.com/99384019/165624350-39ae529d-6b93-40ce-b2f0-c125af8c5e4a.png)

The good thing we can go and check the powershell history right ?

From C:\Users\<username>\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt

![image](https://user-images.githubusercontent.com/99384019/165628485-237bf66b-6f3e-4066-b39e-274238b95b5e.png)

Answer : dfir.science



**************************************************************************************************************************************************************************

#8 What country was picture "20210429_152043.jpg" allegedly taken in?

We can find the photo in (Pictures/Contact) 
Let's extract the photo and try to find the meta data using Exiftool tool 

![image](https://user-images.githubusercontent.com/99384019/165632243-82b3e539-9dc5-4f69-a233-8a0d529e0a69.png)

Woow these seem instersting , am i right ?
let's find out where does these GPS Coordinates lead us .. 

![image](https://user-images.githubusercontent.com/99384019/165632651-abd23f75-fba8-4fa5-b5ed-2cd74b29f0ee.png)

Answer : Zambia

**************************************************************************************************************************************************************************

#9 What is the parent folder name picture "20210429_151535.jpg" was in before the suspect copy it to "contact" folder on his desktop?


First let's extract the photo and see the meta data , we will notice the photo was taken using LG mobile phone .

So let's start shellbags analysis .
1- Extract the registry hive Usrclass.dat from Users\John Doe\AppData\Local\Microsoft\Windows\Usrclass.dat
2- use Eric ShellBagsExplorer & load the registry hive .

![image](https://user-images.githubusercontent.com/99384019/165636336-55757208-6513-4553-998f-d0c761b54a94.png)

Answer : Camera

**************************************************************************************************************************************************************************

#10	A Windows password hashes for an account are below. What is the user's password? Anon:1001:aad3b435b51404eeaad3b435b51404ee:3DE1A36F6DDB8E036DFD75E8E20C4AF4:::

We can know from the hash this is how a windows store the users password 
NAME - LM HASH - NTLM HASH OF THE USER PASSWORD

let's try to crack the PASSWORD using hashcat or john the ripper or we can use onlinehashcrack.com 
3DE1A36F6DDB8E036DFD75E8E20C4AF4 = AFR1CA!

ANSWER :AFR1CA!

**************************************************************************************************************************************************************************

#11	What is the user "John Doe's" Windows login password?

To solve the question we should know where the windows store the users password ? In a registry hive SAM & SYSTEM so we are going to use a tool called samdump which is installed by defualt in linux .

The password hash for John Does's = ecf53750b76cc9a62057ca85ff4c850e

using hashcat or john the ripper or any hash cracking tool or website we will get the password .

![image](https://user-images.githubusercontent.com/99384019/165662364-562d393b-1a31-4e95-95b9-f0858b90ef9c.png)

Answer : ctf2021
