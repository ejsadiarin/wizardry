---
id: start-devops-with-this
aliases: []
tags:
  - DevOps
  - How-To
date: 2024-03-04-1608 (March 04, 2024 04:08 PM)
title: Start DevOps
---
# Starting DevOps (Practical)
- Full Roadmap: [https://roadmap.sh/devops](https://roadmap.sh/devops)

1. Get a lab environment for yourself. A Dell Optiplex will do. Maybe 2.
2. Put a hypervisor on it. Proxmox.
3. Install Linux VMs. Ubuntu is a good start.
4. Setup a kubernetes cluster
5. Write a very simple hello world app, deploy to said cluster manually
6. Automate the deployment process
7. Setup splunk on another VM so you can get logs.
8. Modify your simple app to send logs to splunk

- For Linux stuff. After installing ubuntu, look up a video on some basic commands. (Dont install desktop version, just use command line):
`cd, ls, pwd, cat, less, more, tail, passwd, dmidecode, ip, su and sudo`

- Learn what goes under /usr. /home, /root, /dev, /etc.

- Create a new user account, give it sudo access but lock it down to only specific commands it can sudo.

- Learn how to use vi or vim to modify files in the command line. Or try nano. I prefer vim, its what I learned on.

- Generate an ssh-rsa key with a pass phrase on your local machine, copy the public key to the ubuntu VM to make authenticating/ssh to the server more secure.

Just google all these things and learn how to do them

Theres also a devops roadmap somewhere. Great pathways.

# References
- [https://roadmap.sh/devops](https://roadmap.sh/devops)
