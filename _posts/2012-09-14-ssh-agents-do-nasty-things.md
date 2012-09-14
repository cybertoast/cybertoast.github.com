---
layout: post
title: "SSH Agents do nasty things"
description: ""
category: 
tags: []
---
{% include JB/setup %}

So trying to `git fetch` from my repo gave me a nasty:

    ERROR -- : READ permission denied

but I knew for a fact that my key was correct and had permissions. Debugging (and solution) went as follows:

    ssh -vv git@git.assembla.com

The output was:

    ...
    ...
    debug1: kex: server->client aes128-ctr hmac-md5 none
    debug2: mac_setup: found hmac-md5
    debug1: kex: client->server aes128-ctr hmac-md5 none
    debug1: SSH2_MSG_KEX_DH_GEX_REQUEST(1024<1024<8192) sent
    debug1: expecting SSH2_MSG_KEX_DH_GEX_GROUP
    debug2: dh_gen_key: priv key bits set: 134/256
    debug2: bits set: 524/1024
    debug1: SSH2_MSG_KEX_DH_GEX_INIT sent
    debug1: expecting SSH2_MSG_KEX_DH_GEX_REPLY
    debug1: Host 'git.assembla.com' is known and matches the RSA host key.
    debug1: Found key in /Users/sundar/.ssh/known_hosts:1
    debug2: bits set: 508/1024
    debug1: ssh_rsa_verify: signature correct
    debug2: kex_derive_keys
    debug2: set_newkeys: mode 1
    debug1: SSH2_MSG_NEWKEYS sent
    debug1: expecting SSH2_MSG_NEWKEYS
    debug2: set_newkeys: mode 0
    debug1: SSH2_MSG_NEWKEYS received
    debug1: SSH2_MSG_SERVICE_REQUEST sent
    debug2: service_accept: ssh-userauth
    debug1: SSH2_MSG_SERVICE_ACCEPT received
    debug2: key: /Users/sundar/.ssh/ChangeByUs-TEST.pem (0x100125e40)
    debug2: key: /Users/sundar/.ssh/deploy.readynas.id_rsa (0x100126b00)
    debug2: key: /Users/sundar/.ssh/id_rsa (0x100126d70)
    debug2: key: /Users/sundar/.ssh/localprojects/id_rsa (0x1001257d0)
    debug1: Authentications that can continue: publickey,keyboard-interactive
    debug1: Next authentication method: publickey
    debug1: Offering public key: /Users/sundar/.ssh/ChangeByUs-TEST.pem
    debug2: we sent a publickey packet, wait for reply
    debug1: Authentications that can continue: publickey,keyboard-interactive
    debug1: Offering public key: /Users/sundar/.ssh/deploy.readynas.id_rsa
    debug2: we sent a publickey packet, wait for reply
    debug1: Server accepts key: pkalg ssh-rsa blen 277
    debug2: input_userauth_pk_ok: fp 05:f7:fe:6c:83:cb:d0:cc:a4:58:1b:96:17:ac:ff:30
    debug1: Authentication succeeded (publickey).

    ...

Wait WTF?! What's ChangeByUs-TEST.pem doing in there?!

    ssh-add -l
    2048 d8:6d:c7:5e:d7:d8:d4:07:c9:ea:b6:1d:d1:d2:4b:a6 /Users/sundar/.ssh/ChangeByUs-TEST.pem (RSA)
    2048 05:f7:fe:6c:83:cb:d0:cc:a4:58:1b:96:17:ac:ff:30 /Users/sundar/.ssh/deploy.readynas.id_rsa (RSA)
    2048 83:74:81:d9:a7:bb:df:12:fe:6d:e3:d8:7d:31:b1:a6 /Users/sundar/.ssh/id_rsa (RSA)

Well that's useless ...

    ssh-add -D
    All identities removed.

Ok, now let's try that again
    
    git fetch origin

Ok, all good.
