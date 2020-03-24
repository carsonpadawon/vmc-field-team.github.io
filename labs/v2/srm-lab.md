---
layout: single
title: "Site Recovery Manager (SRM) Lab Manual"
date: 2018-07-20
tags: workshop
toc: true
classes: wide
author_profile: false
comments: true
categories: labs
---

## Introduction

In this lab you will pair up with another student group in order to simulate the setup and configuration tasks for VMware Site Recovery Manager

## Activate Site Recovery Add On

Important Instructions for Site Recovery Exercises

PLEASE BE AWARE THAT THESE EXERCISES MUST BE PERFORMED FROM THE ASSIGNED HORIZON DESKTOP YOUR INSTRUCTORS ASSIGNED. IF YOU TRY TO PERFORM SOME OF THE EXERCISES OUTSIDE OF THE HORIZON SESSION YOU WILL EXPERIENCE SOME FAILURES.

### Activate Site Recovery

![SRM1](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM1.jpg)

1. Click on the *Add Ons* tab
2. Under the Site Recovery Add On, Click the *ACTIVATE* button

    ![SRM2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM2.jpg) 

3. In the pop up window Click *ACTIVATE* again

    ![SRM3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM3.jpg)

## What is VMware Site Recovery

![SRM4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM4.jpg)

VMware Site Recovery brings VMware enterprise-class Software-Defined Data Center (SDDC) Disaster Recovery as a Service to the AWS Cloud. It enables customers to protect and recover applications without the requirement for a dedicated secondary site. It is delivered, sold, supported, maintained and managed by VMware as an on-demand service. IT teams manage their cloud-based resources with familiar VMware tools without the difficulties of learning new skills or utilizing new tools and processes.

Wait until the Site Recovery Add On has been activated. This process should take ~10 minutes to complete.

VMware Site Recovery is an add-on feature to VMware Cloud on AWS. VMware Cloud on AWS integrates VMware's flagship compute, storage, and network virtualization products: VMware vSphere, VMware vSAN, and VMware NSX along with VMware vCenter Server management. It optimizes them to run on elastic, bare-metal AWS infrastructure. With the same architecture and operational experience on-premises and in the cloud, IT teams can now get instant business value via the AWS and VMware hybrid cloud experience.

The VMware Cloud on AWS solution enables customers to have the flexibility to treat their private cloud and public cloud as equal partners and to easily transfer workloads between them, for example, to move applications from DevTest to production or burst capacity. Users can leverage the global AWS footprint while getting the benefits of elastically scalable SDDC clusters, a single bill from VMware for its tightly integrated software plus AWS infrastructure, and on-demand or subscription services like VMware Site Recovery Service. VMware Site Recovery extends VMware Cloud on AWS to provide a managed disaster recovery, disaster avoidance and non-disruptive testing capabilities to VMware customers without the need for a secondary site, or complex configuration.

VMware Site Recovery works in conjunction with VMware Site Recovery Manager and VMware vSphere Replication to automate the process of recovering, testing, re-protecting, and failing-back virtual machine workloads. VMware Site Recovery utilizes VMware Site Recovery Manager servers to coordinate the operations of the VMware SDDC. This is so that, as virtual machines at the protected site are shut down, copies of these virtual machines at the recovery site startup. By using the data replicated from the protected site these virtual machines assume responsibility for providing the same services.

![SRM5](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM5.jpg)

VMware Site Recovery can be used between a customers datacenter and an SDDC deployed on VMware Cloud on AWS or it can be used between two SDDCs deployed to different AWS availability zones or regions. The second option allows VMware Site Recovery to provide a fully VMware managed and maintained Disaster Recovery solution. Migration of protected inventory and services from one site to the other is controlled by a recovery plan that specifies the order in which virtual machines are shut down and started up, the resource pools to which they are allocated, and the networks they can access.

VMware Site Recovery enables the testing of recovery plans, using a temporary copy of the replicated data, and isolated networks in a way that does not disrupt ongoing operations at either site. Multiple recovery plans can be configured to migrate individual applications or entire sites providing finer control over what virtual machines are failed over and failed back. This also enables flexible testing schedules. VMware Site Recovery extends the feature set of the virtual infrastructure platform to provide for rapid business continuity through partial or complete site failures.

## Create a Cross SDDC VPN

We will be setting up an IPSEC VPN connection between your VPC and the VPC of the person you were paired with. **Each student** needs to complete the steps for **your** SDDC.

![SRM6](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM6.jpg)

1. Navigate back to *ALL SDDCs* and click on *VIEW DETAILS* for **the paired student's** SDDC.  For example, if you are *Student-1*, you will view the details for the *Student-2* SDDC.

    ![SRM7](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM7.jpg)

2. Click on the *Networking & Security*

    ![SRM8](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM8.jpg)

    In the Management Gateway section, make a note of the *VPN Public IP* and the *Infrastructure Subnet* CIDR

    ![SRM9](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM9.jpg)

### Create a Policy Based VPN
**Navigate back to the *VIEW DETAILS* section of *YOUR SDDC* to create the VPN to the *Paired Student's SDDC***

On the Networking & Security tab
1. Click *VPN* on the left-hand menu under Network
2. Click *Policy Based*
3. Click on *ADD VPN*

    ![SRM10](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM10.jpg)

    **Note that all workloads deployed in the module will be deployed to the *Demo-Net* Network Segment that was created in the *Working with your SDDC* module**

    Fill in the following information:
4. Enter *Student # MGMT GW* (where # is your peer's student number) in the *Name* field
5. Select *Public IP* for *Local IP Address*
6. Enter the *VPN Public IP* address of the persons Management Gateway you were paired with for *Remote Public IP*
7. Enter the paired student's Demo-Net Network Segment CIDR *10.10.x.0/24* where **X** is the paired student's number and enter the paired student's *Infrastructure Subnet CIDR* you noted earlier in the *Remote Networks* field
8. Click the *Local Networks* field and select both *Demo-Net* and *Infrastructure Subnet*
9. Enter **VMware1!** in the *Preshared Key* field
10. Click *SAVE*

    ![SRM11](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM11.jpg)

    When both you and the person you were paired with have completed these steps you should see the status of the VPN turn to **Success**

## Deploy a Windows Jump Host
**You will need to deploy a jump host and set up a second VPN from a jump host to the SDDC for this workshop setup to work.  This is not normally needed when setting up you on-premises environment to VMC, but it is need for this workshop.**

### Create a New Content Library
![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost1.jpg)

From **your SDDC** copy the cloudadmin password and click *OPEN VCENTER*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost2.jpg)

Enter the cloudadmin credentials and click *LOGIN*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost3.jpg)

In the vSphere Client, click the *Menu* dropdown and click *Content Libraries*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost4.jpg)

Click the **+** to add a new Content Library

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost5.jpg)

1. Enter *Windows-Content-Library* in the *Name* field
2. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost6.jpg)

3. Click the radio button next to *Subscribed content library*
4. Enter *https://s3-us-west-2.amazonaws.com/s3-vmc-iso/lib.json* in the *Subscription URL* field
5. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost7.jpg)

If you get a warning, click *YES*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost8.jpg)

6. Select *Workload Datastore* for the storage location
7. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost9.jpg)

8. Click *FINISH*

### Deploy a VM from Content Library

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost12.jpg)

Click on your *Windows-Content-Library* that you just created 

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost13.jpg)

Click *OVF & OVA Templates* (wait a few seconds if all of the templates aren't listed, particularly the Windows2012r2 template)

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost11.jpg)

1. Right-click on the *Windows10* template 
2. Select *New VM from This Template...*

If you do not have the menu item to deploy a New VM from this template, Click the *ACTIONS* dropdown menu for the content library and click *Synchronize* (shown below) and wait until the sync is complete.

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost10.jpg)

### Deploy a Windows VM

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost14.jpg)

1. Enter *Jump Host* for the *Virtual machine name*
2. Select the *Workloads* folder
3. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost15.jpg)

4. Select the *Compute-ResourcePool*
5. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost16.jpg)

6. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost17.jpg)

7. Select the *WorkloadDatastore*
8. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost18.jpg)

9. Select *Demo-Net* from the *Destination Network* dropdown list  (this network was created in the Working With Your SDDC module)
10. Click *NEXT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost19.jpg)

11. Click *FINISH*

### Request a Public IP for the Jump Host

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost20.jpg)

**Navigate to the Jump Host VM you just deployed, power it on and note the IP Address**

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost21.jpg)

1. Back in your SDDC, navigate to *Networking & Security*
2. Click *Public IPs* in the left-hand navigation menu
3. Click *REQUEST NEW IP*
4. Enter *Jump Host* in for the *Notes*
5. Click *SAVE*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost22.jpg)

6. Note the *Public IP* that was just provided
7. Click *NAT* in the left-hand navigation menu

### Create a NAT rule for the Jump Host

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost23.jpg)

1. Click *ADD NAT RULE*
2. Enter *To Jump Host* in the *Name* field
3. Enter the noted Public IP in the *Public IP* field (it should be pre-populated)
4. Enter the noted internal IP Address for your Jump Host VM in the *Internal IP* field
5. Click *SAVE*

### Create a Compute Gateway FW Rules to allow RDP to the Jump Host VM

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost24.jpg)

1. Click *Gateway Firewall* in the left-hand navigation menu
2. Click *Compute Gateway*
3. Click *ADD RULE*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost25.jpg)

We will add a rule that will allow us to connect to our jump host via RDP

4. Enter *RDP to Jump Host* in the *Name* field
5. Hover over the *Destinations* field and click the pencil icon

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost26.jpg)

We will add a group that contains our jump host's internal IP address

6. Click "ADD GROUP"

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost27.jpg)

7. Enter *Jump Host* in the *Name* field
8. Click *Set Members*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost28.jpg)

9. Click IP/MAC Addresses
10. Click in the IP/MAC Addresses field and enter the interal IP of your jump host
11. Click *APPLY*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost29.jpg)

Note that under Compute Members it now shows 1 IPs/MACS

12. Click *SAVE*
13. Click *APPLY*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost30.jpg)

14. Hover over the *Services* field and click the pencil icon

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost31.jpg)

15. Enter *RDP* into the *Search* field
16. Click the checkbox next to *RDP*
17. Click *APPLY*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost32.jpg)

18. Click *PUBLISH* to publish this new rule

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost33.jpg)

We will add another rule that will allow outbound traffic from our jump host

19. Click *ADD RULE*
20. Enter *Jump Host Outbound* into the *Name* field
21. Select the *Jump Host* group you created previously for *Sources*
22. Click *PUBLISH*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost34.jpg)

23. Click *Management Gateway*
24. If your *vCenter Inbound* rule does not have *ICMP ALL* under *Services*, modify the rule to allow that service and **PUBLISH** the rule when complete.

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost37.jpg)

25. Click *Settings* for your SDDC
26. Click the *>* arrow next to *vCenter FQDN*
27. Click *EDIT*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost38.jpg)

28. Select the *Private IP* in the *Resolution Address* dropdown
29. Click *SAVE*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost36.jpg)

Open **REMOTE DESKTOP CONNECTION** from **YOUR** desktop.  Do not open it from within the virtual desktop.

30. Enter the *Public IP Address* that was allocated for your jump host
31. Log in with *Administrator/VMware1!*
32. Click *Connect* (Click *YES* if you get a certificate error)

### Install Firefox or Chrome into the Windows VM

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost39.jpg)

Internet Explorer does not work well, so you'll need to download and install Firefox or Chrome into your Jump Host VM.

If **Server Manager** does not automatically open, open it.

1. Click *Local Server*
2. Click *On* next to *IE Enhanced Security Configuration*

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/JumpHost40.jpg)

3. Click the radio button next to *Off* under *Administrators*
4. Click the radio button next to *Off* under *Users*
5. Click *OK*

### Test Connectivity

Download and install Firefox or Chrome in your Jump Host

Open your browser and enter the FQDN, found under Settings, for your vCenter server in your SDDC

Open another tab and enter the FQDN for the vCenter for your paired SDDC

**If you can navigate to both vCenters via their FQDN's, all connectivity is established correctly. The remainder of the work can be completed from your Remote Desktop connection to your Jump Host.**

## Prepare and Pair Site Recovery

### Firewall Rules for Site Recovery

We will need to create Management Gateway firewall rules to allow for additional management gateway traffic including Site Recovery and vSphere Replication traffic. This needs to be done in both SDDCs.

![SRM12](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRMNew1.jpg)

1. In your SDDC, click *Networking & Security*
2. Click *Gateway Firewall* in the left-hand navigation menu
3. Click *Management Gateway*
4. Add the additional **four** rules that are shown above. Ask your instructor if you need assistance with creating the management gateway firewall rules. 
5. Click *PUBLISH*

### VMware Site Recovery - Site Pairing

![SRM18](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM18.jpg)

You will be pairing to the other SDDC that is in your workshop organization.  Before beginning, each student needs to navigate to their partner's SDDC and save the vCenter FQDN and cloudadmin password.

1. Click *Settings* for your **partner's SDDC**
 
    The username on both sides (yours and your peer) will always be *cloudadmin@vmc.local*

2. Copy your partner's cloudadmin password and save it to notepad
3. Copy your partners vCenter FQDN.  Before saving it to notepad, change the format from what is displayed to what will be used, as shown below:

    *DISPLAYED*:

    ```link
    https://vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com/ui
    ```

    *USED*:

    ```link
    vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com
    ```

![SRM16](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM16.jpg)

***IMPORTANT NOTE*: Only one person can do the Site Pairing exercise. Please decide between you and your partner who performs this step.**

1. Navigate to the SDDC of the student that will do the site pairing and click *Add Ons*
2. Click *OPEN SITE RECOVERY* (*If necessary, login with the cloudadmin credentials for that SDDC*)


    ![SRM17](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM17.jpg)

3. Click *NEW SITE PAIR*

    ![SRM19](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM19.jpg)
4. Enter the *vCenter FQDN* of your partner's SDDC in the format **vcenter.sddc-xx-xxx-xx-xx.vmc.vmware.com** in the *PSC host name* field
5. Enter *cloudadmin@vmc.local* in the *User name* field
6. Enter the *cloudadmin password* of your partner's SDDC into the *Password* field
7. Click *NEXT*

    ![SRM20](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM20.jpg)
8. Click the *top-level checkbox* to select all Services
9. Click *NEXT*

    ![SRM21](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM21.jpg)
10. Click *FINISH*

    ![SRMNew2](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRMNew2.jpg)

11. Click *VIEW DETAILS*

    ![SRMNew3](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRMNew3.jpg)

12. Enter *cloudadmin@vmc.local* for the *User name*
13. Enter the cloudadmin password for your **partner's SDDC** for the *Password*
14. Click *LOG IN*

    ![SRMNew4](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRMNew4.jpg)

Once logged in to your partner's SDDC, you will see the *Site Pair Summary*

## Configure Mappings
### Configure Network Mappings

![SRM22](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM22.jpg)

1. Click *Network Mappings* in the left pane of the Site Recovery page
2. Click *+ NEW*

    ![SRM23](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM23.jpg)
3. Select *Prepare mappings manually*
4. Click *NEXT*

    ![SRM24](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM24.jpg)
5. Expand *SDDC Datacenter* on both sides
6. Expand *VMC Networks* on both sides
7. Click the *checkbox* next to *Demo-Net* on the left
8. Click the *radio button* next to *Demo-Net* on the right
9. Click *ADD MAPPINGS*

    ![SRM24-a](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM24-a.jpg)

10. Click the *checkbox* next to *sddc-cgw-network-1* on the left
11. Click the *radio button* next to *sddc-cgw-network-1* on the right
12. Click *ADD MAPPINGS*

    ![SRM24-b](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM24-b.jpg)

13. Ensure both network mappings have been created
14. Click *NEXT*

    ![SRM25](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM25.jpg)

    DO NOT select anything in Reverse Mappings

15. Click *NEXT*

    ![SRM26](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM26.jpg)

16. Click *NEXT*

    ![SRM27](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM27.jpg)

17. Click *FINISH*

### Folder mappings

![SRM28](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM28.jpg)

1. Select *Folder Mappings* in the left pane
2. Click *+ NEW

    ![SRM29](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM29.jpg)
3. Select *Prepare mappings manually*
4. Click *Next*

    ![SRM30](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM30.jpg)
5. Expand *vcenter...* on both sides
6. Expand *SDDC-Datacenter* on both sides
7. Click the *checkbox* next to *Workloads* on the left
8. Click the *radio button* next to *Workloads* on the right
9. Click *ADD MAPPINGS*

    ![SRM30-a](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM30-a.jpg)

10. Click *NEXT*

    ![SRM31](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM31.jpg)

    DO NOT select any Reverse mappings

11. Click *NEXT*

    ![SRM32](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM32.jpg)

12. Click *FINISH*

### Resource Mappings

![SRM33](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM33.jpg)

1. Click *Resource Mappings* in the left pane
2. Click *+ NEW*

    ![SRM34](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM34.jpg)

3. Expand *SDDC-Datacenter* on both sides
4. Expand *Cluster-1* on both sides
5. Click the *checkbox* next to *Compute-ResourcePool* on the left
6. Click the *radio button* next to *Compute-ResourcePool* on the right
7. Click *ADD MAPPINGS*

    ![SRM34-a](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM34-a.jpg)
    
8. Click *NEXT*

    ![SRM35](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM35.jpg)
    
    DO NOT select any reverse mappings

9. Click *NEXT*

    ![SRM36](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM36.jpg)

10. Click *FINISH*

### Storage Policy Mappings

![SRM37](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM37.jpg)

1. Select *Storage Policy Mappings* in the left pane
2. Click *+ NEW*

    ![SRM38](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM38.jpg)

3. Select *Prepare mappings manually*
4. Click *NEXT*

    ![SRM39](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM39.jpg)

5. Click the *checkbox* next to *vSAN Default Storage Policy* on the left
6. Click the *radio button* next to *vSAN Default Storage Policy* on the right
7. Click *ADD MAPPINGS*

    ![SRM39-b](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM39-b.jpg)

8. Click *NEXT*

    ![SRM40](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM40.jpg)

9. Click the *checkbox* next to *vSAN Default Storage Policy* for Reverse mappings
10. Click *NEXT*

    ![SRM41](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM41.jpg)

11. Click *FINISH*

### Placeholder Datastores

![SRM42](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM42.jpg)

1. Select *Placeholder Datastores* in the left pane
2. Click *+ NEW*

    ![SRM43](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43.jpg)

3. Click the *checkbox* next to *WorkloadDatastore*
4. Click *ADD*

    ![SRM43-a](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43-a.jpg)

For Placeholder Datastores, you must select a datastore at each side, so you'll repeat the same steps for the other vCenter.

5. Click the *vCenter entry* for the vCenter that you are **paired with**
6. Click *+ NEW*

    ![SRM43-b](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM43-b.jpg)

7. Click the *checkbox* next to *WorkloadDatastore*
8. Click *ADD*

## Create VMs to use wtih Site Recovery

You will now create four VMs to use for Site Recovery.

Navigate back to the vSphere client for ***YOUR SDDC***

If you need to log back in to your SDDC thru the VMC console, use the *cedxx@vmware-hol.com* userid provided to you at the beginning and the password of *VMware1!*

![SRM70](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM70.jpg)

TEST
    ![SRM71](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM71.jpg) 

TEST
    ![SRM72](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM72.jpg) 

TEST
    ![SRM73](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM73.jpg)

TEST
    ![SRM74](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM74.jpg)

TEST
    ![SRM75](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM75.jpg)

TEST
    ![SRM76](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM76.jpg)

TEST
    ![SRM77](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM77.jpg)

TEST
    ![SRM78](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM78.jpg)

## SRM - Protect a VM

![SRM44](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM44.jpg)

1. Select a VM to replicate and right-click
2. Select *All Site Recovery actions*
3. Click *Configure Replication*

    *NOTE*: You may need to log in to the paired site (This is your partner's site), make sure you use *cloudadmin@vmc.local* and get your partner users password. After entering you may need to repeat this step.

    ![SRM45](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM45.jpg)
4. Click *Next*

    ![SRM46](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM46.jpg)
5. Select the Target Site
6. If not logged in you may need to login (Remember this is your partner's site not yours)

    ![SRM47](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM47.jpg)
7. Enter your partners site credentials

    ![SRM48](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM48.jpg)
8. Leave all defaults and click *Next*

    ![SRM49](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM49.jpg)
9. Leave the default *Datastore Default* as the VM Storage Policy
10. Select *WorkloadDatastore*
11. Click *Next*

    ![SRM50](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM50.jpg)
12. Leave the default 1 hour for Recovery Point Objective, RPO can be as low as 5 minutes, as high as 24 hour
13. Click *Next*

    ![SRM51](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM51.jpg)
14. Select *Add to new protection group*
15. Name your Protection Group *PG#* (where # is your student number)
16. Click *Next*

    ![SRM52](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM52.jpg)
17. Select *Add to new recovery plan*
18. Name your Recovery Plan **RP#** (where # is your student number)
19. Click *Next* button

    ![SRM53](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM53.jpg)
20. Click *Finish*

## Perform a Recovery Test

![SRM54](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM54.jpg)

1. In the VMware Cloud on AWS portal, click the *Add Ons* tab
2. Click on *Open Site Recovery* (You may need to allow Pop-ups in browser)

    ![SRM55](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM55.jpg)
3. In the Site Recovery window, click *Open*

    ![SRM56](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM56.jpg)
4. Click on *Recovery Plans*

    ![SRM57](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM57.jpg)
5. Click on your protection group *PG#* (where # is your student number)

    ![SRM58](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM58.jpg)
6. Click on *Recovery Plans*
7. Click on *RP#* which should be your Recovery Plan you created in a previous step

    ![SRM59](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM59.jpg)
8. Click the *Test* button

    ![SRM60](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM60.jpg)

9. Leave all defaults and click *Next* button

    ![SRM61](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM61.jpg)
10. Click *Finish* button

    ![SRM62](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM62.jpg)
11. Follow the progress in the Recent Tasks area at the bottom of your window

    ![SRM63](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM63.jpg)
12. Notice the test has successfully completed
13. Click the *Cleanup* button to clean up the activity and return the environment to its normal state

    ![SRM64](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM64.jpg)
14. Click *Next*

    ![SRM65](https://s3-us-west-2.amazonaws.com/vmc-workshops-images/srm-lab/SRM65.jpg)
15. Click *Finish*, the environment will now be clean. Please note that during testing, your replications protecting your VM's is not interrupted