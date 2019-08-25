---
layout:     post
title:      "RTEMS + lwIP"
subtitle:   ""
date:       2019-08-24 
author:     "Andy Liu"
header-img: "img/post-bg-unix-linux.jpg"
catalog: true
tags:
  - RTEMS
  - lwIP
  - µC/OS
---

> This document is not completed and will be updated anytime.

## RTEMS + lwIP

I have been studying [**RTEMS (Real-Time Executive for Multiprocessor Systems)**](https://www.rtems.org) for a few years in my spare time. It is such a well-designed real-time operating system. The one major thing that I don't feel very happy with is its original TCP/IP stack. Basically, it is a port of an old FreeBSD TCP/IP stack. I have had quite a few difficulties in writing network device drivers for it and also found some potential bugs. [RTEMS LibBSD](https://github.com/RTEMS/rtems-libbsd) is another option. However, it seems like an overkill to me, because what I am actually looking for is more like a lightweight high-performance TCP/IP stack, which in my mind should be mainly designed for embedded applications. 

[**lwIP**](https://savannah.nongnu.org/projects/lwip/) is a great fit! It is open-source, mature and designed for embedded systems when originally developed. In addition, I see that it has been used in some other real-time operating systems that I'm familiar with as the default TCP/IP stack, like [**RT-Thread**](https://en.wikipedia.org/wiki/RT-Thread) and [**SylixOS**](http://www.sylixos.com). Hence, I made a fairly obvious decision last year, which is to port lwIP to RTEMS and have it run on my ARM development boards. It was very challenging at the beginning of my research due to lack of useful knowledge. I was struggling for quite a while before I finally made a breakthrough early this year.   

During my study, I soon realized that it should be possible to port lwIP to different real-time operating systems in a unified way, if I use the basic elements that they normally offer (for example, semaphore, mutex and timer). To prove my concept, I first ported lwIP to µC/OS due to its simplicity and availability of exiting code, then worked out some quite stable code. After that, I reused the same code and made a few necessary changes due to the different APIs these two operating systems provide. My code has been carefully verified on several different microcontroller platforms. The table below shows my achievements up to now.


| Microcontroller / Processor | CPU Core      | Status                               |
|-----------------------------|---------------|--------------------------------------|
| Samsung S3C2440             | ARM920T       | ports on µC/OS and RTEMS are done    |
| Atmel SAM9X35               | ARM926EJS     | ports on µC/OS and RTEMS are done    |
| Samsung S3C6410             | ARM1176JZF-S  | ports on µC/OS and RTEMS are ongoing |
| Atmel SAMA5D3               | ARM Cortex-A5 | ports on µC/OS and RTEMS are done    |
| TI/Sitara AM335x            | ARM Cortex-A8 | ports on µC/OS and RTEMS are ongoing |
| Xilinx ZYNQ                 | ARM Cortex-A9 | TBD                                  |
| STMicro STMF407             | ARM Cortex-M4 | TBD                                  |