---
layout: post
title: "Building a Vulnerable Active Directory for Pen testing"
date: 2026-10-23
tech: ["Cybersecurity", "Active_Directory", "Pentesting", "Lab"]
categories: [Projects]
featured: true
---

# Building a Vulnerable Active Directory Lab for Penetration Testing

Active Directory is Microsoft’s directory service. It stores a company network’s users, computers, groups and policies, and handles authentication (Kerberos/NTLM) and authorization. Think of it as the phone book of a company network, plus the front desk that decides who gets in.”

Now if you want to practice Penetration Testing for Active Directory you can follow this build 

I am basically installing this due to the course from TCM Security and I think it would be amazing and really a good skill to put it in my arsenal I’ll go according to the course and step by step I apologize in advance for the screenshots which may or may not be available due to my computer being utilized by the vm.

![AD Lab Topology.svg](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/AD-Lab-Topology.svg)
A Windows Server 2022 domain controller and two Windows 11 workstations joined to MARVEL.local, plus a Kali Linux machine for pentesting, all on one VMware NAT network.

All the Process from Installing to connecting the workstations to the domain is divided into 6 topics to ensure ease of understanding.

<aside>
⚠️ **Everything in this lab is intentionally insecure.** Weak passwords, a password in a user description, Defender turned off and an open file share are here on purpose so there is something to attack. Never copy these settings into a real environment. Steps tagged **🧪 Lab only** are the risky ones.
</aside>

1. Lab Overview and Requirements
2. Downloading necessary ISOs
3. Setting up Domain Controller
4. Setting up User Machines
5. Setting up Users, Groups, and Policies
6. Joining Machines to the Domain.

## 1. Lab Overview and Requirements

The lab requirements for the course is around 2022 ( Verify the requirements ) but since the time passed so long and the current AD I am building is of the following requirements

Note: Feel free to try the new versions of the Windows server and the workstations if you are looking this Installation after 2026 or when the new versions come out.

| VM name | Role | OS | RAM | Disk |
| --- | --- | --- | --- | --- |
| HYDRA-DC | Domain Controller | Windows Server 2022 | 2 GB | 60 GB |
| THE-PUNISHER | Workstation 1 | Windows 11 Enterprise | 2 GB | 60 GB |
| SPIDERMAN | Workstation 2 | Windows 11 Enterprise | 2 GB | 60 GB |

### Cloud Alternative

If you don’t have the required specifications you can use the cloud version which costs money but you can follow another article from “Kamran Bilgrami” ([https://kamran-bilgrami.medium.com/ethical-hacking-lessons-building-free-active-directory-lab-in-azure-6c67a7eddd7f](https://kamran-bilgrami.medium.com/ethical-hacking-lessons-building-free-active-directory-lab-in-azure-6c67a7eddd7f))

## 2. Downloading ISO Files

1. Windows Server 2022: [https://info.microsoft.com/ww-landing-windows-server-2022.html](https://info.microsoft.com/ww-landing-windows-server-2022.html)

(You can fill in anything in the “Registration Free Trial” Section” because we are doing this in local and not in online)

1. Windows 11 Entreprise: [https://info.microsoft.com/ww-landing-windows-11-enterprise.html](https://info.microsoft.com/ww-landing-windows-11-enterprise.html)

(Same as the last time you can fill anything in the Registration section because we are doing this in local and not online)

## 3. Setting up Domain Controller

1. After Downloading the files, open up Vmware.
    
    ![Screenshot 2026-10-14 at 7.31.29 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-14_at_7.31.29_PM.png)
    
2. Click on “Create a New Virtual Machine”.
    
    ![Capture 1.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_1.png)
    
3. Next Click on “Typical” and click on next.
    
    ![Screenshot 2026-10-14 at 7.38.59 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-14_at_7.38.59_PM.png)
    
4. Browse the Windows Server ISO file (Which should like the SERVER_EVAL_x64FRE_en-us.iso) and click on next.
    
    ![Screenshot 2026-10-16 at 7.15.36 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_7.15.36_PM.png)
    
5. Since it was not able to detect the machine through the iso file we manually can give the version using the dropbox and Select Version 2022 (Note: The screenshot which I took was from VMware 15.5 PRO version and not the latest version, but in the latest version this part won’t be present and instead it would detect automatically if not then just give the version as “Windows Server 2022” or whatever version you are installing)
    
    ![Screenshot 2026-10-16 at 7.22.56 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_7.22.56_PM.png)
    
6. After that give the name for the “Virtual Machine” which is going to be “Server Manager 2022” and you can put any location (Which has more than 60 GB).
    
    ![Capture 4.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_4.png)
    
7. The next part is of encryption, this also gives the VM a virtual TPM, which Windows 11 needs later. which you can enter any password you want but do sure to remember it and click on next.
    
    ![Screenshot 2026-10-16 at 7.23.27 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_7.23.27_PM.png)
    
8. When Specifying the disk capacity be sure to mention 60 gb and then click on “split the disk” and then click on next. (Note this will just reserved the 60GB on your disk and incrementally use the disks)
    
    ![Screenshot 2026-10-16 at 7.31.44 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_7.31.44_PM.png)
    
9. Now after finishing the setup click on “Edit this virtual machine” and then you have to change the “Memory” since I have 16gb of ram, I first allotted as 4GB ram and then after setting up the Domain Controller I changed it back to 2gb ram because we are not going to use domain controller as much and set the network adapter to “NAT”.
10. One more additional thing is that if you are downloading the older version of “Windows Server ISO” then a “Floppy Disk” option will present in the left panel or in the “Hard Disk” Section which you have to remove it or disable it and then Power on the virtual machine.
    
    ![Screenshot 2026-10-16 at 8.56.17 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_8.56.17_PM.png)
    
11. Once turned on make sure to press any key (Literally any key) otherwise it will boot in a different type and then you have to restart the machine.
    
    ![Screenshot 2026-10-16 at 9.19.16 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.19.16_PM.png)
    
12. Once pressed the any button, you have to setup your preferred language and click on next.
    
    ![Screenshot 2026-10-16 at 9.19.35 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.19.35_PM.png)
    
13. After clicking on next you have to press install now and have to wait for a while.
    
    ![Screenshot 2026-10-16 at 9.20.28 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.20.28_PM.png)
    
14. After waiting you have to select the “Operating System you want to install” so for this you have to click “Windows Server 2022 Standard Evaluation (Desktop Experience) x 64 and click on next.
    
    ![Screenshot 2026-10-16 at 9.20.44 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.20.44_PM.png)
    
15. Then Check the box to agree with the Microsoft License Terms Agreement and then click on next.
    
    ![Screenshot 2026-10-16 at 9.20.51 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.20.51_PM.png)
    
16. Then you have to click on Custom Installation.
    
    ![Screenshot 2026-10-16 at 9.21.35 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.21.35_PM.png)
    
    ![Screenshot 2026-10-16 at 9.21.49 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.21.49_PM.png)
    
17. When arrived at this part select “New” and give the exact size as I have given and then click on apply.
    
    ![Screenshot 2026-10-16 at 9.22.07 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-16_at_9.22.07_PM.png)
    
18. You will be seeing the 3 partitions of the disk where the 3rd partition is of 60GB which not truly 60GB just a part of 60GB and click on next.
    
    ![Screenshot 2026-10-17 at 7.30.58 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_7.30.58_PM.png)
    
19. The next stage is the automatic installation of the Operating System this may take a while ( Take a coffee break or do something else meanwhile) after that you should see the “Restarting the machine” on your screen and wait for just few minutes.
    
    ![Screenshot 2026-10-17 at 7.36.58 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_7.36.58_PM.png)
    
20. 🧪Now in this part you have to give the password ( Note: I am learning on how to perform penetration testing on the active directory so i gave the password as “P@$$w0rd!”) if you are working for an organization then be sure give a strong password and click on Finish.
    
    ![Screenshot 2026-10-17 at 7.39.31 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_7.39.31_PM.png)
    
21. After Finalizing Press “CTRL+ALT+DEL” (or) “CTRL+ALT+INSERT” {Thats what VMware says on the screen which acts like first one} on the keyboard, and enter the password which we gave in the previous step.
    
    ![Screenshot 2026-10-17 at 9.01.23 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_9.01.23_PM.png)
    
22. After giving the password you’ll observe the screen size a bit small and bit unable to see, which we will fix in the next step.
    
    ![Screenshot 2026-10-17 at 9.01.59 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_9.01.59_PM.png)
    
23. Hover your cursor to the top bar of the Virtual Machine and click on Workstation > VM > Install VMware Tools.
    
    ![Screenshot 2026-10-17 at 9.03.32 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_9.03.32_PM.png)
    
24. After Clicking it navigate to the D: Drive  and click on “setup64” and not on the autorun with the VMware logo.
    
    ![Screenshot 2026-10-17 at 9.06.53 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_9.06.53_PM.png)
    
25. The wizard opens up and then click on next.
    
    ![Screenshot 2026-10-17 at 9.07.07 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_9.07.07_PM.png)
    

27. Click on “Complete” and then click on next.

![Screenshot 2026-10-17 at 9.07.32 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-17_at_9.07.32_PM.png)

1. During the installation of the VMware Tools your screen will be automatically changed to screen fit.
    
    ![Screenshot 2026-10-18 at 9.00.49 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-18_at_9.00.49_PM.png)
    
2. After that it will ask you to reboot just click on “No” because we will rebooting plenty of times later on.
    
    ![Screenshot 2026-10-18 at 9.02.30 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-18_at_9.02.30_PM.png)
    
3. The first thing to do after the installation of the VMware tools is to change your PC name, Search for “View your PC name” on search bar beside the windows logo and click on it.
    
    ![Screenshot 2026-10-18 at 9.27.06 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-18_at_9.27.06_PM.png)
    
4. After the about page is opened click on Rename this PC.
    
    ![Screenshot 2026-10-18 at 9.27.30 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-18_at_9.27.30_PM.png)
    
5. Since I building the Lab based on Marvel I gave “HYDRA-DC” you can give anything you want and then click on next.
    
    ![Screenshot 2026-10-18 at 9.27.48 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-18_at_9.27.48_PM.png)
    
6. After naming the PC click on “Restart Now”.
    
    ![Screenshot 2026-10-19 at 6.54.41 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.54.41_PM.png)
    
7. After Restarting the machine we have to create the Domain controller to do that click on “Manage” > “Add Roles and features”.
    
    ![Screenshot 2026-10-19 at 6.54.55 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.54.55_PM.png)
    
8. A Wizard will pop up and then click on next
    
    ![Screenshot 2026-10-19 at 6.55.09 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.55.09_PM.png)
    
9. In the “Installation Type” Click on the “Role-based or feature-based installation”
    
    ![Screenshot 2026-10-19 at 6.55.23 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.55.23_PM.png)
    
10. In the “Server Selection”  just click on next and don’t touch anything.
    
    ![Screenshot 2026-10-19 at 6.55.47 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.55.47_PM.png)
    
11. In the “Server Roles” sections you have to click on “Active Directory Domain Services”  and a wizard opens up.
    
    ![Screenshot 2026-10-19 at 6.55.58 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.55.58_PM.png)
    
12. Once the wizard opens up click on “Add Features”.
    
    ![Screenshot 2026-10-19 at 6.56.34 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.56.34_PM.png)
    
13. Click on “Yes” to restart which is necessary for this step and then after that click on “Install” (The restart does not happen Immediately)
    
    ![Screenshot 2026-10-19 at 6.56.56 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_6.56.56_PM.png)
    
14. Now You have to wait for a while the Domain Controller is being installed.
    
    ![Screenshot 2026-10-19 at 7.36.10 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.36.10_PM.png)
    
15. After the installation of the domain controller click on “Promote this server to a domain controller”.
    
    ![Screenshot 2026-10-19 at 7.36.52 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.36.52_PM.png)
    
16. Once the wizard is open, click on the “Deployment Configuration” choose the “Add a new forest” and  enter the root domain name of your choice (I Kept Mine as MARVEL.local) and click on next.
    
    ![Screenshot 2026-10-19 at 7.37.34 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.37.34_PM.png)
    
17. In the next section you have to create a password for DSRM  (You can give the same password which you used to login as administrator) and click on next.
    
    ![Screenshot 2026-10-19 at 7.38.01 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.38.01_PM.png)
    
18. When coming to the “Additional Options” the name will pop up automatically after a minute and then click on next.
    
    ![Screenshot 2026-10-19 at 7.38.29 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.38.29_PM.png)
    
19. Next section we see the path of the files such as “Database, Log files and SYSVOL” keep the default path and click on next.
    
    ![Screenshot 2026-10-19 at 7.38.39 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.38.39_PM.png)
    
20. You can review your options if you want any changes you can simply go to the previous section and then change the settings, if there is nothing to change then simply click on next.
    
    ![Screenshot 2026-10-19 at 7.39.21 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.39.21_PM.png)
    
21. A prerequisite check occur before installation, if there are no errors present simply click on “Install”.
    
    ![Screenshot 2026-10-19 at 7.39.40 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.39.40_PM.png)
    
22. Now installation is happening once again, just wait for a minute or 2.
    
    ![Screenshot 2026-10-19 at 7.40.04 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.40.04_PM.png)
    
23. Once the installation is done, click on done and then let the machine restart.
    
    ![Screenshot 2026-10-19 at 7.40.43 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-19_at_7.40.43_PM.png)
    
24. Once the restarted you can observe that the “MARVEL\Administrator” is showing up  simply give the password which you used for “Administrator account” and you will be logged in as a domain controller for the very first time.
25. We need to install one last thing before we move on to the next step which is installing the certificates (Certificates can be used to authenticate as a user, so a poorly configured certificate template can let a low-privileged user request a certificate that works for an administrator) and how it can be misused ( In terms of penetration tester).
26. Once the server manager has opened up after the restarting the machine follow the same “**steps from 33 to 38**”
    
    ![Screenshot 2026-10-20 at 11.11.42 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.11.42_AM.png)
    
27. Once arrived at this page click on “Active Directory Certificate Services” and a wizard will pop up on the screen.
    
    ![Screenshot 2026-10-20 at 11.12.49 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.12.49_AM.png)
    
28. Click on Add Features and also to check mark the “Include management tools ( if applicable)” 
    
    ![Screenshot 2026-10-20 at 11.13.17 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.13.17_AM.png)
    
29. From Server Roles after checking the certificate services click on next until you reach “AD CS Role Services” {The Features section has to be untouched}  make sure to check mark the “Certification Authority” and then click on next.
    
    ![Screenshot 2026-10-20 at 11.13.32 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.13.32_AM.png)
    
30. In the confirmation section check the “Restart the destination server automatically if required” and when the wizard pops up click on “YES”, and then click on Install which will be similar to the previous installation.
    
    ![Screenshot 2026-10-20 at 11.21.51 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.21.51_AM.png)
    
31. After Installation click on “Configure Active Directory Certificate Services on the destination server”.
    
    ![Screenshot 2026-10-20 at 11.22.05 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.22.05_AM.png)
    
32. We are going to use most of the default options which is available so in the “Credentials” section you can see the credentials and click on next.
    
    ![Screenshot 2026-10-20 at 11.22.20 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.22.20_AM.png)
    
33. In the next section click on the “Certificate Authority” and click on next.
    
    ![Screenshot 2026-10-20 at 11.22.33 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.22.33_AM.png)
    
34. In the “Setup type” by default it will be “Enterprise CA” and then click on next.
    
    ![Screenshot 2026-10-20 at 11.22.43 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.22.43_AM.png)
    
35. Similarly in the CA Type section by default it will be “Root CA” and then click on next.
    
    ![Screenshot 2026-10-20 at 11.22.55 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.22.55_AM.png)
    
36. For the private key section click on the “Create a new private key” and then click on next.
    
    ![Screenshot 2026-10-20 at 11.23.12 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.23.12_AM.png)
    
37. For the cryptography select the SHA256 will be by default and the key length will be 2048 and then click on next.
    
    ![Screenshot 2026-10-20 at 11.23.24 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.23.24_AM.png)
    
38. For the CA Name it will be given by default based on your domain controller name and then click on next.
    
    ![Screenshot 2026-10-20 at 11.23.39 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.23.39_AM.png)
    
39. 🧪For the validity period you can type based on the requirement from the organization (Since I am again using for penetration testing for local i am going to keep at 99 Years) and click on next.
    
    ![Screenshot 2026-10-20 at 11.23.51 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.23.51_AM.png)
    
40. This is the default “Certificate Database” locations you can just simply give next or if you want to change you can do so.
    
    ![Screenshot 2026-10-20 at 11.24.06 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-20_at_11.24.06_AM.png)
    
41. In the Confirmation section shows the review of your selection if everything is according to it click on next and then let it install finally in the Results section you will see the Configuration succeeded click on the close button.
42. One last thing is to restart the machine once again and server manager is ready.

<aside>
💡

Head on to the next section to setup the windows 11 virtual machines.

</aside>

## 4.  Setting up User Machines

To Set up the user machines follow the steps down below accordingly (process may change over time)

1. Open up VmWare and click on “new virtual machine”
    
    ![Capture 1.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_1.png)
    
2. After the wizard has opened click on  **“Typical”** and click on “Next”
    
    ![Capture 2.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_2.png)
    
3. Select “Installed disc image file (iso)” and click on browse and select the file iso file (which was downloaded in “Downloading ISO files”) and click on next.
    
    ![Capture 3.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_3.png)
    
4. Change the “Virtual Machine Name” as per your need and click next.
    
    ![Capture 4.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_4.png)
    
5. This password is not for the computer but for “encryption” you can type any password if you want (Remember it!) this also gives the VM a virtual TPM, which Windows 11 needs later and click next.
    
    ![Capture 5.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_5.png)
    
6. Specify the disk size as 60GB ( Don’t worry just follow the steps everything will fit according to the process) and click “Split Virtual Disk into multiple files” and click next.
    
    ![Capture 6.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_6.png)
    
7. The following specifications will be shown (these are the proper specifications you have to follow)

![Capture 9 ( Remove Floppy DIsk).PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_9_(_Remove_Floppy_DIsk).png)

1. Edit the VM once again to remove the floppy disk which will be present in the “Hard Disk” Section {Since It didn’t show me I skipped the part, but by default it won’t show you as well}

Note: From above image you have observed that the allocated memory is “2GB” but unfortunately when I ran the VM it gave me a message saying “Windows installation needs 4GB Ram” so after the complete installation you can turn it back to “2GB” but expect to rub slow.

Note (For the people following the course): Repeat the same process for the “Spiderman” VM 

1. Turn on the Virtual Machine (VM) {Either both or one at a time depending on the RAM}

![Capture 10.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_10.png)

1. After launching the VM you will be greeted and asked set up the language ( you can select Language based on your need) and click next.

![Capture 11.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_11.png)

1. Select the keyboard layout and click next.

![Capture 12.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_12.png)

1. Select the “Install Windows 11” options (Might not show this on the first time, as I am installing this the second time) and check the box and click on next.

![Capture 13.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_13.png)

1. Accept the “Applicable notices and license terms”.

![Capture 14.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_14.png)

1. In this section click on “Create Partition” and click on “Apply”

![Capture 15.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_15.png)

1. You will be able to see the 3 disk partitions and then click on “next”.

![Capture 16.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_16.png)

1. Select your country or region.

![Capture 17.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_17.png)

1. When you arrive at this section don’t sign in using your email just click on **“Sign-in options”.**

![Capture 18.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_18.png)

1. In the Sign-in options click on “Domain join instead”.

![Capture 19.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_19.png)

1. Enter the name according to your need and click on next.

![Capture 20.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_20.png)

1. Type in the password  as per your need and click on next.

![Capture 21.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_21.png)

1. Select any 3 security questions and give them the answer as “bob” or according to you and click next.

![Capture 22.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_22.png)

1. Make sure turn “off” all these settings and click on next.

![Screenshot 2026-10-05 at 11.47.39 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-05_at_11.47.39_AM.png)

![Capture 23.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_23.png)

1. Once everything is done the screen size will be small and hover your cursor to the top of the”VMWare header” and locate and install  the “Install VMware tools”. Once the dialogue box appears click on “Complete” and click on next.

![Capture 24.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_24.png)

1. Once finished click on finish.

![Capture 25.PNG](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Capture_25.png)

1. Open settings and Rename your PC to the particular name according to your desire, once renamed you can safely restart the PC.
2. Do the same process for other workstation and you should have a proper workstations.

## 5. Setting up Users, Groups, and Policies

After setting up the workstations, turn off all the workstations and follow the below process.

1. Power on the “Windows Server Virtual Machine (or) Domain Controller”.
2. Once the VM is turned “On”, the “Server Manager” opens up automatically.
    
    ![Screenshot 2026-10-06 at 3.04.55 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_3.04.55_PM.png)
    
3. Once the “Server Manager” is opened click on “Tools” then click on “Active Directory Users and Computers”.
    
    ![Screenshot 2026-10-06 at 6.39.53 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_6.39.53_PM.png)
    
4. Once it opens up you can see the created “Domain” on the left panel, in this case it is “MARVEL.local”, expand it by clicking on the arrow beside the name to see different OUs (Organizational Units).
    
    ![Screenshot 2026-10-06 at 6.40.26 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_6.40.26_PM.png)
    
5. Once Expanded you can observe that you can see the different OU’s, if you select Computers you can see the computers which have joined the domain (Unfortunately we didn’t join any yet).
6. Expand the Users OU to list out the users.
    
    ![Screenshot 2026-10-06 at 6.53.34 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_6.53.34_PM.png)
    
7. From the Image, you can see there are lots of users and it may some difficulty to see the user, although the “Administrator” and the “Guest” is the only user present in this OU.
    
    ![Screenshot 2026-10-06 at 6.56.01 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_6.56.01_PM.png)
    
    ![Screenshot 2026-10-06 at 6.56.19 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_6.56.19_PM.png)
    
8. Right Click on the “Domain” then click on New, then click on “Organizational Unit”.
    
    ![Screenshot 2026-10-06 at 6.59.10 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_6.59.10_PM.png)
    
9. Once the dialogue box opens up Enter the name as “Groups” and then click on OK.
    
    ![Screenshot 2026-10-06 at 7.11.30 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.11.30_PM.png)
    
10. Go back to the “Users” section and then select all the users, besides the guest and the administrator and then transfer it to the “Groups” OU.
    
    ![Screenshot 2026-10-06 at 7.15.02 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.15.02_PM.png)
    
11. A box opens up stating a warning ( You don’t have to follow this I am following the course so I have to do it).
    
    ![Screenshot 2026-10-06 at 7.20.31 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.20.31_PM.png)
    
12. The final “Users” section will look like this only 2 users should be present, the small arrow next to the icon in the “guest” means the account is disabled so the only account enabled is the administrator.
13. Let’s create more accounts, we need to create another administrator.
    
    ![Screenshot 2026-10-06 at 7.27.08 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.27.08_PM.png)
    
14. Right Click on the administrator and then click on copy this will copy all the permissions that the administrator has, if you want to know what permission it has you can just double click on the “Administrator”.
    
    ![Screenshot 2026-10-06 at 7.29.40 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.29.40_PM.png)
    
    1. You can see that there no name given to it because this the built in account, if you click on the “Member Of” you can see all the members or membership that it does have.
    
    ![Screenshot 2026-10-06 at 7.29.54 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.29.54_PM.png)
    
    1. You can see all the membership it does have, so simply we are going to copy this.
    
    ![Screenshot 2026-10-06 at 7.36.41 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_7.36.41_PM.png)
    
    1. Right Click on the administrator and click on copy, a dialogue box appears on the screen.
    
    ![Screenshot 2026-10-06 at 8.18.24 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_8.18.24_PM.png)
    
    1. You can give any name you want, I gave according to the course and click on next.
    
    ![Screenshot 2026-10-06 at 8.18.56 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_8.18.56_PM.png)
    
    19.🧪Enter the password of your own choice and check only on “Password never expires” (Usually it expires in organizations but for the sake of the course for Penetration testing I’ll check this box) and then click on next.
    
    ![Screenshot 2026-10-06 at 8.26.43 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-06_at_8.26.43_PM.png)
    
    1. After creation you should be able to see 3 users.
    
    ### Creating service account that is administrator account
    
    - Service account they are used to run a service
    1. Once again Copy the “Administrator” and once the dialogue box opens up fill in the SQL user details (Look at the image down below)
        
        ![Screenshot 2026-10-07 at 1.58.14 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_1.58.14_PM.png)
        
    2. After inserting the details click on next.
        
        ![Screenshot 2026-10-07 at 2.01.08 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_2.01.08_PM.png)
        
    3. You can type in the Password on your own but ( I gave the Password as “MYPassword123#” ) { The password satisfies the condition which is of 14 characters } and then click on next.
        
        ![Screenshot 2026-10-07 at 2.01.26 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_2.01.26_PM.png)
        
    4. Once again you will be able to see 4 Users in the main panel, In order to not forget the password is you can double click the “SQL Service” user and then under the description you can type the following.
        
        ![Screenshot 2026-10-07 at 2.04.19 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_2.04.19_PM.png)
        
    5. 🧪 Although this is big NO to store the password in the description while setting AD for the organizations. (Since I am following the course I have to create this) and then click on “Apply” and click on OK.
    1. Now we are going to create other low-level users which are part of the domain user group.
        
        ![Screenshot 2026-10-07 at 2.24.04 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_2.24.04_PM.png)
        
    2. Right Click on the white space and then hover on to new and then “User”.
        
        ![Screenshot 2026-10-07 at 2.25.24 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_2.25.24_PM.png)
        
    3. After typing in the details click on the next, and then give the password as “Password1”
        
        ![Screenshot 2026-10-07 at 2.26.41 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-07_at_2.26.41_PM.png)
        
    4. 🧪 Uncheck the “User must change password at next logon” and then check the “Password never expires.”
    5. Click on Next.
        
        ![Screenshot 2026-10-12 at 6.36.56 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.36.56_PM.png)
        
    6. From the image you can see that now we have 5 Users.
    7. Create peter parker user by right clicking on the frank castle and click on copy.
        
        ![Screenshot 2026-10-12 at 6.38.38 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.38.38_PM.png)
        
        ![Screenshot 2026-10-12 at 6.38.56 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.38.56_PM.png)
        
    8. For Peter Parker give the password as “Password2” and click on next.
        
        ![Screenshot 2026-10-12 at 6.40.37 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.40.37_PM.png)
        
    9. Finally you will be able to see 6 users on your main panel.
    10. Next thing we are going to create is a file share which we are going to abuse ( for Penetration testing)
        
        ![Screenshot 2026-10-12 at 6.42.28 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.42.28_PM.png)
        
    11. Come to the main panel and click on File and Storage Services on the left panel.
        
        ![Screenshot 2026-10-12 at 6.49.05 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.49.05_PM.png)
        
    12. Click on “Shares” and then Click on “Tasks” and “NewShare”.
        
        ![Screenshot 2026-10-12 at 6.50.02 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.50.02_PM.png)
        
    13. Click on “SMB Share - Quick” and the click Next.
        
        ![Screenshot 2026-10-12 at 6.50.34 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.50.34_PM.png)
        
    14. The “Share Location” does not matter then click on next, when arrived at “Share Name” type in the “hackme” and then click next.
        
        ![Screenshot 2026-10-12 at 6.51.03 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.51.03_PM.png)
        
    15. Finally Click on “Next” Until you arrive at Confirmation and click “Create”.
        
        ![Screenshot 2026-10-12 at 6.54.07 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-12_at_6.54.07_PM.png)
        
    16. The Creation of the Share should be done in an instant and then after that you can hit close.
    17. Next thing is to setup the service account which we created (SQL User)
    18. Open up Command Prompt as “Administrator” and type in the following command.
        
        ```
        setspn -a HYDRA-DC/SQLService.MARVEL.local:60111 MARVEL\SQLService
        ```
        
    
    Note: If you have different Domain Controller and Domain name you need to replace the name.
    
    ![Screenshot 2026-10-13 at 6.50.54 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_6.50.54_PM.png)
    
    1. After typing in the command hit enter the response should be the same as the above image, now in order to verify that we can type in another command and hit enter
    
    ```
    setspn -T MARVEL.local -Q */*
    ```
    
    ![Screenshot 2026-10-13 at 6.53.16 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_6.53.16_PM.png)
    
    1. After hitting enter we should see the “Existing SPN found!”
        
        <aside>
        💡 SPN stands for “Service Principal Name”
        </aside>
        
    2. One final thing is to add policy to the entire domain, click on the “Windows” button and search for “Group Policy Management”.
        
        <aside>
        💡
        Note: If you are setting up for the Organization then don’t create this policy just leave this part, but if you want to practice for Penetration testing in your local network then carry on with this steps.
        </aside>
        
        ![Screenshot 2026-10-13 at 7.21.11 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_7.21.11_PM.png)
        
    3. After opening the “Group Policy Management” open the Forest and domain and then right click on the “MARVEL.local” and click on “Create a GPO in this domain, and link it here”.
        
        ![Screenshot 2026-10-13 at 7.24.05 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_7.24.05_PM.png)
        
    4. 🧪 Once opened type in the name as “Disable Windows Defender” and click OK.
    5. After clicking “OK” you should be able to see “Disable Windows Defender” along with Default Domain Policy.
        
        ![Screenshot 2026-10-13 at 8.18.51 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.18.51_PM.png)
        
    6. Right click on “Disable Windows Defender” and click on Edit.
        
        ![Screenshot 2026-10-13 at 8.21.31 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.21.31_PM.png)
        
    7. The “Group Policy Management Editor” opens up, navigate to the following path Computer Configuration > Policies > Administrative Templates > Windows Components > Microsoft Defender Antivirus [In Server Manager 2022]  (or) Windows Defender Antivirus -> [Is in 2019 Version of Server Manager] 
        
        ![Screenshot 2026-10-13 at 8.25.51 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.25.51_PM.png)
        
    8. After clicking on the “Microsoft Defender Antivirus” look for “Turn off Microsoft Defender Antivirus” on the right panel and Double Click it.
        
        ![Screenshot 2026-10-13 at 8.28.48 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.28.48_PM.png)
        
    9. Once the dialogue box opens up, Click on Enabled and hit the apply as well as “OK” button.
        
        ![Screenshot 2026-10-13 at 8.31.37 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.31.37_PM.png)
        
    10. Coming back the Group policy Management Screen right click on the “Disable Windows Defender” and Click on “Enforced”, where you can see the “Enforced” on the right panel after clicking it.
    11. Lastly you need to make sure that the Server Manager’s VM’s Ip address is “Static Connection” rather than dynamic.
        
        ### Steps to turn the Dynamic IP address into Static
        
        1. Open Command Terminal and type in the “ipconfig” copy the “IPV4 Address, Subnet Mask, and Default Gateway” [Sorry no screenshot available]
            
            ![Screenshot 2026-10-13 at 8.43.32 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.43.32_PM.png)
            
        2. Next up is to click on “Open Network & Internet settings” which you can right click on the “Computer with a wire logo (or) a Wifi logo”.
            
            <aside>
            💡 Note: If you are not able to find it, press the “Windows Key button + R button ” on keyboard and type in “ncpa.cpl” and skip till step 4.
            </aside>
            
            ![Screenshot 2026-10-13 at 8.43.45 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.43.45_PM.png)
            
        3. Click on “Change adapter options”.
            
            ![Screenshot 2026-10-13 at 8.43.56 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.43.56_PM.png)
            
        4. You should see the Network Connections and Double Click on the Interface.
            
            ![Screenshot 2026-10-13 at 8.44.22 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.44.22_PM.png)
            
        5. Navigate to “Internet Protocol Version 4(TCP/IPv4)” and then click on it.
            
            ![Screenshot 2026-10-13 at 8.44.35 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-13_at_8.44.35_PM.png)
            
        6. Finally Click on “Use the following IP address”, and then enter the details which copied from the 1st Step, and then click on OK.
    12. Go on to the next topic for joining the Machines to the Domain.

## 6. Joining Machines to the Domain

1. This section helps you how to connect the machines to the domain controller , before joining you configure you virtual machines once again.
2. Now if you have 16GB of RAM then in my opinion you should allot every machine a 2GB RAM or memory (I gave every machine 2 GB, because larger allocations gave me a memory error.) just allocate depend upon your availability.
3. Now Power on every machine from Domain Controller to the Punisher to Spiderman machine or you can just turn on only 2 machines (Depending on the RAM) and login to the computers.
4. Once signed in you need to Point DNS to the Domain Controller.
    
    ![Screenshot 2026-10-21 at 11.06.54 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.06.54_AM.png)
    
5. Press “Windows + R” button on the keyboard and type in the command “ncpa.cpl” which should this section on your screen (See the above image) and double click on it.
    
    ![Screenshot 2026-10-21 at 11.07.03 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.07.03_AM.png)
    
6. Click on “Properties” in the general section.
    
    ![Screenshot 2026-10-21 at 11.07.16 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.07.16_AM.png)
    
7. Once the properties open up click on “IPV4”.
    
    ![Screenshot 2026-10-21 at 11.07.53 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.07.53_AM.png)
    
8. When the IPV4 properties opens up in the DNS section click on “Use the following DNS server addresses”, now you have to give the ip address of the domain controller (You can check the previous section on how to convert the “Dynamic IP to Static IP”) and then click on OK and the same process should be done for the other machine as well.
    
    ![Screenshot 2026-10-21 at 11.29.20 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/cba87a51-ef4b-4d7f-99d8-aba0e7b29829.png)
    
9. Now this is the part to join where the machines should join the domain (Make sure the domain controller Virtual Machine is on otherwise it won’t work) click on the windows and search for “Access work or school” and then click on it.
    
    ![Screenshot 2026-10-21 at 11.29.33 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.29.33_AM.png)
    
10. Click on connect.
    
    ![Screenshot 2026-10-21 at 11.29.47 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.29.47_AM.png)
    
11. If you see the “Alternate actions” you can see the “Join this device to a local Active Directory domain”  click on it 
    
    <aside>
    💡
    
    Note: I advise you to perform this process simultaneously for the other virtual machine because sometimes if one machine gets joined in the domain the other machine will not be able to open the “Microsoft Wizard” (as seen in the upper image) and you have to perform the other way of joining this but I advice you to use this method and not the other way joining it.
    
    </aside>
    
    ![Screenshot 2026-10-21 at 11.30.03 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.30.03_AM.png)
    
12. Enter the domain name as “MARVEL.local” and then click on next.
    
    ![Screenshot 2026-10-21 at 11.30.28 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.30.28_AM.png)
    
13. Once you click on next you will see a pop up box asking for the username and password go ahead and give the username as “administrator” and the password (which you have already gave it) and then click on OK {Once again make sure the Domain controller VM is turned on otherwise it will say not found}
    
    ![Screenshot 2026-10-21 at 11.30.46 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.30.46_AM.png)
    
14. Once done let the user account be administrator and by default the “Account type” will be “Standard User” but you can click on the drop box and the select “Administrator” and then click on next choosing the “Administrator” makes the domain account a local admin.
    
    ![Screenshot 2026-10-21 at 11.30.58 AM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_11.30.58_AM.png)
    
15. And make sure to restart the machine.
16. Login back to the Domain controller virtual machine in order to check if the machines joined to the domain.
    
    ![Screenshot 2026-10-21 at 12.06.04 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.06.04_PM.png)
    
17. After the logging in to the Domain Controller click on Tools > Active Directory Users and Computers
    
    ![Screenshot 2026-10-21 at 12.07.55 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.07.55_PM.png)
    
18. Locate to the MARVEL.local > Computers and from there you can see there are 2 computers present on the right panel this confirms that the machines have successfully joined the domain.
    
    ![Screenshot 2026-10-21 at 12.09.39 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.09.39_PM.png)
    
19. Next thing is to go back to the “Punisher Machine” (Which was just restarted) and login using the MARVEL\Administrator and the password and you can perform the same thing on the Spiderman Machine as well.
    
    ![Screenshot 2026-10-21 at 12.12.10 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.12.10_PM.png)
    
20. Once logged in as the administrator in the Punisher machine search for “Edit local users and groups and click on that.
    
    ![Screenshot 2026-10-21 at 12.12.40 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.12.40_PM.png)
    
21. Once opened click on the users.
    
    ![Screenshot 2026-10-21 at 12.13.16 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.13.16_PM.png)
    
22. 🧪 You can see the list of users in this section, right click on the administrator and click on “Set Password”.
    
    ![Screenshot 2026-10-21 at 12.13.25 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.13.25_PM.png)
    
23. 🧪 Click on Proceed.
    
    ![Screenshot 2026-10-21 at 12.13.45 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.13.45_PM.png)
    
24. 🧪 Enter the new password as “Password1!” For both the sections and then click on ok.
    
    ![Screenshot 2026-10-21 at 12.14.05 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.14.05_PM.png)
    
25. 🧪 Once you have set the password double click on the administrator and then uncheck the “Account is disabled” and this hit apply ( Usually the Account is usually disabled ) and then click on ok.
    
    ![Screenshot 2026-10-21 at 12.14.37 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.14.37_PM.png)
    
26. Navigate to the Groups section and the  double click on “Administrators”.
    
    ![Screenshot 2026-10-21 at 12.14.50 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.14.50_PM.png)
    
27. You can see the which users has the administrator privilege, from this we need to add one more user click on “Add”.
    
    ![Screenshot 2026-10-21 at 12.15.12 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.15.12_PM.png)
    
28. Type in “fcastle” and click on “Check Names”.
    
    ![Screenshot 2026-10-21 at 12.15.21 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.15.21_PM.png)
    
29. You will see the complete name “Frank Castle” and then click on OK.
    
    ![Screenshot 2026-10-21 at 12.15.31 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.15.31_PM.png)
    
30. After that you will see 5 users which have the admin privileges and then click on apply and then ok.
    
    ![Screenshot 2026-10-21 at 12.17.51 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.17.51_PM.png)
    
31. After that open the “File Explorer” and click on “Network”.
    
    ![Screenshot 2026-10-21 at 12.18.25 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.18.25_PM.png)
    
32. You will get a pop saying that the “Network discovery is OFF” just click ok, and then you will see another pop just below the file path click on that and click on “Turn on network discovery and file sharing”.
    
    ![Screenshot 2026-10-21 at 12.19.27 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.19.27_PM.png)
    
33. Once turned on you will see the “Hydra-DC” on the network and double clicking on that.
    
    ![Screenshot 2026-10-21 at 12.19.40 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.19.40_PM.png)
    
34. You can see the “hackme” folder which we set up in the previous section.
35. Now you have to do the same thing for the “Spiderman Machine” but there is slight changes which I will show you.
    
    ![Screenshot 2026-10-21 at 12.13.45 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.13.45_PM%201.png)
    
36. In this section you have to give the same password as you gave in the “Punisher Machine” { This is going to be useful when we are performing for penetration testing }
    
    ![Screenshot 2026-10-21 at 12.43.23 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.43.23_PM.png)
    
37. In the groups administration part (In the local users and groups) you have to add 2 users
    1. pparker > Check Names > Peter Parker ( click on OK)
    2. fcastle > Check Names > Frank Castle ( Click on OK)
    
    ![Screenshot 2026-10-21 at 12.46.45 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.46.45_PM.png)
    
38. Now you need to sign out and login as the peter parker in the Spiderman Machine (You can see above username) and the password is Password1, and log in. (You are basically logging in locally and not on the domain)
    
    ![Screenshot 2026-10-21 at 12.49.01 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.49.01_PM.png)
    
39. Once Logged in open “File Explorer” go to “This PC” and then on the top click on “Computer” and then click on “Map Network Drive” {Note: the Screenshot above is from windows 10 but there are some changes in windows 11 which you can find it easily or you can take help from the google, but the rest of the steps can be followed in windows 11}
    
    ![Screenshot 2026-10-21 at 12.49.31 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.49.31_PM.png)
    
40. Once opened up give the folder name as “\\HYDRA-DC\hackme” and then check the “Connect using different credentials” and then click on finish.
    
    ![Screenshot 2026-10-21 at 12.49.47 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.49.47_PM.png)
    
41. Once clicked on finish you will be prompted to enter the credentials which will be the Domain controllers Credentials and then check on “Remember my credentials” and click on OK.
    
    ![Screenshot 2026-10-21 at 12.50.01 PM.png](/assets/img/Building-Vulnerable-Active-Directory-For-Pentesting-Lab/Screenshot_2026-10-21_at_12.50.01_PM.png)
    
42. After click on OK you will be able to see a Shared drive on the Main panel.

<aside>
💡 This is the end of building of the lab, and from here onwards you can start performing penetration testing attacks on the active directory

</aside>

## How this lab maps to Saudi cybersecurity requirements

Saudi Arabia's National Cybersecurity Authority (NCA) was created by royal decree in 2017 as the country's central cybersecurity authority. Its baseline is the **Essential Cybersecurity Controls (ECC-2:2024)**, which is mandatory for government bodies and owners or operators of critical national infrastructure, and recommended for everyone else. Further NCA frameworks sit on top of it: cloud (CCC), critical systems (CSCC), operational technology (OTCC), data (DCC) and telework (TCC). Banks and other financial firms must also follow the **SAMA Cyber Security Framework**.

### What I learned building this

Building this lab taught me how a Windows domain is put together from scratch, which is the foundation for assessing or hardening one. Specifically, I:

- Installed and promoted a Windows Server 2022 Domain Controller and created the `MARVEL.local` domain
- Installed and configured Active Directory Certificate Services (an Enterprise Root CA)
- Created users, groups, organizational units and service accounts, including a service account with a Service Principal Name
- Set up a file share and applied settings with Group Policy
- Configured static IPs and DNS, and joined two Windows 11 workstations to the domain
- Set up local administrator access and shared drives, then documented the weak spots that attackers target

Active Directory handles identity and access in most organizations, and identity and access management is one of the control areas in the ECC. Knowing how AD is built makes it easier to understand how those controls apply in practice, and where they commonly fail.

### How the lab's weaknesses relate to control areas

| What I built in the lab | Control area it relates to |
| --- | --- |
| Weak, never-expiring passwords; password in a description | Identity and access management, password policy |
| Admin service account with an SPN; reused local admin password | Privileged access management, least privilege |
| Open `hackme` SMB share | Access control, data protection |
| Defender disabled by GPO | Malware and endpoint protection |
| Host-only network | Network segmentation |
| Snapshots | Backup and recovery |
| The attacks this lab enables | Vulnerability management and penetration testing |

This is a home lab, not a production environment. Every weakness is deliberate, so I can learn how an attacker finds these gaps and how to help close them.

**References:** [NCA ECC-2:2024 (official document)](https://cdn.nca.gov.sa/api/files/public/upload/86e09090-44e4-481f-bc28-355673607654_ECC--2024-EN.pdf) · [Clyde & Co: ECC-2 key updates](https://www.clydeco.com/en/insights/2025/01/saudi-arabia-essential-cybersecurity-controls)
