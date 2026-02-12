---
title: "Network Configuration"
description: "Required URLs, firewall rules, and network troubleshooting steps for IDmelon client applications."
lead: ""
date: 2022-02-16T18:14:02-08:00
lastmod: 2022-02-16T18:14:02-08:00
draft: false
images: []
menu:
  docs:
    parent: "troubleshoot"
weight: 800000
toc: true
---


Client applications need to be able to access the following addresses and IP addresses,
so you should configure your firewall settings to allow access to these addresses.

You need to grant access to the following addresses:

* https://authnapi.idmelon.com
* https://skm.idmelon.com
* https://panel.idmelon.com
* https://login.idmelon.com
* https://idmp.idmelon.com
* https://notify.idmelon.com
* https://notifyservice.idmelon.com

## Firewall Configuration Reminder

Please ensure that your firewall rules and security policies are correctly configured
and that connections to the above addresses (`FQDNs`), especially on port `443`, are allowed.

The IP addresses of these addresses are based on Amazon's load balancer, and they are dynamic, so they are subject to change.

## Network troubleshooting

If you encounter any issues accessing the services,
please execute the following commands for troubleshooting and examine the results.

```bash
curl  -v https://skm.idmelon.com/
```

```bash
wget  -v https://skm.idmelon.com/
```

In Linux OS:

```bash
dig +short skm.idmelon.com
```

```bash
sudo tcpdump -i any host skm.idmelon.com
```

In Windows OS:

```bash
nslookup -q=ns skm.idmelon.com
```

```bash
Test-NetConnection -ComputerName skm.idmelon.com -Port 443
```
