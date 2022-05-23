# Abdullah Ali Alhakami
# Twitter : @Alhakami1_
# Email : Aalhakami.26@gmail.com

![image](https://user-images.githubusercontent.com/99384019/169725451-337e9962-1e84-48e3-9671-4375e2868f3c.png)

![image](https://user-images.githubusercontent.com/99384019/157856624-2338eee7-2f0f-4713-87fb-b5365ee4dc78.png)

**************************************************************************************************************************************************************************

Scenario:
One of the SOC analysts took a memory dump from a machine infected with a meterpreter malware. As a Digital Forensicators, your job is to analyze the dump, extract the available indicators of compromise (IOCs) and answer the provided questions.



We have been provided with a memory dump, let's start analyzing it using Volatility. 


Now, Let's START ANSWERING THE QUESTIONS : )  


**************************************************************************************************************************************************************************


#1 What is the SHA1 hash of Triage-Memory.mem (memory dump)?
Why we are calculating the SHA1 hash ? because the integrity of the memory dump .

![image](https://user-images.githubusercontent.com/99384019/169726777-b1d77331-86b8-4c58-8a54-af3108371353.png)

ANSWER : c95e8cc8c946f95a109ea8e47a6800de10a27abd


*************************************************************************************************************************************************************************


#2 What volatility profile is the most appropriate for this machine? (ex: Win10x86_14393)?

let's use a plugin in volatility called "imageinfo" to get information about the memory dump such as suggesting operating system . 

![image](https://user-images.githubusercontent.com/99384019/169727112-cb176136-7201-4129-ac09-8d75bfb865f9.png)


ANSWER : Win7SP1x64

*************************************************************************************************************************************************************************

#3 What was the process ID of notepad.exe?

Let's see the running process using pslist(will show us the running process) plugin .

![image](https://user-images.githubusercontent.com/99384019/169727321-98ae4f0a-0753-4a02-b1ec-42051cbc7bca.png)


![image](https://user-images.githubusercontent.com/99384019/169727295-9c98faaf-e238-4c17-b413-f8cb08f463fb.png)


ANSWER : 3032

*************************************************************************************************************************************************************************

#4 Name the child process of wscript.exe. ? 

To solve this question we will use pstree plugin to show us the relation between process . 

![image](https://user-images.githubusercontent.com/99384019/169727838-0236356a-fd66-400c-a821-3942c90a364f.png)


![image](https://user-images.githubusercontent.com/99384019/169727806-d9f29cd1-0457-4035-8d30-848d05102869.png)

ANSWER :  UWkpjFjDzM.exe


*************************************************************************************************************************************************************************

#5 What was the IP address of the machine at the time the RAM dump was created?

Using netscan plugin we can see the local ip address 

![image](https://user-images.githubusercontent.com/99384019/169729997-d2d42c9f-2545-4224-a39a-103306c362d5.png)

ANSWER : 10.0.0.101


*************************************************************************************************************************************************************************

#6 Based on the answer regarding the infected PID, can you determine the IP of the attacker?

Using the same plugin netscan to show us all the connection we find out the infected process esthablish a connection and the attacker ip was listining in port 4444 

![image](https://user-images.githubusercontent.com/99384019/169730268-2ca5cc31-f59d-400c-902c-eb47834f81e6.png)

ANSWER : 10.0.0.106

*************************************************************************************************************************************************************************

#7 How many processes are associated with VCRUNTIME140.dll?

Using dlllist plugin to show us the running dlls in the memory .
What is dll ? Compesed of code that can be used in multiple program

![image](https://user-images.githubusercontent.com/99384019/169730465-f71ece7e-afe3-4fd8-9365-b7812bf85f87.png)

ANSWER : 5

*************************************************************************************************************************************************************************

#8 After dumping the infected process, what is its md5 hash?

To dump a process we have to use a plugin called procdump , The infected process we already knew (THE PROCESS THAT ESTABLISH THE CONNECTION IN PORT 4444)

![image](https://user-images.githubusercontent.com/99384019/169733935-e12cc1f7-165b-4806-98b6-7a07c6b7dfe8.png)

To make sure it is the infected process let's drop it in virustotal .

![image](https://user-images.githubusercontent.com/99384019/169734047-bf854151-8ee5-4c3a-b52c-50349f13fce4.png)


![image](https://user-images.githubusercontent.com/99384019/169734129-3987f8a6-80c5-413f-99d8-f719d8ac9ef5.png)

ANSWER : 690ea20bc3bdfb328e23005d9a80c290

*************************************************************************************************************************************************************************

#9 What is the LM hash of Bob's account?

Using a plugin called hashdump will show us the password for all the user's in the machine encrypted .

![image](https://user-images.githubusercontent.com/99384019/169734569-0f098b88-9fb5-458a-bbe2-fa59f9f3312f.png)

ANSWER :  aad3b435b51404eeaad3b435b51404ee

*************************************************************************************************************************************************************************

#10	What memory protection constants does the VAD node at 0xfffffa800577ba10 have?

First what is VAD ? VAD stand for virtual address descriptor .

What is VAD node ? 

The VAD tree : 
VAD tree can provide useful information to an investigator such as the pattern of memory allocations used by a process and files mapped into its virtual address space.

![image](https://user-images.githubusercontent.com/99384019/169735516-691fd551-63e4-40d2-95d0-a6f201b84bb7.png)


![image](https://user-images.githubusercontent.com/99384019/169735463-a9871c26-2b2f-46ea-ba4f-1add37ce5d60.png)

for more details:https://www.sciencedirect.com/science/article/pii/S1742287607000503#:~:text=The%20Virtual%20Address%20Descriptor%20tree,entry%20in%20the%20VAD%20tree.


Using a plugin called vadinfo we can see Protection 

![image](https://user-images.githubusercontent.com/99384019/169735380-6ff73e55-9050-4d91-be47-85cb1acc75d3.png)

ANSWER : PAGE_READONLY

*************************************************************************************************************************************************************************

#11	What memory protection did the VAD starting at 0x00000000033c0000 and ending at 0x00000000033dffff have?

As same as the prevouse question .

![image](https://user-images.githubusercontent.com/99384019/169736174-53aee4e1-0084-42e3-b171-20ab1e9f2c7f.png)


ANSWER : PAGE_NOACCESS

*************************************************************************************************************************************************************************

#12	There was a VBS script that ran on the machine. What is the name of the script? (submit without file extension)?

Since the script had ran on the machine. So let's use cmdline plugin to show us the command that ran .

![image](https://user-images.githubusercontent.com/99384019/169830394-822bef3a-f618-42e0-8dbf-bf73a0913d4a.png)

We can see the process id is 5116 which means the process is the parent process of the infected one .

ANSWER : vhjReUDEuumrX

*************************************************************************************************************************************************************************

#13	An application was run at 2019-03-07 23:06:58 UTC. What is the name of the program? (Include extension)

We are going to use timeliner plugin then we look for this time 

![image](https://user-images.githubusercontent.com/99384019/169834913-b32b0ec3-72d5-44d5-96ba-bc4024be4875.png)

ANSWER : SKYPE.exe

*************************************************************************************************************************************************************************

#14 What was written in notepad.exe at the time when the memory dump was captured?

First we will dump the process then we are going to use strings tool 

![image](https://user-images.githubusercontent.com/99384019/169841801-5d1f3655-1399-43da-a23c-2966fd98e1ee.png)

strings -e l using this option because notepad stores text in the little-endian format

![image](https://user-images.githubusercontent.com/99384019/169842874-8c1ef8d2-7f8d-4922-acdb-a6ed0f65b4aa.png)

ANSWER : flag<REDBULL_IS_LIFE>


*************************************************************************************************************************************************************************

#15	What is the short name of the file at file record 59045?

Since this is a NTFS so we are going to dump the Master File Table using mftparser plugin , then we will search for a file record number = 59045

![image](https://user-images.githubusercontent.com/99384019/169839808-7e67b26a-5324-48a0-b689-826fc6763acf.png)

ANSWER :  EMPLOY~1.XLS

*************************************************************************************************************************************************************************

#16 This box was exploited and is running meterpreter. What was the infected PID?

definitely, we know the infected process ,it is the process we previously scan it in virustotal .

ANSWER : 3496

**************************************************************************************************************************************************************************

![image](https://user-images.githubusercontent.com/99384019/157884931-813a515c-189a-42f3-a826-7042fef8ddc1.png)
