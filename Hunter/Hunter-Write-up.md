# Abdullah Ali Alhakami
# Twitter : @Alhakami1_
# Email : Aalhakami.26@gmail.com


We have been provide with a disk image , let's open it with FTK IMAGER 

![image](https://user-images.githubusercontent.com/99384019/157634182-98a1abf6-31be-407a-80ec-fb7369b362f3.png)


Now , Let's START ANSWERING THE QUESTIONS : )  

#1	What is the computer name of the suspect machine?

We can find the computer name in registry hive --> \SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName
So let's start with export the registry files and start analyzing it using Any registry analysis tool . I will use Eric Zimmerman's tool : Registry explorer .

We can find the registry file in --> Windows\System32\Config\

![image](https://user-images.githubusercontent.com/99384019/157635644-8bc64357-ae1a-4db6-b838-de4c53969c13.png)

let's export the registry files and save it . Then let's open it with the tool .

![image](https://user-images.githubusercontent.com/99384019/157636099-148b9ea5-e3b1-49b0-95a9-e09ebb978a93.png)

Now let's go and look to the hive that contain computer name 

![image](https://user-images.githubusercontent.com/99384019/157636297-88644649-e518-48a2-b0eb-b22cac7a114e.png)

ANSWER : 4ORENSICS

#2 What is the computer IP? 

We can find the computer ip in registry hive --> \HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{*}
let's go and get it .

![image](https://user-images.githubusercontent.com/99384019/157637347-57f6aecf-018a-4742-9222-706d2a343867.png)

ANSWER :  10.0.2.15

#3 What was the DHCP LeaseObtainedTime?

We can find the DHCP LeaseObtainedTime in the same hive we find the computer ip .

![image](https://user-images.githubusercontent.com/99384019/157637808-a137410a-ef3f-403e-8d60-573a7e18f9f9.png)

BUT , it's epoch time let's convert it to human readable using https://www.epochconverter.com/

![image](https://user-images.githubusercontent.com/99384019/157638242-0cb998a4-6648-462c-b7ed-44da784f341a.png)

ANSWER : 21/06/2016 02:24:12 UTC

#4 What is the computer SID?

We can find security identifier in registry file --> SECURITY\SAM\Domains\Account

![image](https://user-images.githubusercontent.com/99384019/157643546-2b5e0015-5cc3-495a-92c2-de68226ed2b6.png)

The key has to values: F and V . The V value is a binary value that has the computer SID embedded within it at the end of its data (last 96 bits).
Let's decode the Machine SID

The MACHINE SID encoded = 2E-D9-61-94-33-5A-2B-A4-80-82-5C-2A

First let's divide the bytes into 3 sections:

24 digit / 3 section = 8 digit per section .

2E-D9-61-94 , 33-5A-2B-A4 , 80-82-5C-2A

Now let's Reverse the order of bytes in each section

94-61-D9-2E , A4-2B-5A-33 , 2A-5C-82-80

Then let's convert each section to decimal.

2489440558 - 2754304563 - 710705792

Finally we are goint to add the machine SID prefix at the beginning : 

SID prefix = S-1-5-21

SID decoded = S-1-5-21-2489440558-2754304563-710705792

ANSWER : S-1-5-21-2489440558-2754304563-710705792

#5 What is the Operating System(OS) version? 

The os version we can found it in --> Software\Microsoft\Windows NT\CurrentVersion

![image](https://user-images.githubusercontent.com/99384019/157644857-6ee03006-ff30-4e86-bc81-f578b8938ffe.png)

ANSWER : 8.1

#6 What was the computer timezone? 

let's look at registry values name in --> SYSTEM\CurrentControlSet\Control\TimeZoneInformation

![image](https://user-images.githubusercontent.com/99384019/157645271-6cad5c07-da6b-4693-93b8-071b193901cc.png)

 Pacific Standard Time = UTC-07:00
 
 ANSWER = UTC-07:00
 
 #7 How many times did this user log on to the computer? 

To make it much easier we are going to use RegRipper tool for registry analysis . 
" You can solve the question manually if you want ."

![image](https://user-images.githubusercontent.com/99384019/157651301-caa4f831-1afa-4e84-83c3-5823d8d58f2c.png)

![image](https://user-images.githubusercontent.com/99384019/157652179-52990297-4d94-4805-b7c5-44914e679374.png)


Login count = 3

ANSWER : 3

#8 When was the last login time for the discovered account? Format: one-space between date and time

We will find it in the SAM registry file output file .

![image](https://user-images.githubusercontent.com/99384019/157652450-2ce617f3-085a-4b7a-81b8-c3f9ff944687.png)

Last Login Date : 2016-06-21 01:42:40Z

ANSWER : 2016-06-21 01:42:40

#9 There was a “Network Scanner” running on this computer, what was it? And when was the last time the suspect used it? Format: program.exe,YYYY-MM-DD HH:MM:SS UTC

since we are suspecios the employee , if he did it of course he going to unstall the tool so we look for the uninstall program and we found Nmap and he is system and since he is using windows so it's maybe GUI (We will look for some other artifact to support our theory) .

![image](https://user-images.githubusercontent.com/99384019/157659711-d526e706-555a-4eb9-9634-3259db93b443.png)


We can find uninstall in registry file : SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\
Tool : Zenmap.exe version 7.12

But , to get more details we should start analyze the Prefetch Files to help us to discover details about the programs was running on the system . 
We can find Prefetch file in Windows\Prefetch . Extract the file then start analyze it using Eric Zimmerman tool PECmd .

Now we can confirmed our theory the tool is zenmap 
![image](https://user-images.githubusercontent.com/99384019/157666990-dba33a0c-8e52-465a-a23f-c3f07189588c.png)

Using PECmd tool we got : 

![image](https://user-images.githubusercontent.com/99384019/157667905-25467a6b-8859-494d-9620-01506782f206.png)


ANSWER : zenmap.exe,2016-06-21 12:08:13 UTC

#10 When did the port scan end? (Example: Sat Jan 23 hh:mm:ss 2016)

There is a database stored in a file called zenmap.db in zenmap tool. By default, scans are kept in the database for 60 days and then removed. so let's check the database maybe we will get the answer for the question . 

PATH = /Users/Hunter/.zenmap

![image](https://user-images.githubusercontent.com/99384019/157669044-70aa80ad-5514-4d9e-bb84-0347a3121ae8.png)

let's open it with SQLite 

![image](https://user-images.githubusercontent.com/99384019/157669679-805ea16d-9764-4224-ae05-d32c292385ec.png)

ANSWER : Tue Jun 21 05:12:09 2016



