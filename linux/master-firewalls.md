---
title: Guide to Mastering Firewalls
date: 2024-03-07-2205 (March 8, 2024 10:05 PM)
tags: 
- Linux
- Firewall
- Security
---

# Guide to Mastering Firewall Configuration
- Read the classic documentation: [Netfilter](https://netfilter.org/documentation/index.html)

## MUST READ FIRST:
- `ufw` is just a persistent frontend to `iptables`
- LEARN `iptables` functionality (more difficult to grasp) - anything/everything else will sort of fall into place in time

  > learning the basics of iptables will allow you to not only implement custom and rock solid firewall rules, but it will give you a leg up on software IDF like psad as well.
  > <br><br>
  > The pros make basics second nature. That's how you become a pro.

### "Is there a way I learn these things like a pro?"
Thing is, it's really easy to make a mistake and render your system unusable (denying outbound to localhost, for instance). The nice thing about `iptables` is that if you do mess up, persistence isn't a problem and you're a reboot away from everything being back to normal.
 
- You also don't have to rely on app interfaces. If you can locate your fw config files, you can read those to get a decent understanding of what your sys is doing. Same deal for logs. You could briefly set your logging level to informational to see what's hitting and not hitting.
 
- Also, Netstat (`netstat`) is a good tool to help identify which ports are currently active, etc.
 
**To summarize,** and answer the question, 'Is there a way I learn these things like a pro?' Yes there is. 
- The amateur way is to fuck up repeatedly until things start working the way you want them to. 
- **The pro way way is to fuck up repeatedly until things start working the way you want them to, while keeping track of all the fuck ups.**
 
## The Practical Way
<!-- TODO: add guide here-->
1. Set up a LAN and play around with firewall rules to get quick feedback. 
- first figure out how iptables work and then try to figure out the nftables approach.
2. Next step: set up a simple web server on Amazon EC2 free tier and play with firewall rules over a VPC/Security group.

Just change stuff and see what happens
