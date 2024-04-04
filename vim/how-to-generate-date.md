---
id: how-to-generate-date
aliases: []
tags:
  - How-To
date: 2024-04-03T22:01
title: How to Generate (Format) Date and Time
---
<!-- 2024-04-03 (April 03, 2024 10:01:55 PM) -->

|   Format String   |         Example Output               |
| ----------------- | ---------------------------------    |
| %c                | Thu 27 Sep 2007 07:37:42 AM EDT      |
| %a %d %b %Y       | Thu 27 Sep 2007                      |
| %b %d, %Y         | 	Sep 27, 2007                       |
| %H:%M:%S          | 	07:36:44                           |
| %T                | 	07:38:09                           |
| %m/%d/%y          | 	09/27/07                           |
| %y%m%d            | 	070927                             |
| %x  %X (%Z)       | 	09/27/2007 08:00:59 AM (EDT)       |
| %Y-%m-%d          | 	2016-11-23                         |
| %F                | 	2016-11-23 (works on some systems) |
| %d/%m/%y %H:%M:%S | 	27/09/07 07:36:32                  |

- generate: `2024-04-03-2156 (April 03, 2024 09:56:19 PM)`
```bash
:pu=strftime('%Y-%m-%d-%H%M (%B %d, %Y %X)')
# or (%F works only on some systems)
:pu=strftime('%F-%H%M (%B %d, %Y %X)')
```

- using `:r!date`
```bash
Wed Apr  3 10:12:11 PM +08 2024
```

- using `:.!date`
```bash
Wed Apr  3 10:12:17 PM +08 2024
```

