# SIEM Lab Using Microsoft(Azure) Sentinel


# Technologies Used


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
- After we are finished with this our SEIM will connect to the workspace and display the geodata on a map.
- Alright, let's get started.
- We can search for "log analytics workspace" and open that.
- After that go ahead and click on "Create log analytics workspace."
- Under the resource group, we want to go ahead and change it to the resource group that we made earlier.
- After this, name it whatever you want and we'll use the same region as before, West US2.
- Next, we can finish up by selecting create.

<img src="https://github.com/user-attachments/assets/52b8d735-82f4-4b82-8fe6-ef671a2f5782" alt="Screenshot 2024-09-05 141824" width="600" style="float: left; margin-right: 10px;">

#

- Now we need to enable the ability to gather logs from the VM and display them in the log analytics workspace.
- To do this, let's head back to the search bar and search for "Microsoft Defender for Cloud"
- Once there we can head over to "Management" and then "Environment Settings."
- Then we use the drop-down menu under "Tenant Root Group" to find the long analytics workspace we just made.
<img src="https://github.com/user-attachments/assets/7b6da8f3-8488-48ef-9782-d00b25a68571" alt="Screenshot 2024-09-06 090147" width="600" style="float: left; margin-right: 10px;">

#

- Click on whatever you named it and once we get to Defender Plans we want to select "enable all plans."
- After this, we want to just disable SQL servers on machines. Go ahead and save that and then head over to data collection.
- Here we want to enable "All Events" and then save that. 
- Now we'll head back over to our log analytics workspace and connect it to the VM.
- Once we make it back over to the log analytics workspace we want to find the VM that we made previously.
- Find the "Classic" dropdown menu and select Virtual Machines.
- After this select the VM.
- Then we select connect.
<img src="https://github.com/user-attachments/assets/339fdffd-9a73-42bb-8ebc-52811b0314b9" alt="Screenshot 2024-09-06 091523" width="600" style="float: left; margin-right: 10px;">

#
- 































