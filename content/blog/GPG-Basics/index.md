---
title: "GPG Basics"
date: 2020-10-01T14:26:57-04:00
categories:
    - blog
tags:
    - linux
    - security
    - gpg
draft: false
---

GPG keys can be used for many things for example signing Git commits, sign email messages, encrypt email, sign files, encrypt files and so on.

## GPG Key Generation


### Create a Key

To create a key just use `gpg --full-gen-key` and answer the questions. The keysize by default is 3072 bits but should be changed to 4096 bits.

```sh
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: John Doe
Email address: john.doe@example.org
Comment: 
You selected this USER-ID:
    "John Doe <john.doe@example.org>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /home/arthurk/.gnupg/trustdb.gpg: trustdb created
gpg: key A5B2491173BD98A8 marked as ultimately trusted
gpg: directory '/home/arthurk/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/arthurk/.gnupg/openpgp-revocs.d/988958E85D62FAEEA102ED00A5B2491173BD98A8.rev'
public and secret key created and signed.

pub   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid                      John Doe <john.doe@example.org>
sub   rsa4096 2020-10-06 [E]
```

> When you are playing around and remove the gnupg folder by hand to start from scratch, then the following error can occur:
> ```sh
> gpg: agent_genkey failed: No such file or directory
> key generation failed: No such file or directory
> ```
>
> Solution: `gpgconf --kill gpg-agent`

After creation of the key `gpg --list-keys` shows the overview of the available public keys in the keyring.

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
pub   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
sub   rsa4096 2020-10-06 [E]

```

The command `gpg --list-secret-keys` shows the available secret keys in the keyring.

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
sec   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
ssb   rsa4096 2020-10-06 [E]
```

### Revoc Cert During Key Creation

During key creation a revocation certificate is created and stored somewhere. In the case of Ubuntu 20.04 the path where the certificate is stored in `/home/johndoe/.gnupg/openpgp-revocs.d/` and looks like the one below:

```sh
This is a revocation certificate for the OpenPGP key:

pub   rsa4096 2020-10-06 [S]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid          John Doe <john.doe@example.org>

A revocation certificate is a kind of "kill switch" to publicly
declare that a key shall not anymore be used.  It is not possible
to retract such a revocation certificate once it has been published.

Use it to revoke this key in case of a compromise or loss of
the secret key.  However, if the secret key is still accessible,
it is better to generate a new revocation certificate and give
a reason for the revocation.  For details see the description of
of the gpg command "--generate-revocation" in the GnuPG manual.

To avoid an accidental use of this file, a colon has been inserted
before the 5 dashes below.  Remove this colon with a text editor
before importing and publishing this revocation certificate.

:-----BEGIN PGP PUBLIC KEY BLOCK-----
Comment: This is a revocation certificate

iQI2BCABCgAgFiEEmIlY6F1i+u6hAu0ApbJJEXO9mKgFAl98wfkCHQAACgkQpbJJ
EXO9mKhdGQ/+M9wniqgw2EAih8Tc3Fphdyx0jtjTunpYrU0lNGAx3cur67vkFtV8
SKn7dtwKTQ2ngDqRyzaItqMVeOIIHHHpBwOIRMyST4Cr7X3SJWEFPBpyHSksO+PL
fmtYblOMuRvsXN+rscA9o7Npbu7Bg5p2vPAjEYOycH6Ukg2Ui7sj6jXbUBkPj/5s
[...]
om18odemFd9tO3oEoc//8fjkKWbq3dOSfkrYWdBgB2eYx92z/bJ7DMnTAsDBYqxc
KtJjdgDT8ObLK5DIGUNycxkcy30QyipyNb3YMAMZ5B0febZa2JnDrEDkcA0Snf53
mjmhSFqd7brMY5BR4MS3NQdxpC8qHyjbzxVIZq3pihWdM8IvrTSFHVk=
=JIuh
-----END PGP PUBLIC KEY BLOCK-----
```

### Add Subkey

[Subkeys](https://wiki.debian.org/Subkeys) are like normal keypairs, but they are bound to master keypair. This master keypair was created before.

To create a signing subkey use `gpg --edit-key 988958E85D62FAEEA102ED00A5B2491173BD98A8` and use the `addkey` command.

```sh
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
[ultimate] (1). John Doe <john.doe@example.org>

gpg> addkey
Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
  (14) Existing key from card
Your selection? 4
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y
Really create? (y/N) y
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  expires: never       usage: S   
[ultimate] (1). John Doe <john.doe@example.org>

gpg> quit
Save changes? (y/N) y

```

Now look at the available keys with `gpg --list-keys`.

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
pub   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
sub   rsa4096 2020-10-06 [E]
sub   rsa4096 2020-10-06 [S]
```

And a quick look at the available private keys with `gpg --list-secret-keys`.

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
sec   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
ssb   rsa4096 2020-10-06 [E]
ssb   rsa4096 2020-10-06 [S]
```

But why subkeys? The master key pair is very important since this is the basic proof of your identity and the start for building your reputation. If you lose or revoke the master key pair then you have to start building your reputation again from scratch. Subkeys can be revoked independently from the master keys, they can also be stored separately from the master keys. 

So create a master key pair and keep it safe. The master key should be only used for a few tasks:

- Signing someone else's key
- Revoke an existing signature
- Add a new UID or mark an existing UID as primary
- Create a new subkey
- Revoke an existing UID
- Revoke an existing subkey
- Change the preferences on a UID
- Change the expiration date on your master key or any of its subkey
- Revoke or generate a revocation certificate for the complete key. 


## Key Security and Laptop Keypair

### Export Public and Private Keys

To export the public keys use `gpg --export -a "John Doe" > john_doe_public.key`.

```sh
-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBF93RVkBEACxVXPbcReDoWYyZBIqBusIX0EUHo8vxYSN8BmO3DcESpOfxXc0
lqlNT0wpwJxdXCnFUim8YpP/+I1fCO9pUArVtsIy64KEFZlSwwi8f9rpUBVLvrsQ
rwpO+iIghxkEoJ1W6coJedJ6NehEC7+4nUC+eKlbJDo5TBYk528QSkttXRecVdR2
soVOPCT0IufLyFKpW7qFZJEUuZglBTEOs45R0nAac6mJDI98Xm+SztQeFYDyk0ay
[...]
qFnvcgTyxGNeVdULt5oqAGE0rBubVay0OxAyw7rZaoI3VBhIk2BPfn2W4hDuNpsu
UoUYq6rX7rptSGJwuefeUoFM+Plid1hpBE/yv6a1f7/LoZbc1OpatL7snC+Nsnbu
JvXnB9z6mNSuJ0vJ/wP6bR75/XDarY58s302NXbp6kZnfQldySMPo1v/XiE+cSvn
vJUjGOTM9tVOc6h/LlBZtEagquB49NmHNaAPZsKzehkwsHp4rmAjKJ2+Pg==
=YE1n
-----END PGP PUBLIC KEY BLOCK-----
```

To create the so called **laptop keypair** export the private keys by using `gpg --export-secret-key -a "John Doe" > john_doe_private.key`.

```sh
-----BEGIN PGP PRIVATE KEY BLOCK-----

lQdGBF93RVkBEACxVXPbcReDoWYyZBIqBusIX0EUHo8vxYSN8BmO3DcESpOfxXc0
lqlNT0wpwJxdXCnFUim8YpP/+I1fCO9pUArVtsIy64KEFZlSwwi8f9rpUBVLvrsQ
rwpO+iIghxkEoJ1W6coJedJ6NehEC7+4nUC+eKlbJDo5TBYk528QSkttXRecVdR2
soVOPCT0IufLyFKpW7qFZJEUuZglBTEOs45R0nAac6mJDI98Xm+SztQeFYDyk0ay
[...]
KgBhNKwbm1WstDsQMsO62WqCN1QYSJNgT359luIQ7jabLlKFGKuq1+66bUhicLnn
3lKBTPj5YndYaQRP8r+mtX+/y6GW3NTqWrS+7JwvjbJ27ib15wfc+pjUridLyf8D
+m0e+f1w2q2OfLN9NjV26epGZ30JXckjD6Nb/14hPnEr57yVIxjkzPbVTnOofy5Q
WbRGoKrgePTZhzWgD2bCs3oZMLB6eK5gIyidvj4=
=0Rkl
-----END PGP PRIVATE KEY BLOCK-----
```

> Store these keys in a very, very safe place!

### Manually Create Revoc Cert

If you want to create a revocation key at some point the following command will create one:

`gpg --output 988958E85D62FAEEA102ED00A5B2491173BD98A8_revoc_cert.cer --gen-revoke 988958E85D62FAEEA102ED00A5B2491173BD98A8`

After a couple of questions you get the following certificate:

```sh
-----BEGIN PGP PUBLIC KEY BLOCK-----
Comment: This is a revocation certificate

iQI2BCABCgAgFiEEAy4iE0YlgksT/yUD2IVtTokj4oEFAl93VK0CHQAACgkQ2IVt
Tokj4oHWUA/+I86euJh8Kl4fJepWlpeMlJijJ1d9gsjYYGk01HWkXn+Fq+VIOWIU
SLI+J0v8BfpcmtsHTxU1ZdcQ6I6yHHkgBXCG6D6tpJyhB5i0Y7v4YYYg0uJ6weFk
yDLGSHL32rnR26TfNzpov2wgC7+FCRMJm+2FPBBWwRB83viQh/dGeFzD66N/be3k
[...]
n2C7Zy+h0jGWIrSut2GRLhBGTSXcLORjWbnkDwlbLEJNeBPkmkjvo1D1dQPrwXwC
4R+YK+I7H4rHaZG+uJvdkAV6eFNMCFGtVRF1PK6TOdfa9FEY8oNXY+O26eUjzmgx
Z4LPHrHeP+wgS3AxNbGB2m4jFUjFuJ03Gj9k3yUpoZ8dA9x6xCwWM4k4eOSd6JnI
Fh1TX4iZTZRGMgUM+c3K85GwT+WeYmQUmct639fKB+FZJhNlK1vxFpA=
=wdkR
-----END PGP PUBLIC KEY BLOCK-----
```

Add a colon manually at the beginning to the certificate so that this will be invalid. **If you accidentally import this certificate then your key will be immediately revoked.**

```sh
:-----BEGIN PGP PUBLIC KEY BLOCK-----
Comment: This is a revocation certificate

[...]
-----END PGP PUBLIC KEY BLOCK-----
```

If you have to use the revoc certificate remove the colon and import the certificate to invalidate the whole key.

> Usually I create two revocation certificates, 
> - one certificate without naming a specific reason for the revocation procedure and 
> - another certificate for the worst case scenario - a stolen or compromised master keypair. 

> - Store this revocation certificate in a very, very safe place! 
> - Delete all other revocation certificates.

### Create Laptop Keypair

Since we have exported and securely backed up the master keypair and the revocation certificate we can create the so called laptop keypair.

When we use `gpg --edit-key 988958E85D62FAEEA102ED00A5B2491173BD98A8` then we see the id's of each key we have.

```sh
Secret key is available.

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  expires: never       usage: S   
[ultimate] (1). John Doe <john.doe@example.org>
```

Now we can export the subkey with

`gpg --armor --export-secret-subkeys CEDA609D8DAB2B40 > CEDA609D8DAB2B40_private_subkey.key`

And now we can delete the master keypair

`gpg --delete-secret-keys A5B2491173BD98A8`

> Be careful, just delete the master key! Do not delete the subkeys. Read what's on your display.

To use the subkey on a laptop or somewhere else we have to import the subkey

`gpg --import CEDA609D8DAB2B40_private_subkey.key`

After importing the subkey does not have a any trust. If you trust the imported key you do this with `gpg --edit-key 988958E85D62FAEEA102ED00A5B2491173BD98A8` and the command `trust`.

```sh
gpg> trust
pub  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: unknown       validity: unknown
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  expires: never       usage: S   
[ unknown] (1). John Doe <john.doe@example.org>

Please decide how far you trust this user to correctly verify other users' keys
(by looking at passports, checking fingerprints from different sources, etc.)

  1 = I don't know or won't say
  2 = I do NOT trust
  3 = I trust marginally
  4 = I trust fully
  5 = I trust ultimately
  m = back to the main menu

Your decision? 5
Do you really want to set this key to ultimate trust? (y/N) y
```

Verify with `gpg --list-secret-keys`

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
sec#  rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
ssb   rsa4096 2020-10-06 [E]
ssb   rsa4096 2020-10-06 [S]
```

The `sec#` means that the signing master key is not this keyring. 

## Revocation Process

### Revoke Subkey

To revoke subkey is straight forward. Import the master keypair `gpg --import john_doe_public.key john_doe_private.key` and revoke the subkey with `gpg --edit-key 988958E85D62FAEEA102ED00A5B2491173BD98A8`. Select the correct key with `key <keynumber>` and with the command `revkey` the key will be revoked. 

```sh
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  expires: never       usage: S   
[ultimate] (1). John Doe <john.doe@example.org>

gpg> key 2

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
ssb* rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  expires: never       usage: S   
[ultimate] (1). John Doe <john.doe@example.org>

gpg> revkey
Do you really want to revoke this subkey? (y/N) y
Please select the reason for the revocation:
  0 = No reason specified
  1 = Key has been compromised
  2 = Key is superseded
  3 = Key is no longer used
  Q = Cancel
Your decision? 0
Enter an optional description; end it with an empty line:
> 
Reason for revocation: No reason specified
(No description given)
Is this okay? (y/N) y

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
The following key was revoked on 2020-10-06 by RSA key A5B2491173BD98A8 John Doe <john.doe@example.org>
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  revoked: 2020-10-06  usage: S   
[ultimate] (1). John Doe <john.doe@example.org>

gpg> quit
Save changes? (y/N) y
```

`gpg --list-keys`

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
pub   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
sub   rsa4096 2020-10-06 [E]
```

`gpg --list-secret-keys`

```sh
/home/johndoe/.gnupg/pubring.kbx
sec   rsa4096 2020-10-06 [SC]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ultimate] John Doe <john.doe@example.org>
ssb   rsa4096 2020-10-06 [E]
```

`gpg --edit-key 988958E85D62FAEEA102ED00A5B2491173BD98A8`

```sh
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

sec  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  expires: never       usage: SC  
     trust: ultimate      validity: ultimate
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  expires: never       usage: E   
The following key was revoked on 2020-10-06 by RSA key A5B2491173BD98A8 John Doe <john.doe@example.org>
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  revoked: 2020-10-06  usage: S   
[ultimate] (1). John Doe <john.doe@example.org>

gpg> quit
```

### Revoke Whole Key

If the master key still exists, import it and revoke the whole key, like a subkey. Otherwise, just import one of the revocation certificates if the master key is lost:

`gpg --import 988958E85D62FAEEA102ED00A5B2491173BD98A8_revoc_cert.cer`

And with `gpg --edit-key 988958E85D62FAEEA102ED00A5B2491173BD98A8` we see the whole history of the key and its revocation status.

```sh
gpg (GnuPG) 2.2.19; Copyright (C) 2019 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret subkeys are available.

The following key was revoked on 2020-10-06 by RSA key A5B2491173BD98A8 John Doe <john.doe@example.org>
pub  rsa4096/A5B2491173BD98A8
     created: 2020-10-06  revoked: 2020-10-06  usage: SC  
     trust: unknown       validity: revoked
The following key was revoked on 2020-10-06 by RSA key A5B2491173BD98A8 John Doe <john.doe@example.org>
ssb  rsa4096/8A5F5241B1B26625
     created: 2020-10-06  revoked: 2020-10-06  usage: E   
The following key was revoked on 2020-10-06 by RSA key A5B2491173BD98A8 John Doe <john.doe@example.org>
ssb  rsa4096/CEDA609D8DAB2B40
     created: 2020-10-06  revoked: 2020-10-06  usage: S   
[ revoked] (1). John Doe <john.doe@example.org>

gpg> quit
```

`gpg --list-secret-keys`

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
sec#  rsa4096 2020-10-06 [SC] [revoked: 2020-10-06]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ revoked] John Doe <john.doe@example.org>
```

`gpg --list-keys`

```sh
/home/johndoe/.gnupg/pubring.kbx
--------------------------------
pub   rsa4096 2020-10-06 [SC] [revoked: 2020-10-06]
      988958E85D62FAEEA102ED00A5B2491173BD98A8
uid           [ revoked] John Doe <john.doe@example.org>
``` 

## Keyserver

Until now everything happened locally. To inform the outside world we have to send the key to a keyserver:

`gpg --keyserver https://keys.gnupg.net --send-key 988958E85D62FAEEA102[...]`