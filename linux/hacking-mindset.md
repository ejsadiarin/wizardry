---
title: Hacking Mindset
date: 2024-02-06-1837 (February 6, 2024 6:37 PM)
tags:
- Hacking
- Starters
- Mindset
---

# Hacking mindset for starters
- identify open ports
- once identified, identify the services running behind the ports
- what the person would be able to do depends on whats running there, if anything.
> note that, what a person can do depends entirely on what service they're able to reach behind your http port. You could have a web server there, an ftp server, or a tea pot. 
> the possibility of exploitation is fully dependent on the services running behind the ports or you are hosting behind them.
> The port itself does not matter for this, nor does it matter if the connection between client and server is encrypted. 
> All what matters is that there is some application which accepts data on this port and acts on these data in an insecure way.

# References:
- [https://security.stackexchange.com/questions/270701/could-someone-hack-into-my-website-through-http-ports](https://security.stackexchange.com/questions/270701/could-someone-hack-into-my-website-through-http-ports)
