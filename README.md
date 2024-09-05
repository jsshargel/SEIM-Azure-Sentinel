# SEIM Lab Using Azure Sentinel
- 
# Technologies Used
- VirtualBox
- Azure Sentinel

# Configuration Steps
- The first thing we will do is create an Azure account. 
- An important note: At the end of this lab we need to circle back and delete the resource group with all of the resources in it.
- After we create our Azure account let's head over to portal.azure.com
- Once here let's create our virtual machine. This will be our honeypot.
- We'll click on create and select the Azure virtual machine option because we want to configure it ourselves.
- The first thing we should do is create a resource group. I am naming mine "HoneyPotLab."
- Next, I am going to name the VM "HoneyPot"
- After that, we need to pick a region. I am going to use West US 2.
- Let's make sure availability options are set to "No infrastructure redundancy required" and security type is set to "Standard."
- I am going to change the image to Windows 10 Pro.
- The size we can keep the same, "Standard_D2s_v3 - 2 vcpus, 8GB Memory."
- Now we can make a username and password.
- After that we do not need to change anything else and we can go ahead and move on to the next part of the configuration.
<img src="https://github.com/user-attachments/assets/33440341-61d8-44fb-afce-3c7b0389e595" alt="Top Image" width="600" style="float: left; margin-right: 10px;">

#

- We can move past disks and head over to networking. 
- In the networking configuration, we need to change some settings that will make the VM much more enticing to attackers.
- To do this select "Advanced" under the NIC network security group. This is the firewall.
- After that, we need to create and configure a new network security group.
- Under inbound rules let's just go ahead and delete the existing rule.
- We need to create our own inbound rule that allows anyone into the VM.
- To do let's change the destination port to a * and change the priority to 100 and name it whatever you want. The settings should look the same as below.
<img src="https://github.com/user-attachments/assets/53ec3f05-64e4-448f-a56b-3ec8fa4bad76" alt="Screenshot 2024-09-05 134953" width="600" style="float: left; margin-right: 10px;">

#

- After we are finished with that let's go ahead and create the VM.
- While that is happening let's create a log analytics workspace.
- What this does is ingest logs from the VM we just created.
- We are going to be analyzing Windows event logs and creating our own custom log that contains geographic information so that we can see where the attacks are coming from. 
