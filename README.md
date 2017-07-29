I needed an easy and somewhat clean way to quickly log into
hundreds or even thousands of accounts from a qemu virtual machine
running android from a linux host.

Copying all the xml's to the device and using tools like sifam was
not an option because they can be thousands and they need to be
stored and updated from my pc.

I decided to throw together this bash script that accesses the
xml's through ssh.

# Requirements
* rooted device/emulator (including the su binary). I use qemu
  x86\_64 with android x86 7.1-rc1
* a shell (android's built in /system/bin/sh works)
* ssh, scp
* host system must have a ssh daemon running as well as a /tmp/
  directory

# Quick setup
* install termux from the google play store
* install requirements  
```
pkg install openssh
pkg install curl
pkg install python
pkg install nano # or your favorite editor
pip install percol
```
* generate a ssh key and copy it to your host pc otherwise you will
be asked for a password multiple times per call.
  
```
ssh-keygen
ssh-copy-id youruser@your.host.pc.ip
```
  
  I leave everything default in ssh-keygen. setting up a passphrase
  is safer, but it will bug you every time you run sif, so I leave
  it empty.
* install siftool  
```
curl https://github.com/Francesco149/siftool/master/sif > sif
chmod +x sif
curl https://github.com/Francesco149/siftool/master/sifrc.example \
  > ~/.sifrc
chmod +x ~/.sifrc
nano ~/.sifrc # or your favorite editor
```
  
  make sure to adjust values in .sifrc as explained by the comments
  inside.
  
  you will also want to disable root access notifications for
  applications you've already enabled in supersu/superuser or
  whatever you are using, otherwise you will get spammed much like
  with the ssh passphrase.

# Usage
```
./sif start ?
./sif kill
```

Use ```./sif help``` for a list of available commands.

You can make usage even simpler by putting sif somewhere within
```PATH```, or appending its path to ```PATH```.

For example, on busybox (shell used by termux) I simply put sif in
a ```~/sif``` directory and added
```export PATH="$PATH:$HOME/sif"``` to my ```~/.profile```, then
restarted termux and now I can just call sif without a path:
```sif start ?```.
