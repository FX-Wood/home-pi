# Home Pi

A repo to keep config and things for my home automation pi.

## Setup
Did most of the stuff in here:
https://raspberrytips.com/security-tips-raspberry-pi/
TL;DR is 
1. change default password
2. create new non-root user
3. disable password ssh
4. change ssh port
5. make sudoers use password
6. prevent root ssh
7. install rate limiter/brute-force blocker (fail2ban)
8. TODO set up firewall to limit external access

## Email (ssmtp)
### ssmtp is deprecated for debian buster (raspbian since 2019)
see here: https://github.com/maksimstojkovic/rpi-msmtp-setup
If using the gmail smtp server, you should be using 2FA for your gmail
account, which necessitates creating an "app password" for to use with
smtp auth. The main account password won't work.

It was a real challenge getting the mail configured. SMTP indicates you need
all three of: 
- Mail User Agent
- Mail Transfer Agent
- Mail Delivery agent (in my case, the google smtp server served this purpose)
in order to send mail. What I ended up using was msmtp and msmtp-mta and then it started working.

It wasn't clear which packages implement which of the three agents, these were
the ones I explored:

| package  | apparent use | helpful resource |
| ------------- | ------------- | ------------ |
| ssmtp | orphaned project that doesn't work on debian buster (raspbian is based on debian buster) | https://wiki.debian.org/sSMTP |
| **msmtp, msmtp-mta** | **msmtp implements some sending, but if you need a sendmail implementation, you also need msmtp-mta** | https://superuser.com/questions/1289550/msmtp-vs-msmtp-mta-package-which-one-to-choose |
| dsb-mailx | implements 'mail' and 'mailx' so for utils that use those it will work. Suggested in the readme for unattended-upgrade | |
| S-nail | seems like this could be a drop-in replacement for msmtp | https://wiki.archlinux.org/title/S-nail |
| mailutils |scary because a rando said on ubuntu it includes a beefy mailserver. Seems like overkill? | https://mailutils.org |

This is the post that broke down the barrier to understanding after some 
banging-of-head-on-wall: https://forums.raspberrypi.com/viewtopic.php?t=318994

## unattended-upgrade
this gist was helpful to get an overview of a working unattended-upgrades config: https://github.com/maksimstojkovic/rpi-msmtp-setup
These chunks of source code was also helpful to decipher which binaries
unattended-upgrade was using:
https://github.com/mvo5/unattended-upgrades/blob/8c62c05444ed505c2ca96a8092ee6be027033af3/unattended-upgrade#L94-L95
https://github.com/mvo5/unattended-upgrades/blob/8c62c05444ed505c2ca96a8092ee6be027033af3/unattended-upgrade#L1402-L1442

