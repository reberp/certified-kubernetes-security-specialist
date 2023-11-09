#### Check status of apparmor:
```sh
systemctl status apparmor
```
#### Sample Script:
```sh
mkdir /root/apparmor
cd /root/apparmor
```
```sh
nano myscript.sh
```
```sh
#!/bin/bash
touch /tmp/file.txt
echo "New File created"

rm -f /tmp/file.txt
echo "New file removed"
```
```sh
chmod +x myscript.sh
```
#### Install Apparmor Utils:
```sh
apt install apparmor-utils -y
```
#### Generate a new profile:
```sh
aa-genprof ./myscript.sh
# Other tab run
S #to scan
I #to inherit
```
```sh
./myscript.sh (from new tab)
```
#### Verify the new profile:
```sh
cat /etc/apparmor.d/root.tt.script.sh
"""
# Last Modified: Thu Nov  9 13:46:00 2023
abi <abi/3.0>,

include <tunables/global>

/root/apparmor/myscript.sh {
  include <abstractions/base>
  include <abstractions/bash>

  /root/apparmor/myscript.sh r,
  /usr/bin/bash ix,
  /usr/bin/rm mrix,
  /usr/bin/touch mrix,

}
"""
aa-status
"""
Couple things in output:
2 profiles are in complain mode.
   /root/apparmor/myscript.sh//null-/usr/bin/rm
   /root/apparmor/myscript.sh//null-/usr/bin/touch
"""
#Try to run again and won't do curl
```
#### Disable a profile:
```sh
ln -s /etc/apparmor.d/root.apparmor.myscript.sh /etc/apparmor.d/disable/
apparmor_parser -R /etc/apparmor.d/root.apparmor.myscript.sh
```
