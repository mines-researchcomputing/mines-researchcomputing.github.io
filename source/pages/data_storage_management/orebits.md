# Orebits Storage Platform

## Overview

The OreBits Storage Platform is an on-premise storage appliance hosted by IT, and was created to accommodate larger data storage needs at Mines.  OreBits is a “middle-of-the-road” solution, offering a balance between performance, protection and price.

If you would like more information or have questions, please see the Memorandum of Understanding below, or contact the RC Team for assistance.

If you are interested in purchasing additional storage for your group, please open a helpdesk ticket by using the [Research Consultation Request](https://outlook.office365.com/owa/calendar/ResearchComputingSupportTeamServices@mines0.onmicrosoft.com/bookings/).

## Features

* On-premise storage solution (required by some government and many industry contracts).
* Accessible on or off campus.
* Ability to share folders with Mines users.
* Purchased in increments of a terabyte.
* Utilize [Globus](https://globus.org) to transfer large files via gridFTP.
* Hardware:
	- OreBits operates on Linux and ZFS filesystem.
	- Configured as a ZFS RAIDZ2 with 2 spare drives.
	- Dual controller’s, power supplies, and network connections

Connectivity: Please see Memorandum of Understanding (MOU) for details of service.


## Billing 

RC has added full backup infrastructure to the OreBits service which serves to protect data in case of datacenter incidents or cryptolocker/malware attacks.  We still intend to provide the service “at-cost”; currently at $1.75/TB/mo.  This translates to $21/year/TB for fully replicated storage/snapshots/backup.

OreBits was designed as a “middle-of-the-road”, on-premise solution, offering a balance between performance, protection and price. Users only pay hardware and maintenance contract costs at a subscription rate of $1.75/TB/mo.  

If OreBits will be funded by a research grant, please contact [ORA](https://ora.mines.edu/) regarding proper appropriation of funds.

## Memorandum of Understanding (MOU)


Colorado School of Mines

IT / Research Computing (RC)

OreBits Storage Platform MOU

 

### 1.  INTRODUCTION
 

This is the memorandum of understanding (MOU) for OreBits.  It includes both a Service Level Agreement (services provided to the customer) and an End User Agreement (terms agreed to by the customer).

 

### 2.  DESCRIPTION OF SERVICE
 

The OreBits storage appliance is a cooperative funded model to support storage for the Mines research community.

#### 2.1 Service Classes:

There is currently one service class of storage available:

* Active
	* For data that is frequently written or read
	* Directly accessible (read/write) from computational resources within the Mines environment
	* Accessible from outside the Mines environment only via specific data transfer protocols (GlobusOnline) through designated gateway nodes
Not designed for highly I/O-intensive or massively parallel usage


#### 2.2 Service Details:

Currently, we only offer one storage service.  Storage space must be pre-purchased, with a minimum purchase of 1 TB for 1 year.  In addition to active storage service, it is recommended that you have a local backup copy of critical data.  We do not centrally backup the active storage device.

* Active – single copy on disk
	* Appropriate for non-critical data
	* Volumes are configured as RAID 6 (2 parity disks)
	* Some protection against accidental file deletion with local snapshots
	* No protection against catastrophic failure of the primary storage system
	* There is no central backup, please backup your data locally*
\*Backup services are scheduled for a future implementation

 

### 3.  END-USER EXPECTATIONS

#### 3.1 Usage:

See Appendix A for the End User Agreement, which must be acknowledged by the researcher / PI before a project can be initiated.

#### 3.2 User/Group Administration:

Every research group using OreBits should provide IT / RC with two official contacts: The Principal Investigator (PI) and an alternate Technical Point of Contact (POC) person.  The PI must be a Mines faculty member.  These two people are the only ones authorized to make changes to the set of users that comprise a group, to add or remove users on the access list, or to request other changes on behalf of the project.

Any change request that may impact a group will have to come through the PI or POC.  Without such approval, IT / RC will not act on group-level requests.  Change and service requests should be made through the Mines helpdesk ticketing system.

The PI and POC should verify their list of users on a yearly basis.  Note that while Mines Multipass password may be terminated after a person leaves the university, his or her files will be retained in OreBits and their ownership will be transferred to the PI.  This is done to ensure that shared files do not become unusable by a research group when one user account becomes inactive.  The PI is encouraged to purge out-of-date data periodically.  In the event that a PI leaves the university, the PI understands that the data may be purged to reclaim space.

OreBits relies on Mines Multipass credentials for access and authorization.  All users must have a valid Mines Multipass account which ensures that all users are Mines faculty, students, staff, or affiliates.  Note that a Mines faculty member may sponsor a non-Mines person as an affiliate, and then add them to the service.  IT / RC cannot act as the sponsor.

#### 3.4 Costs/Fees:

Mines users pay actual media costs for disk space.  Appendix B outlines the costs of the different services.

#### 3.5 Ownership of Media:

The physical media that the data resides on belongs to IT and may not be removed from OreBits


### 4.  SERVICE EXPECTATIONS

#### 4.1 Support levels:

Two levels of service are available:

Tier 1: Support is provided during business hours only, Monday through Friday, 8am to 5pm.
Tier 2: Support is provided from Monday through Friday 8am to 5pm, including after-hours support on those weekdays with “best effort”. There is no guarantee of weekend or holiday support, though IT / RC will try to respond to incidents impacting the operation of the service with “best effort”.
IT / RC will operate OreBits as a hybrid between the “Tier 1” and “Tier 2” service levels.  Customer-initiated support requests (e.g., group membership or permission changes) will be “Tier 1”.  The backend storage systems and gateway nodes will be “Tier 2”.

While every reasonable and good faith effort will be made to ensure the integrity, reliability and availability of OreBits and of the files stored on it, access to data in OreBits may be affected by circumstances outside of the control of  IT/ RC.

#### 4.2 Accessing storage:  

Active storage is directly accessible for read/write via NFS, CIFS, and ssh (scp, sftp, or rsync) from computational resources within the Mines environment, including login nodes.

To access active storage from outside of the Mines environment, it is necessary to use ssh or VPN connections.  Connections can be made through orebits2.mines.edu.  Additionally, data transfers can also be made utilizing gridftp/Globus; Globus will almost always provide much higher throughput and resiliency than all other connection options and is the preferred protocol for any but the smallest transfers.

#### 4.3 Duration of Service:

In the event that IT / RC ceases to provide OreBits or any comparable resource, IT / RC will give at least 90-day advance notice.  It will be the responsibility of the PI to transfer their data to other storage resources within that time window.

#### 4.4 Amount of storage available:

The service will be grown as needed to address demand.  IT / RC reserves the right to limit initial or additional allocations (on a temporary basis) to ensure the availability of the service to all who request it.

When it becomes more clear which services have the highest demand, IT / RC can purchase additional infrastructure to support those services.

#### 4.5 Reporting:

A report detailing the project’s current storage usage will be emailed to the PI and the POC upon request.

At least annually, IT / RC will provide a list of users to all PIs and POCs showing the user IDs for every user who has access to the project’s data.  The PI’s must affirm that those individuals should retain access and request the removal of access for individuals who no longer need it.

#### 4.6 Maintenance:

Planned maintenance of the IT / RC infrastructure, including OreBits hardware, will take place every second Thursday of the month at 5:30pm.  IT / RC reserves the right to change the window, always with advance notice.

#### 4.7 Change management:

IT / RC will make every effort to announce to all OreBits customers any significant changes to the system a minimum of 30 days in advance.  Exceptions may be critical security updates and bug fixes that improve the stability of the system.

#### 4.8 Performance:

IT / RC will provide expected throughput performance targets for each service type; however, since OreBits is a shared infrastructure actual performance may vary depending on workload from other customers.  When they become available, statistics on data performance and availability will be posted.

#### 4.9 Refunds:

We are not able to process refunds or pro-rate any fees for any time lost due to repairs or maintenance events (planned or otherwise), nor for any datacenter-related down time.

### 5.  HELP/SUPPORT REQUESTS
 

Orebits users may initiate support requests through the IT Help Desk ticketing system by placing a help ticket at [https://helpcenter.mines.edu](https://helpcenter.mines.edu).  While IT / RC will make every effort to respond to and resolve support requests, those support requests which require domain-specific knowledge or expertise may not be able to be handled by IT / RC alone.  In these cases, the support request may be forwarded to the POC for assistance.

 

### APPENDIX A:

End User Agreement

Colorado School of Mines OreBits Storage Use Agreement


As the PI, I understand that my use of and access to the digital storage facility known as the Colorado School of Mines OreBits shall be in accordance with all of the following stipulations:


____  The PI is responsible for ensuring data stored on the OreBits Platform complies with all institutional policies as well as all state and federal laws, including copyright, the Health Insurance Portability and Accountability Act (HIPAA), the Family Educational Rights and Privacy Act (FERPA), and the International Traffic in Arms Regulations (ITAR).  Please contact IT / RC for advisement of sensitive data

 

Colorado School of Mines IT Policies:
https://IT.mines.edu/POGO-Information-Technology
Colorado School of Mines Administrative Policies
https://IT.mines.edu/POGO-Policies-Governance
Health Insurance Portability and Accountability Act (HIPAA):
https://www.hhs.gov/ocr/privacy/hipaa/understanding/index.html
Family Educational Rights and Privacy Act (FERPA):
https://inside.mines.edu/FERPA
International Traffic in Arms Regulations (ITAR):
https://pmddtc.state.gov/regulations_laws/itar_official.html
 

____  If any of the files that I store on the OreBits Platform are subject to one or more agreements with any  Institutional Review Board (IRB), including but not limited to the IRB of Mines, then I will take full responsibility for ensuring full compliance with such agreement(s).

____  If I am collaborating with colleagues who are at institutions outside of the United States of America (that is, outside of both US states and US territories), then I will take full responsibility for ensuring that those colleagues do not access the OreBits themselves, but rather I and/or or other members of my team who are at US institutions will access the OreBits on behalf of the entire team.

____  I understand that, if and when I cease to be employed by and/or a student at an institution in the United States of America, then access to my files on OreBits will be available only to those of my collaborators who are employed by and/or students at US institutions.

____  If I am one of the Principal/Co-Principal investigators of a team, then I will take full responsibility for ensuring that any other members of the team are likewise in full compliance.

____  In the event that the OreBits Platform ceases providing storage or any comparable resource, then I will take full responsibility for transferring any and all relevant files to other storage resources, and in a timely manner.

____  I will take full responsibility for ensuring that I keep abreast of and comply with changes to any of the relevant laws, policies and circumstances described above.

 
### APPENDIX B:

Order Form: Please submit a [Mines Service Request](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/TicketRequests/NewForm?ID=4GCQlvW5OYk_&RequestorType=Service)


## Connection Information


### Instructions for Windows users

Open Windows Explorer, My Computer or My Documents.
Locate the option to map a network drive. The method for doing this will vary with different versions of Windows. Typically, it will be either:
     Options > Tools > Map Network Drive or This PC > Computer > Map Network Drive

A “Map Network Drive” (or similar) panel should appear. The first item identified will usually be the “Drive” letter to be assigned on your system. This will be the virtual drive letter that you will later use to access your files on the remote domain. Usually the last available drive letter will show up. You can any other unassigned drive letter in the drop-down list.
The second item requested is the “path” or “share” that tells your computer where to look on the network to find your files; this is the “full path to your files”. For Orebits user, this will be `\\orebits2.mines.edu\[share name]` or `\\orebits3.mines.edu\[share name]` brackets removed
If the “Reconnect at Login” box is checked, you should check or uncheck it as appropriate. For desktop machines constantly on the Mines network, this is a good choice. Laptops that travel off the Mines network are more problematic.
Press the “OK” or “Finish” button.
If the information you provided is correct and the drive is available, you will be prompted to enter your ADIT password (or username and password). Submit that information and press the “OK” button. You may need to enter your username as “ADIT\username” instead of just “username”– indicating the domain where your account is found (ADIT in this case).
If the system validates your username/password, the drive letter you selected will now be mapped and you should be able to navigate to that virtual drive and access your files, just as if it were a local drive on your computer.


### Instructions for macOS Users

To access a remote drive using macOS, determine the path to the remote network drive. The methods noted above may be useful for discovering the path if it is not already known.

Using the Mac Finder, click Go > Connect to Server.
Type the server address in this format:
     smb://orebits2.mines.edu/[share name]  brackets removed, or
     smb://orebits3.mines.edu/[share name]  brackets removed

If you are successful, click the [+] icon at right to save the address permanently.
Note: If asked for login credentials, use those for the system to which you are trying to connect, not your local Mac username and password (unless, of course, they are the same).

Add a shortcut in Finder by selecting Go > Computer. Then dragging and dropping the mounted folder anywhere to left side bar.

### Instructions for Linux Users:

There are many ways to mount network drives using a Linux system. Some Linux distributions have graphical clients for this purpose. Others require command-line solutions. In general, when mounting Windows shares, the “Samba” package will be used. For help with your particular Linux distribution, and a particular network file system, please submit a support request at Mines Help Center (https://helpdesk.mines.edu). Describe as completely as possible the variety of Linux you are using and the file system you are attempting to access. Or proceed using the hints found below.

#### Command Line
For Linux users comfortable with the terminal or shell interface, the simplest method for accessing the files involves using the Samba package’s “smbclient” executable, along with the cifs-utils package. They may need to be installed using the package manager appropriate to your Linux distribution, and can be used as follows:

     sudo smbclient -m SMB3 -U username -W adit //orebits2.mines.edu/[share name]  brackets removed
     sudo mount.cifs –verbose -o username=joeuser@adit,vers=3.0 //orebits2.mines.edu/[share name]  brackets removed Replace the “username” with your campus username, “server” with the server in question (Hornet for Z: drive, Files for Y: drive, orebits2.mines.edu for Orebits users), and “share” with the share that is given to you during account setup.

 

#### Graphical Interface

For Linux users more comfortable in the GUI environment, there are a variety of options, but the most commonly available is to use Nautilus (or “Files” in recent versions of Ubuntu). For most, this is by far the easiest way to connect to a Windows share in Linux.

Click on “Connect to Server” in the bottom of the left hand side bar (on Ubuntu 18.04, you will need to click on “Other Locations” first), and enter the server address in the format:

     smb://orebits2.mines.edu/[share name]  brackets removed

Enter. You will be asked to supply your ADIT username, domain name (ADIT), and ADIT password (these are highly likely to be the same login credentials as your MultiPass credentials). You will also specify how long the system should remember your ADIT password (choose “Remember forever” for maximum convenience). Then click Connect and your ADIT directory on Hornet will now be available to you via the Nautilus file manager.

In summary, this process involves a lot of variation between different Linux distributions and configurations. Let us know if you have problems with the instructions above and we will try to help if your version of Linux is one with which we are familiar.

