Adjusted version of "pam-demos" from Jürgen Schmitt (c't magazin, please see original infos below) for 
* **Arch Linux** (with systemd-boot)
* with a **Nitrokey FIDO 2**
* replacing **system-auth** instead of common-auth, which is used for **sudo** (with a slightly changed syntax).

For more information on the **setup** steps for the **Nitrokey FIDO 2** please also read https://docs.nitrokey.com/fido2/linux/desktop-login


### For creating the u2f_keys file
Example commands for creating the u2f_keys file

```sh
# Create an empty folder
mkdir -p ~/.config/Nitrokey

# Create
pamu2fcfg > ~/.config/Nitrokey/u2f_keys
# As an alternative for using pin and afterwards presence (touch the device):
#  pamu2fcfg -N > ~/.config/Nitrokey/u2f_keys

# Move the folder  to the suggested place under /etc (you could also keep it in your home folder and adjust the path though...)
sudo mv ~/.config/Nitrokey /etc
```
*Hint:* The generated file determines which methods for authentication with the UTF2/FIDO2 token are used, e.g. PIN, presence (touch the device) and the file can be manually adjusted as the method is mentioned at the end of the line.


### For installation 
*Hint:* Currently only the following file was adjusted:
```
inc-u2f-pw		    U2F/FIDO2 Token / Password
```

1. Copy the file to /etc/pam.d:
```
cd pam-demos/
sudo cp inc-u2f-pw /etc/pam.d/
```
2. And replace for example in /etc/pam.d/sudo the line with "system-auth", e.g. to:
```
#%PAM-1.0
#auth           include         system-auth
auth            include         inc-u2f-pw
#account                include         system-auth
account         include         inc-u2f-pw      
#session                include         system-auth
session         include         inc-u2f-pw
```

-------------------------------------------------------------------------------------

"Hallo Linux" - PAM-Demos
--------------------------
(c) 2019 Jürgen Schmidt aka ju

All the details are in "Entspannt entsperrt,
Linux-Authentifizierung mit mehr Komfort", 
c't 10/2019 Seite 132ff

This are PAM configuration files for demonstration
purposes. They always allow authentication with your
Linux system password as a last resort.

Attention: For Authentication with face recognition,
U2F token and PIN you have to install and configure the
respective PAM libraries first. The details are
explained in the articles.


```
inc-pin+howdy-pw	PIN+Face / Password
inc-pin+u2f-pw		PIN+U2F-Token / Password
inc-u2f+howdy-pw	U2F+Face / Password
inc-u2f-pw		U2F-Token / Passwort
```

For installation copy the files to /etc/pam.d:

```
cd pam-demos/
sudo cp inc-* /etc/pam.d/
```

and replace for example in /etc/pam.d/sudo the line with "common-auth":

```
-@include common-auth

+@include inc-u2f-pw
+# @include common-auth
```
