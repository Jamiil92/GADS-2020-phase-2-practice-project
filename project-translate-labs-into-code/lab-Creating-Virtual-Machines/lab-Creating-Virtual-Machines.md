# Lab - Creating Virtual Machines

## Objectives

In this lab, you will explore the Virtual Machine instance options and create several VMs with different characteristics.

In this lab, you learn how to perform the following tasks:

	- Create a utility virtual machine

	- Create a windows virtual machine

	- create a custom virtual machine

## Steps:

1. Create a utility virtual machine.

	a- Create a VM

    	gcloud compute instances create utility-vm --zone=us-central1-c --machine-type=n1-standard-1 --subnet=default --no-address --image=debian-10-buster-v20200910 --image-project=debian-cloud

	- Result : utility virtual machine created with some details that is explained in the following sections
	
	b- Explore VM details

	   **Examining 

	   Notice that you can't change the machine type, the CPU platform, or the zone.

	   You can add network tags and allow specific network traffic from the internet through firewalls.

           Some properties of a VM are integral to the VM, are established when the VM is created, and cannot be changed.
	
	   Other properties can be edited. 

	   You can add additional disks and you can also determine whether the boot disk is deleted when the instance is deleted.

	   Normally the boot disk defaults to being deleted automatically when the instance is deleted.
	   
	   But sometimes you will want to override this behavior. 
           
	   This feature is very important because you cannot create an image from a boot disk when it is attached to a running instance.

	   So you would need to disable Delete boot disk when instance is deleted to enable creating a system image from the boot disk.

	   **Examining availability policies**

	   You can't convert a non-preemptible instance into a preemptible one. This choice must be made at VM creation. A preemptible instance can be interrupted at any time and is available at a lower cost.

	   If a VM is stopped for any reason, (for example an outage or a hardware failure) the automatic restart feature will start it back up. Is this the behavior you want? Are your applications idempotent (written to handle a second startup properly)?

During host maintenance, the VM is set for live migration. However, you can have the VM terminated instead of migrated.

If you make changes, they can sometimes take several minutes to be implemented, especially if they involve networking changes like adding firewalls or changing the external IP.
	

	c- Explore the VM logs


2. Create a Windows virtual machine.
    
	a- Create a VM

	gcloud compute instances create windows-vm --zone=europe-west2-a --machine-type=n1-standard-2 --subnet=default --tags=http-server,https-server --image=windows-server-2016-dc-v20200908 --image-project=windows-cloud --boot-disk-size=100GB --boot-disk-type=pd-ssd

	gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --target-tags=http-server

	gcloud compute firewall-rules create default-allow-https --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:443 --target-tags=https-server

	- Result : We created a windows virtual machine with two firewall rules. On the GCP console you will notice that the connection option in the far right column in this case is RDP, not SSH. 

	RDP is the Remote Desktop Protocol.

	You would need the RDP client installed on your local machine to connect to the Windows desktop.

	Note: Installing an RDP client on your local machine is outside the scope of this lab and of the class.

	For this reason, you will not be connecting to the Windows VM during this lab.

	However, you will step through the usual procedures up to the point of requiring the RDP client.

	Instructions for connecting to Windows VMs are here:

	b- set the password for the VM

	You will not connect to the Windows VM during this lab. However, the process would look something like the following (depending on the RDP client you installed). The RDP client shown can be installed for Chrome here:

	https://chrome.google.com/webstore/detail/chrome-rdp-for-google-clo/mpbbnannobiobpnfblimoapbephgifkm?hl=en-US

	On the VM instances page, you would click RDP for your Windows VM and connect with the password copied earlier.

3. Create a custom virtual machine.

	a- Create a VM

	gcloud compute instances create custom-vm --zone=us-west1-b --machine-type=custom-6-32768 --subnet=default --image=debian-10-buster-v20200910 --image-project=debian-cloud

	b- Connect via SSH to your custom VM

	- Connect to custom-vm:

		gcloud compute ssh custom-vm

		if prompted Do you want to continue (Y/n)?  type Y

		if prompted a message like Did you mean zone [europe-west1-c] for instance: [custom-vm] (Y/n)?  type n

	- To see information about unused and used memory and swap space on your custom VM, run the following command:

		free
	- To see details about the RAM installed on your VM, run the following command:

		sudo dmidecode -t 17

	- To verify the number of processors, run the following command:

		nproc

	- To see details about the CPUs installed on your VM, run the following command:

		lscpu

	- To exit the SSH terminal, run the following command:

		exit

In this lab, you created several virtual machine instances of different types with different characteristics.

One was a small utility VM for administration purposes. 

You also created a standard VM and a custom VM. You launched both Windows and Linux VMs and deleted VMs.
