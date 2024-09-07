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

- Alright, now it is time to set up Sentinel which we will use to visualize the attack data!
- Head up to the search bar again and head over to "Microsoft Sentinel."
- Now we just create Microsoft Sentinel and add the workspace that we want to connect to.  
<img src="https://github.com/user-attachments/assets/6adc55b9-d8ea-4e2b-bd4a-47f0a1edc5d2" alt="Screenshot 2024-09-06 092911" width="600" style="float: left; margin-right: 10px;">

#

- Ok, now let's head back over to our VM.
- We're going to log into it using Remote Desktop.
- First, we'll copy the IP address of our VM and then open Remote Desktop on our computer.
- Once there let's enter that IP address and click connect.
- When the login screen pops up click on "Use a different account."
- Use the credentials created previously to log in.
- Accept the security certificate notification.
- After this is finished loading just disable all of the privacy settings and continue.
- Alright, We're in!
<img src="https://github.com/user-attachments/assets/c23172b0-7dbc-42b7-b4b4-f0c0eee1eb21" alt="Screenshot 2024-09-06 094650" width="600" style="float: left; margin-right: 10px;">

#

- Inside the VM head over to the start menu and open "Event Viewer."
- Once there click on the "Windows Logs" drop-down menu and find "Security."
- When we select security we can see all the security events on the VM.
- For this lab, we will be focusing on Event ID 4625, which is an audit failure.
- We're going to gather all these failures of attackers trying to log in through RDP!
- If we click on one of these we can gather information and see the IP address of the person trying to log in.
- We are going to use Powershell to gather all of these failures and post the IP addresses to an IP geolocation API which will then return a bunch of information for us.
- We will then use the information it gives us to create a custom log which we'll then send to the log analytics workspace and then use Sentinel to read the coordinates of the attacks and plot them on a map for us to visualize. 
<img src="https://github.com/user-attachments/assets/f29d121a-d4d5-42fe-a725-94b824f865a0" alt="Screenshot 2024-09-06 100730" width="600" style="float: left; margin-right: 10px;">

#

- Let's go ahead and turn the firewall off so can respond to ICMP echo requests.
- This will allow attackers to discover the VM on the internet much faster.
- Let's head over to the command line on our own computer.
- Once there let's ping our virtual machine.
- To do this we run the command "ping (IP Address) -t"
- The -t tells it to ping the address perpetually.
- When we do this and point it toward our VM we can see that it is timing out. 
<img src="https://github.com/user-attachments/assets/a360bcb0-a7c3-4fa2-9d9c-cb06a9c483ba" alt="Screenshot 2024-09-07 084716" width="600" style="float: left; margin-right: 10px;">

#

- Now we'll head over to the virtual machine and turn off the firewall.
- Open the start menu and enter "wf.msc."
- Once there click "Windows Defender Firewall Policies" and turn everything off.
- As soon as we do that we can see that the ping is starting to work since echo requests are allowed.
<img src="https://github.com/user-attachments/assets/a48b37a5-bf89-48aa-90a8-d4f541bb7716" alt="Screenshot 2024-09-07 085355" width="600" style="float: left; margin-right: 10px;">

#

- Next up, we will copy a PowerShell provided by Josh Madakor.
- The script is located at https://github.com/joshmadakor1/Sentinel-Lab/blob/main/Custom_Security_Log_Exporter.ps1
- Once we copy the script we head over to the Start menu and open up PowerShell ISE.
- Once we are there we can paste the script and save it onto the desktop.
<img src="https://github.com/user-attachments/assets/2a8f72f1-5984-4881-ba52-5a1a392c2602" alt="Screenshot 2024-09-07 090251" width="600" style="float: left; margin-right: 10px;">

#

- The next thing we need to do is get an API key from https://ipgeolocation.io/
- We'll put this into the PowerShell Script so that we can convert our IP Address to longitude and latitude.
- Ok so let's head over to that site inside of our VM and sign up so that we can get that API key.
<img src="https://github.com/user-attachments/assets/a44d59a2-216f-499d-b4c9-b1cf7211ef33" alt="Screenshot 2024-09-07 090825" width="600" style="float: left; margin-right: 10px;">

#

- Once we can log in we can copy the API key, transfer it to the PowerShell script, and then save the file.

<img src="https://github.com/user-attachments/assets/af8f1902-70d7-4927-93cf-328ef25f2f94" alt="Screenshot 2024-09-07 091402" width="600" style="float: left; margin-right: 10px;">

#

- Next, let's go ahead and run the PowerShell script.
- The script will be creating a log file.
- It continuously monitors the security log, takes all the events of people who failed to log in, and collects the geodata from them based on their IP Addresses.
- After encountering some troubleshooting issues check out how many people are attacking this machine already! They are pouring in.
- I had to change the first line because the script wouldn't run correctly. Then I had to go into the program data file and delete the first log file that the script made and try again. 
  <img src="https://github.com/user-attachments/assets/b6ab5908-f208-4e03-be41-6a59e0fe60ba" alt="Screenshot 2024-09-07 103350" width="600" style="float: left; margin-right: 10px;">

#

- New up, we are going to create a custom log in Log Analytics Workspace. 
- Let's go ahead and head over there in the Azure portal.
- Once there, and in our workspace, we head over to settings.
- Under settings, we select "Tables" and then create a new custom log (MMA-Based).
- Once there we need a sample log to select. We are going to grab the one from the Program Data file in our VM.
- Head over to the VM and click start, run, C:\ProgramData
- Open the failed_rdp file that the script created and copy the contents.
- Afterwards, save the contents onto a file on your main computer and then open that file in the log analytics workspace. 
- This will train log analytics on what to look for in our log file.
<img src="https://github.com/user-attachments/assets/f9391a1c-312e-43f9-90bb-d32feb7b0a03" alt="Screenshot 2024-09-07 104807" width="600" style="float: left; margin-right: 10px;">

#

- Select next, and then under collection paths we're going to put the path where the log file is which is C:\ProgramData\failed_rdp.log
- On the next page under details we can just name it whatever but I am going to name it Failed_RDP.
- Alright now just create the custom log!
- 
<img src="https://github.com/user-attachments/assets/686ba11c-1322-4b4d-a0a5-40118b5c3983" alt="Screenshot 2024-09-07 105305" width="600" style="float: left; margin-right: 10px;">

#

- To check out the log we can head over to Logs and search for the log we just created, which in my case is Failed_RDP_CL
- When we find this and then run it we can see everything that is in it.
- Let's check out the security events under our log analytics workspace also.
- This time let's type in securityevents and then run that.
- We can see all of the security events we were looking at earlier in our VM!
<img src="https://github.com/user-attachments/assets/69263fc2-f388-47f4-b9b1-5a26cab16616" alt="Screenshot 2024-09-07 110229" width="600" style="float: left; margin-right: 10px;">

#


































