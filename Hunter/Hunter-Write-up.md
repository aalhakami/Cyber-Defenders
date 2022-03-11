# Abdullah Ali Alhakami
# Twitter : @Alhakami1_
# Email : Aalhakami.26@gmail.com

![image](https://user-images.githubusercontent.com/99384019/157856502-23d649da-6070-4390-a669-3b861ce7892a.png)

![image](https://user-images.githubusercontent.com/99384019/157856624-2338eee7-2f0f-4713-87fb-b5365ee4dc78.png)

**************************************************************************************************************************************************************************

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


**************************************************************************************************************************************************************************


#2 What is the computer IP? 

We can find the computer ip in registry hive --> \HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces\{*}
let's go and get it .

![image](https://user-images.githubusercontent.com/99384019/157637347-57f6aecf-018a-4742-9222-706d2a343867.png)

ANSWER :  10.0.2.15


**************************************************************************************************************************************************************************


#3 What was the DHCP LeaseObtainedTime?

We can find the DHCP LeaseObtainedTime in the same hive we find the computer ip .

![image](https://user-images.githubusercontent.com/99384019/157637808-a137410a-ef3f-403e-8d60-573a7e18f9f9.png)

BUT , it's epoch time let's convert it to human readable using https://www.epochconverter.com/

![image](https://user-images.githubusercontent.com/99384019/157638242-0cb998a4-6648-462c-b7ed-44da784f341a.png)

ANSWER : 21/06/2016 02:24:12 UTC


**************************************************************************************************************************************************************************


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


**************************************************************************************************************************************************************************



#5 What is the Operating System(OS) version? 

The os version we can found it in --> Software\Microsoft\Windows NT\CurrentVersion

![image](https://user-images.githubusercontent.com/99384019/157644857-6ee03006-ff30-4e86-bc81-f578b8938ffe.png)

ANSWER : 8.1


**************************************************************************************************************************************************************************



#6 What was the computer timezone? 

let's look at registry values name in --> SYSTEM\CurrentControlSet\Control\TimeZoneInformation

![image](https://user-images.githubusercontent.com/99384019/157645271-6cad5c07-da6b-4693-93b8-071b193901cc.png)

 Pacific Standard Time = UTC-07:00
 
 ANSWER = UTC-07:00


**************************************************************************************************************************************************************************


 
 #7 How many times did this user log on to the computer? 

To make it much easier we are going to use RegRipper tool for registry analysis . 
" You can solve the question manually if you want ."

![image](https://user-images.githubusercontent.com/99384019/157651301-caa4f831-1afa-4e84-83c3-5823d8d58f2c.png)

![image](https://user-images.githubusercontent.com/99384019/157652179-52990297-4d94-4805-b7c5-44914e679374.png)


Login count = 3

ANSWER : 3


**************************************************************************************************************************************************************************



#8 When was the last login time for the discovered account? Format: one-space between date and time

We will find it in the SAM registry file output file .

![image](https://user-images.githubusercontent.com/99384019/157652450-2ce617f3-085a-4b7a-81b8-c3f9ff944687.png)

Last Login Date : 2016-06-21 01:42:40Z

ANSWER : 2016-06-21 01:42:40


**************************************************************************************************************************************************************************



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


**************************************************************************************************************************************************************************



#10 When did the port scan end? (Example: Sat Jan 23 hh:mm:ss 2016)

There is a database stored in a file called zenmap.db in zenmap tool. By default, scans are kept in the database for 60 days and then removed. so let's check the database maybe we will get the answer for the question . 

PATH = /Users/Hunter/.zenmap

![image](https://user-images.githubusercontent.com/99384019/157669044-70aa80ad-5514-4d9e-bb84-0347a3121ae8.png)

let's open it with SQLite 

![image](https://user-images.githubusercontent.com/99384019/157669679-805ea16d-9764-4224-ae05-d32c292385ec.png)

ANSWER : Tue Jun 21 05:12:09 2016


**************************************************************************************************************************************************************************



#11 How many ports were scanned?

Following-up with the previous question we found the number of scanning ports :

![image](https://user-images.githubusercontent.com/99384019/157670914-2733aa94-a735-467d-ac12-97e290b2b92a.png)

ANSWER = 1000


**************************************************************************************************************************************************************************



#12 What ports were found "open"?(comma-separated, ascending)

Following-up with the previous question we can found the open ports by seeing the output :

![image](https://user-images.githubusercontent.com/99384019/157672238-df9ca365-2b44-49cd-857b-d32885e67219.png)

![image](https://user-images.githubusercontent.com/99384019/157672398-05f9e468-4951-4454-8731-8ef98a5ad5ac.png)

ANSWER : 22,80,9929,31337


**************************************************************************************************************************************************************************



#13 What was the version of the network scanner running on this computer?

We already discovered this answer in question number 9 .

version 7.12

ANSWER : 7.12


**************************************************************************************************************************************************************************



#14 The employee engaged in a Skype conversation with someone. What is the skype username of the other party?

When the investigation come to skype there is a powerfull tool we are going to use called skyperious .
First let's extract all the evidnce that related to skype so we can import it to the tool .We can find it in AppData where does information stored such as conversations and other details , Then Roaming .

![image](https://user-images.githubusercontent.com/99384019/157699850-6f0a930c-6445-49b1-9e42-005438275877.png)

Now let's run the tool and import the file . let's explore main.db 

![image](https://user-images.githubusercontent.com/99384019/157700650-eb97e335-0854-4674-ac86-0fd28f881b0e.png)

Skype username of the other party is : Linux rul3z (linux-rul3z)

ANSWER : linux-rul3z


**************************************************************************************************************************************************************************



#15 What is the name of the application both parties agreed to use to exfiltrate data and provide remote access for the external attacker in their Skype conversation?

While analyzing the chat i found the application which they agreed to use it .

![image](https://user-images.githubusercontent.com/99384019/157701919-2c155018-1e8d-4ab1-99bf-33c2c57b319d.png)


**************************************************************************************************************************************************************************



#16 What is the Gmail email address of the suspect employee?

![image](https://user-images.githubusercontent.com/99384019/157702622-7c234c86-c6f2-4f5e-b8c3-0a6b43387e7b.png)

ANSWER :  ehptmsgs@gmail.com


**************************************************************************************************************************************************************************



#17 It looks like the suspect user deleted an important diagram after his conversation with the external attacker. What is the file name of the deleted diagram?

While analyzing the chat we found : 

![image](https://user-images.githubusercontent.com/99384019/157705348-627edc92-2ac6-452e-ab12-7fc9ce1d1abf.png)

so it's obviously the diagram has send via outlook . let's check outlook file .  

![image](https://user-images.githubusercontent.com/99384019/157705971-d0e26aa5-988a-44c7-8001-0e044ab20dc4.png)

There is a backup folder with extension .pst let's open it using online pst viewer --> https://goldfynch.com/pst-viewer/

![image](https://user-images.githubusercontent.com/99384019/157706764-a6b185fc-f74b-4767-8a5b-9e826b54e2cb.png)

ANSWER : home-network-design-networking-for-a-single-family-home-case-house-arkko-1433-x-792.jpg


**************************************************************************************************************************************************************************



#18 The user Documents' directory contained a PDF file discussing data exfiltration techniques. What is the name of the file?

This is a gift qeustion thank you ali alhadi

![image](https://user-images.githubusercontent.com/99384019/157707431-5d5dac6a-2380-4456-ad4c-51bcf3d296d8.png)


ANSER : Ryan_VanAntwerp_thesis.pdf


**************************************************************************************************************************************************************************



#19 What was the name of the Disk Encryption application Installed on the victim system? (two words space separated)

After searching the whole disk , i could not find anything the good thing that i notes a program called BCWipe. BCWipe is designed to surgically delete specific data files without harming the hard drive; to erase all contents of whole hard drives . We found a log file let's search to the deleted program it has . 

![image](https://user-images.githubusercontent.com/99384019/157709913-1111c53e-3b53-4cd0-af9f-4b8103f2327c.png)

![image](https://user-images.githubusercontent.com/99384019/157709947-f694fd40-8dc7-47d7-9f4a-93e8063e397b.png)

ANSWER : Crypto Swap.lnk


**************************************************************************************************************************************************************************



#20 What are the serial numbers of the two identified USB storage?

We can find the identfied USB in registry key : \SYSTEM\CURRENTCONTROLSET\ENUM\USBSTOR

![image](https://user-images.githubusercontent.com/99384019/157855058-22b4e8f4-0b7a-4b46-99b3-87601bb9498b.png)


ANSWER = 07B20C03C80830A9,AAI6UXDKZDV8E9OU


**************************************************************************************************************************************************************************


#21 One of the installed applications is a file shredder. What is the name of the application? (two words space separated)

We already discovered this answer in question number 19 . 

![image](https://user-images.githubusercontent.com/99384019/157857827-6d4772a0-217d-4621-9e97-02019a294d0b.png)

ANSWER : jetico bcwipe


**************************************************************************************************************************************************************************


#22 How many prefetch files were discovered on the system?

let's extract prefetch file and open the WinPrefetchView . 

![image](https://user-images.githubusercontent.com/99384019/157859278-07e61635-b72a-4ee1-8c8a-2478a46ca2f1.png)

Answer = 174


**************************************************************************************************************************************************************************


#23 How many times was the file shredder application executed?

First lets export the prefetch file of the BCwipe.exe and use Eric Zimmerman tool PECmd 

![image](https://user-images.githubusercontent.com/99384019/157860497-fda1156c-d7dc-4ce0-a0a8-e03ee090177f.png)

Answer = 5


**************************************************************************************************************************************************************************


#24 Using prefetch, determine when was the last time ZENMAP.EXE-56B17C4C.pf was executed?

let's extract the ZENMAP.EXE-56B17C4C.pf prefetch file and use PECmd tool 

![image](https://user-images.githubusercontent.com/99384019/157861266-7a5d8c2b-e9f2-49cc-8f97-bb520e91bb1e.png)

ANSWER : 06/21/2016 12:08:13 PM


**************************************************************************************************************************************************************************


#25 A JAR file for an offensive traffic manipulation tool was executed. What is the absolute path of the file?

Let's check Downloads folder .

![image](https://user-images.githubusercontent.com/99384019/157863633-f2d84697-8756-4e4d-9a08-d64c328df5cf.png)

ANSWER :  C:\Users\Hunter\Downloads\burpsuite_free_v1.7.03.jar


**************************************************************************************************************************************************************************


#26 The suspect employee tried to exfiltrate data by sending it as an email attachment. What is the name of the suspected attachment?

Going back to question #17 let's see the sent mail we find 

![image](https://user-images.githubusercontent.com/99384019/157864273-0f5065b4-1cf3-4340-98f7-7d09e1d4bdc4.png)

ANSWER :  Pictures.7z


**************************************************************************************************************************************************************************


#27 Shellbags shows that the employee created a folder to include all the data he will exfiltrate. What is the full path of that folder?

FIRST let's export the NTUSER.DAT AS WELL AS UsrCLASS.DAT but we could not find NTUSER.DAT so we will export only UsrCLASS.DAT then we will use Eric Zimmerman tool ShellBags Explorer .


C:/Users/Hunter/AppData/Local/Microsoft/Windows

![image](https://user-images.githubusercontent.com/99384019/157878704-7b701981-bc47-4978-99cc-d4bdbe87c8f6.png)

Then let's open the tool and load the hive .

![image](https://user-images.githubusercontent.com/99384019/157879065-8fb0ea73-653c-4192-8162-ce4a11546de5.png)

![image](https://user-images.githubusercontent.com/99384019/157879838-b532f10a-7592-4568-b6a6-af7f0060755d.png)

ANSWER : C:\Users\Hunter\Pictures\Exfil


**************************************************************************************************************************************************************************


#28 The user deleted two JPG files from the system and moved them to $Recycle-Bin. What is the file name that has the resolution of 1920x1200?

![image](https://user-images.githubusercontent.com/99384019/157866040-4aad4e8a-2d5b-4540-8c23-17586f39aa12.png)

ANSWER :  ws_Small_cute_kitty_1920x1200.jpg


**************************************************************************************************************************************************************************


#29 Provide the name of the directory where information about jump lists items (created automatically by the system) is stored?

ANSWER :  AutomaticDestinations


**************************************************************************************************************************************************************************


#30 Using JUMP LIST analysis, provide the full path of the application with the AppID of "aa28770954eaeaaa" used to bypass network security monitoring controls.

Know let's export the JumpList items for this specifc AppID aa28770954eaeaaa 

![image](https://user-images.githubusercontent.com/99384019/157883905-ed18a1fd-d968-4603-b66d-0519e477bf64.png)


Now let's use Eric Zimmerman tool JumpList Explorer and start looking for the path	

![image](https://user-images.githubusercontent.com/99384019/157884270-7345ea66-b4e9-41b0-9092-9b1bce21d067.png)

ANSWER : C:\Users\Hunter\Desktop\Tor Browser\Browser\firefox.exe

**************************************************************************************************************************************************************************

![image](https://user-images.githubusercontent.com/99384019/157884931-813a515c-189a-42f3-a826-7042fef8ddc1.png)
