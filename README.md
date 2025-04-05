# Overview

This repository contains `sf-ip-allocs` extension to [Server Farmer](https://github.com/serverfarmer/). This particular extension provides a single file, which
can be used independently of Server Farmer, with any other solution as well.

File [`allocs`](https://github.com/serverfarmer/sf-ip-allocs/blob/master/allocs) has a generic shell syntax, and can be directly included into most firewall configuration scripts.
It defines several variables, where each variable contains a list of IP addresses (or ranges) for particular Internet Service Providers, Cloud providers, and some other services.

Example variable:

```
TPNET="79.184.0.0/13 80.48.0.0/13 83.0.0.0/11"
```

If you don't have any firewalling solution, please take a look at [Server Farmer scriptable firewall](https://github.com/serverfarmer/sf-ip-fw) (for Linux).


# Long Term Support declaration

This public repository is actively maintained since Sep 2016. Previously, some of these variables were maintained since 2009, in private repository, as part of commercial solution.

Right now, our plan regarding this repository is to maintain it as long as possible. Also, **it will stay free** and, as far as possible, without any changes breaking compatibility.


# Which IP ranges are listed, and which not?

1. Most Internet Service Providers have 2 types of [IP allocations](http://www-public.int-evry.fr/~maigron/RIR_Stats/RIPE_Allocations/Allocs/PL.html):

   - for DHCP/NAT/etc. - from which outgoing traffic is directed to other networks (for you, it's incoming traffic)
   - for hosting services (web pages, mail servers etc.) and other internal uses

   Variables in `allocs` file are related only to the first type.

2. There are some global ISP brands in Poland (eg. T-Mobile, Orange). Variables like `ORANGE` are related only to their polish branches and polish networks.


# Variables related to your incoming traffic

### Poland - Cable TV providers

- `INEA` refers to the biggest ISP and CATV provider in western Poland, [INEA S.A.](https://www.inea.pl/)
- `ECHOSTAR`, `MAVERICK`, `EASTWEST`, `SYSTEMIAPL` - local ISP/CATV companies located in Poznań (western Poland)
- `MULTIMEDIA` - global polish CATV [Multimedia Polska](https://www.multimedia.pl/), now part of bigger CATV [Vectra](https://www.vectra.pl/)
- `VECTRA` - Vectra itself
- `UPC` - another global polish CATV [UPC Polska](https://www.upc.pl/)

### Poland - GSM operators

In poland, there are 4 major GSM operators with their own networks - all of them are listed below. All the rest are [MVNOs](https://en.wikipedia.org/wiki/Mobile_virtual_network_operator),
utilizing IP addresses provided by their operator (except for Virgin Mobile Polska, which is not listed here).

- `PLAY` - Play Mobile, [P4 Sp. z. o.o.](https://www.play.pl/)
- `PLUS` - Plus GSM [Polkomtel Sp. z o.o.](https://www.plus.pl/)
- `TMOBILE` - [T-Mobile Polska S.A.](https://www.t-mobile.pl/) (previously Era GSM)
- `ORANGE` - [Orange Polska S.A.](https://www.orange.pl/) (only GSM part of their network, see `TPNET` variable below)

### Poland - other ISPs

- `TPNET` - the biggest polish ADSL/FTTH network, previously polish national ISP (Telekomunikacja Polska), now part of Orange, but still branded as Neostrada (this variable is only related to ASDL/FTTH part of their network)
- `NETIA` - [Netia S.A.](https://www.netia.pl/), second biggest global ISP in Poland, their address ranges mix many types of networks (ADSL, broadband, other) - note that many Netia customers have non-Netia IP addresses:
   - Netia Mobile - uses IP ranges from `PLUS`, previously `PLAY`
   - corporate customers (Netia has many) often use their own IP allocations

### Finland

- `SONERAFI` - [Telia](https://www.telia.fi/), major ISP in Helsinki and Turku
- `ELISAFI` - [Elisa](https://elisa.fi/), GSM operator deployed in finnish trains

### global cloud services

- `AMAZONAWS` - [Amazon Web Services](https://aws.amazon.com/) (only IP ranges, from which you can expect incoming traffic, and sometimes merged into bigger subnets - [the full list](https://ip-ranges.amazonaws.com/ip-ranges.json) includes over 6500 different subnets!)
- `GCLOUD` - [Google Cloud Platform](https://cloud.google.com/) (only major IP ranges, see [this script](https://gist.github.com/n0531m/f3714f6ad6ef738a3b0a) for the full list)

### CI/CD tools

- `BITBUCKET_PIPELINES` - Bitbucket Pipelines build environments, see `BITBUCKET` variable below

### monitoring services

- `UPTIMEROBOT` - [UptimeRobot](https://uptimerobot.com/) cheap (at least before 2022) website monitoring, that we use since 2016

### special variables

- `NONROUTABLE` - expands to all local addresses (used only within your LAN)
- `DOCKERONLY` - default [Docker](https://www.docker.com/) subnet (see [example](https://github.com/serverfarmer/sf-ip-fw#example-per-host-profile-with-docker-support) how to use it)


# Variables related to your outgoing traffic

### repositories with system software packages for important Linux distributions

- `DEBIAN` - [Debian](https://www.debian.org/) - all global addresses + polish mirror
- `CANONICAL` - [Ubuntu](https://ubuntu.com/)
- `RASPBIAN` - Raspbian, currently [Raspberry Pi OS](https://www.raspberrypi.com/software/) - Debian clone for Raspberry Pi hardware
- `DEVUAN` - [Devuan](https://www.devuan.org/) - Debian for without systemd
- `PROXMOX` - [Proxmox VE](https://www.proxmox.com/en/proxmox-ve) - commercial hypervisor based on Debian

### major Git repositories

- `GITHUB` - major Git repository, address ranges related both to web frontend and ssh endpoints
- `BITBUCKET` - second most important Git repository, owned by Atlassian (creators of JIRA) - see [here](https://support.atlassian.com/bitbucket-cloud/docs/what-are-the-bitbucket-cloud-ip-addresses-i-should-use-to-configure-my-corporate-firewall/) for current list; Pipelines addresses are listed in separate `BITBUCKET_PIPELINES` variable

### monitoring services

- `NEWRELIC_COLLECTOR` - [New Relic](https://newrelic.com/) - addresses required for reporting data from connected servers

### others

- `SMSAPI` - [smsapi.pl](https://www.smsapi.pl/) polish commercial SMS gateway
- `RARLAB` - [RAR/WinRAR](https://www.rarlab.com/) archiver download site


# How to contribute

We are open to add new variables, related to important ISPs or services, and to update existing ones (we have notifications about changes where possible, but still we can miss something).
Just [create a new issue](https://github.com/serverfarmer/sf-ip-allocs/issues) for us.


# License

|                      |                                          |
|:---------------------|:-----------------------------------------|
| **Author:**          | Tomasz Klim (<opensource@tomaszklim.pl>) |
| **Copyright:**       | Copyright 2009-2025 Tomasz Klim          |
| **License:**         | MIT                                      |

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
