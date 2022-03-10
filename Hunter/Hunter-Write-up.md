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
