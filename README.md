# Yubico Passwordless Ubuntu Login
Yubico Passwordless Ubuntu Login
This is an awesome installation.  It doesn't require the key but make it optional but a `sufficient` attempt and if the key isn't recognized then the password is required.  It even gives the option during sudo access.

Please keep in mind that I did not come up with all of the information from this but did copy a large portion of it from [AskUbuntu](https://askubuntu.com/questions/1167691/passwordless-login-with-yubikey-5-nfc?newreg=d3833870cc924fedb49ce95d064f3a09)

### Install needed software to interface with the Yubico Key
```shell
sudo apt install libpam-u2f
```

### Create a mappings file
Run this for each user.  Update `USERNAME` with the username of the appropriate user
```shell
echo Press the button
sudo pamu2fcfg -u USERNAME >> /etc/u2f_mappings
```

### This finalizes the installation
An error message may appear during this portion.  But that is normal under certain circumstances.

```shell
sudo su
cd /etc/pam.d
echo 'auth sufficient pam_u2f.so authfile=/etc/u2f_mappings cue' > common-u2f
for f in gdm-password lightdm sudo login; do
mv $f $f~
awk '/@include common-auth/ {print "@include common-u2f"}; {print}' $f~ > $f
done
exit
```
