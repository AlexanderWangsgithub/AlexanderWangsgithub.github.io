---
layout: post
title: 使用Gradle发布中央Maven仓库
categories: [blog]
tags: [tags]
description: 
---

使用Gradle发布中央Maven仓库

[TOC]

## 注册&创建issue

[创建issue地址](https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134)

https://issues.sonatype.org/browse/OSSRH-24421



## gpg密钥对

```
➜  arena git:(master) gpg --gen-key
gpg (GnuPG) 1.4.20; Copyright (C) 2015 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: directory `/Users/wanggang/.gnupg' created
gpg: new configuration file `/Users/wanggang/.gnupg/gpg.conf' created
gpg: WARNING: options in `/Users/wanggang/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/Users/wanggang/.gnupg/secring.gpg' created
gpg: keyring `/Users/wanggang/.gnupg/pubring.gpg' created
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? q
Invalid selection.
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 
Requested keysize is 2048 bits   
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 
Key does not expire at all
Is this correct? (y/N) y
                        
You need a user ID to identify your key; the software constructs the user ID
from the Real Name, Comment and Email Address in this form:
    "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"

Real name: Alexander Wang
Email address: wg1033755123@gmail.com
Comment: maven                       
You selected this USER-ID:
    "Alexander Wang (maven) <wg1033755123@gmail.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? o
You need a Passphrase to protect your secret key.    

We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
+++++
....+++++
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
....+++++
.+++++
gpg: /Users/wanggang/.gnupg/trustdb.gpg: trustdb created
gpg: key 552C70B8 marked as ultimately trusted
public and secret key created and signed.

gpg: checking the trustdb
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
pub   2048R/552C70B8 2016-08-19
      Key fingerprint = 7B63 6F6D F1FB CA58 5120  A0C7 6480 4E1E 552C 70B8
uid                  Alexander Wang (maven) <wg1033755123@gmail.com>
sub   2048R/993347CE 2016-08-19

```

查看key

```
➜  arena git:(master) gpg --list-keys
/Users/wanggang/.gnupg/pubring.gpg
----------------------------------
pub   2048R/552C70B8 2016-08-19
uid                  Alexander Wang (maven) <wg1033755123@gmail.com>
sub   2048R/993347CE 2016-08-19
```

发布Key

```
➜  arena git:(master) gpg --keyserver hkp://pool.sks-keyservers.net --send-keys 552C70B8 
gpg: sending key 552C70B8 to hkp server pool.sks-keyservers.net
➜  arena git:(master) gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 552C70B8      
gpg: requesting key 552C70B8 from hkp server pool.sks-keyservers.net
gpgkeys: key 552C70B8 not found on keyserver
gpg: no valid OpenPGP data found.
gpg: Total number processed: 0
gpg: keyserver communications error: key not found
gpg: keyserver communications error: bad public key
gpg: keyserver receive failed: bad public key
```

原因：需要下载PGP软件[下载地址](https://releases.gpgtools.org/GPG_Suite-2016.08_v2.dmg)

