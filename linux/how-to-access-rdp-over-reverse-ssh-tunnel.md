---
tags:
  - How-To
date: 2024-05-02T22:24
---
<!-- 2024-05-02-2224 (May 02, 2024 10:24:41 PM) -->

# How to Access RDP over a reverse SSH Tunnel

*Implement a reverse SSH tunnel and then forward any service to/from another machine in the same network.*

For the purpose of this exercise we'll have a remote Windows machine we want to access using RDP, and in the same remote network behind a firewall we'll have a linux machine that we'll use to setup the tunnel and forward the connection. We'll call these machines "windowspc" and "linuxpc" respectively.

We will require a publicy-accessible machine through which to relay our packets; we'll call this machine "relay". This will also be running linux. For the purpose of this exercise we'll assume that it's wide open, but in practice it should be firewalled as tightly as possible for whatever clients you are connecting from. In my case, I have restricted the packets to be only from a particular subnet; I did this at the router level so the relay machine doesn't know or care about these restrictions. I also have the router setup to NAT unusual ports to the relay just as an extra minor level of security.

The windowspc shouldn't have any configuration applied to it, except to allow rdp from local network. Note that this happens by default on port 3389, so we'll need to forward to that port on the windows machine when we get to that.
Setup the Reverse Tunnel for direct SSH access

We don't strictly need this for our end goal of an rdp relay, but it's still useful to both test the reverse tunnel, and also to allow us to remotely administer the linux pc if we should need to. This step will demonstrate the basic concepts of the tunnel without the added complexity of the service forward. The ssh key generation and transfer is still required for all services though, so don't skip that part.

We'll need to setup the reverse ssh tunnel to the linuxpc. To do this, ensure openssh-server is installed on the linuxpc, and the relay. We assume the relay's SSH server is listening on port 2022.

On the linuxpc, we need to setup the ssh keys to access the relay without a password:
```bash
ssh-keygen
ssh-copy-id -p user@relay.public.ip
```

- On the linuxpc, we run the following command to setup the tunnel:
```bash
ssh -f -N -R 2222:localhost:22 -p 2022 -o ServerAliveInterval=60 user@relay.public.ip
```

The options to ssh are as follows:
```
    -f : Fork on tunnel creation (go to background)

    -N : Don't create a prompt on the remote machine

    -R <inport>:<IP>:<outport> : Create a reverse tunnel, where: * <inport> is the port we reference when accessing the tunnel * <IP> is the IP we use when accessing the tunnel * <outport> is the port we want on the machine where the tunnel originated.

    -p <number> : The port on the relay we need to access the ssh server **on the relay**

    -o ServerAliveInterval <seconds> : We use this so when we create the tunnel it doesn't die after a period of inactivity, useful when starting from a systemd script for example

    user@ip - The user on the relay machine that the tunnel will be created for.
```

If that succeeds, then I should be able to open a shell on the relay machine, and perform the following:

user@relay:~> ssh -p 2222 linuxuser@localhost
linuxuser@linuxpc:~> 

Note that the port 2222 on localhost at relay has been forwarded to the port 22 on the linuxpc, using the reverse ssh tunnel! So we can now access the linuxpc, which is sitting behind a firewall and possibly on an unroutable network from the internet by logging in to the relay and then doing a simple 'ssh localhost'.
SSH to remote PC without logging in to relay

That's all well and good, but what if we want to just login directly to the linuxpc without first SSH'ing to the relay? Well, with the magic of socat we can manage that. Socat creates a bidirectional network forward, and is quite simple to setup.

Assuming that port 12322 is open on the relay, we setup a socat port forward on the relay computer:

user@relay:~> socat TCP-LISTEN:12322,fork TCP:localhost:2222

This sets up the port forward on the relay computer, so we can now access the linuxpc from anywhere in the world!

anyuser@anywhere:~> ssh -p 12322 linuxuser@relay.public.ip
linuxuser@linuxpc:~> 

Forward RDP to the Windows computer

So now we have all the tools to do the entire chain from an arbitrary computer on the internet to the windows PC inside the firewall. We need to make a couple of socat forward instances (one on the relay and one on the linuxpc), and run them over a reverse ssh tunnel.

I'll just write the commands without too much explanation. The trick is making sure your ports line up, and that when you do the socat you can't forward to the same port you're listening on for the reason that the port is already in use when you're listening on it (remember it's a bidirectional forward).

On the linux PC, setup a reverse SSH tunnel, and a socat forward to the windows pc:

linuxuser@linuxpc:~> ssh -f -N -R 33389:localhost:33389 -p 2022 user@relay.public.ip
linuxuser@linuxpc:~> socat TCP-LISTEN:33389,fork TCP:windowspclocal:3389

Then we setup a socat from the relay to the tunnel port of 33389:

user@relay:~> socat TCP-LISTEN:12389,fork TCP:localhost:33389

For clarity, I've set the listen port to be 12389, to differentiate from the 3389 of the linuxpc socat, but we could make it any port. Now from anywhere in the world, we should be able to RDP to the windows PC, using the normal RDP clients, just make sure to set the port to 12389. When you open your rdp client, set the target to be relay.public.ip:12389, and use the windows computer credentials to login.
Security Implications

I'm not a security expert by any means, but note that there are a couple of security issues here. Mostly, we are opening the windows PC to anywhere on the internet, so if you're worried about that (you should be!), lock down your relay firewall to only accept connections from either a certain computer or subnet as appropriate.

In my case I've set the relay user to root, but as I've set it up as a VM and the only thing it does is to run the relay, I'm not overly concerned about the security of the root user. If you set your relay ssh server to only allow connections using an ssh key pair, that will also help with some security.

There's also a whole bunch of ssh key transfer that happens between the computers to make access more convenient, but that's beyond the scope of this post - it's not too hard to figure out where those need to be done and were omitted for clarity of the ssh/socat process.

I've wrapped everything up in a bunch of systemd services so they all start at boot, the extra little trick for that is for the ssh reverse tunnel you'll need to set `Type=forking` in the service otherwise systemd thinks it's hung and tries to restart it constantly. Alternatively, it should be possible remove the '-f' ssh option instead, but I haven't tested that explicitly.
Summary

You should now be able to setup a reverse tunnel to/from a remote computer that's on a NAT'ed, non-routable network, and then forward that connection to another computer inside that same network for arbitrary service access from the internet.

Thanks for coming to my TED talk :-).

[EDIT] Fixed a couple of typos, added some clarification; added systemd stuff; edited to make it read a bit better.
