# Connecting to Mines@HPC Systems

We have two High Performance Computing (HPC) systems on campus: Wendian and Mio. This document describes how to log on to these systems, once an account has been granted to you. Mio is only available to legacy node owners, while Wendian is available to anyone with an index number that can be charged core hours. You can find more details below:

- [HPC Ticket Request](https://helpcenter.mines.edu/TDClient/1946/Portal/Requests/ServiceDet?ID=52356) - Use this link if you'd like to request an Account for Wendian for research or class use, or if you are a node owner on Mio and would like to add a Mio user to your existing partition.
- [Wendian Core-hour Pricing](https://ciarc.mines.edu/hpc-business-model/) - Use this link if you'd like to learn more about the Wendian HPC pricing model. 


The initial way to connect to HPC platforms is by using ssh. Unix and Unix-like operating systems, (macOS, Linux, etc) have ssh built in. If you are using a Windows-based machine then you must use a terminal package that supports ssh, such as Mobaxterm or Windows Subsystem for Linux. As of the April 2018 update, ssh is also available by default in Windows Powershell.

 All of the HPC platforms are behind the campus firewall. The firewall blocks access from off campus. Thus you need to be on campus to get access, or you need to use VPN software discussed on the [Global Protect VPN](https://helpcenter.mines.edu/TDClient/1946/Portal/KB/ArticleDet?ID=154280) page. To make ssh more convenient, we allow the use of ssh keys which will bypass needing to re-entering your password every time you login.

Assuming you are on campus and you are using a machine that supports ssh directly, you can get to Mio and Wendian, respectively, by entering the following in a terminal window:

	ssh username@mio.mines.edu
	ssh username@wendian.mines.edu
You will be asked for your password. The password required here is your Mines Single Sign-on (SSO) password. 

## Setting up SSH keys for easier login
SSH keys are a way to make login into remote servers, like our HPC clusters, much more convenient. This can work from both on and off campus.  The procedure discussed below will also allow you to log in by entering a passphrase every 8 hours.

The following is a quick guide for setting up keys to access wendian.mines.edu and mio.mines.edu from either an on-campus or VPN-connected off-campus Linux, macOS or Windows WSL (Windows Subsystem for Linux)  machine.

The first step is to generate an ssh key pair:

```
Generate your key pair (do not use an empty passphrase):

osage:~ username$  ssh-keygen -f $HOME/.ssh/forhpc
Generating public/private ed25519 key pair.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/username/.ssh/forhpc
Your public key has been saved in /Users/username/.ssh/forhpc.pub
The key fingerprint is:
SHA256:mCvTpd7lwcQcupXCXm67xpCGeFfygoVN5O0EFG5bHjI username@osage
The key's randomart image is:
+--[ED25519 256]--+
|       o=.       |
|       o.o       |
|       +E *      |
|      .=+%.+     |
|     .o+S=X      |
|    ..+=*O.      |
|    o.+oo+*      |
|     + . +oo     |
|      . ..+.     |
+----[SHA256]-----+
```


Copy the public key to Wendian:

```
cat ~/.ssh/forhpc.pub | ssh wendian.mines.edu "cat >> ~/.ssh/authorized_keys"
```

If you are a Mio user, copy the public key to Mio:

```
cat ~/.ssh/forhpc.pub | ssh mio.mines.edu "cat >> ~/.ssh/authorized_keys"
```

Add the following lines to your ~/.ssh/config file **on your local machine**. Create one if it does not exist. Replace “username” with your Mines username:

```
#Next 5 lines are optional if you don't do X-Windows.  The location of XAuthLocation might be different.
ForwardAgent yes
ForwardX11 yes
ForwardX11Trusted yes
ServerAliveInterval 60
PubkeyAcceptedKeyTypes=+ssh-dss
AddKeysToAgent yes 

Host mio
HostName 138.67.132.244
User username
Identityfile ~/.ssh/forhpc

Host wendian
HostName wendian.mines.edu
User username 
Identityfile ~/.ssh/forhpc
```

Set the permissions on your config file:

```
chmod 600 ~/.ssh/config
```

Run the following to set an 8-hour limit on your key:
```
ssh-add -t 28800 ~/.ssh/forhpc
```

Log in to Wendian or Mio using ssh:


	ssh mio
or
	ssh wendian
This login instance should not require you to enter a password.

## Connecting to HPC using Open OnDemand

Once you have logged into Wendian or Mio for the first time using ssh, this sets up your home directory in order to use our web application interface for HPC, powered by the [Open OnDemand](https://openondemand.org/) application.

To use the Open OnDemand interface, login using your Mines SSO credientials:

- Mio: [https://mio-ondemand.mines.edu](https://mio-ondemand.mines.edu)
- Wendian: [https://wendian-ondemand.mines.edu](https://wendian-ondemand.mines.edu)


