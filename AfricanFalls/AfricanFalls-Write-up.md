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








