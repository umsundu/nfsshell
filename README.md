# nfsshell
Useful tool to explore and navigate NFS shares. 

## All credit to:
* phillips321.co.uk

## References:
* https://www.phillips321.co.uk/2015/09/15/nfsshell-on-kali-linux-2-0/
* https://askubuntu.com/questions/1168787/libreadline-so-6-issue-in-ubuntu-18-04

# Getting Started:

## Download nfsshell_64:
```
root@kali:/opt# wget https://www.phillips321.co.uk/downloads/nfsshell_64

---- https://www.phillips321.co.uk/downloads/nfsshell_64
Resolving www.phillips321.co.uk (www.phillips321.co.uk)... 83.151.205.157
Connecting to www.phillips321.co.uk (www.phillips321.co.uk)|83.151.205.157|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 161704 (158K)
Saving to: ‘nfsshell_64’
nfsshell_64 100%[==============================================>] 157.91K --.-KB/s in 0.05s
 (3.11 MB/s) - ‘nfsshell_64’ saved [161704/161704]
```

## Set execution permissions:
```
root@kali:/opt# cd nfsshell/

root@kali:/opt/nfsshell# ls -la
total 168
drwxr-xr-x 2 root root 4096 Mar 2 13:59 .
drwxr-xr-x 10 root root 4096 Mar 2 13:59 ..
-rw-r--r-- 1 root root 161704 Dec 16 2018 nfsshell_64

root@kali:/opt/nfsshell# chmod +x nfsshell_64
```

## Install the required libraries:
`sudo apt-get install libreadline-dev`

Once the binary has been downloaded and you try to run it, you may run into the following error:
```
root@kali:/opt/nfsshell# ./nfsshell_64

./nfsshell_64: error while loading shared libraries: libreadline.so.6: cannot open shared object file: No such file or directory
```

## Fixing  libreadline.so.6 and libhistory.so.6

If the above error occurs you will need to create a symlink as the nsfshell binary is looking for version 6 of two files called libreadline.so.6 and libhistory.so.6. These are old versions and not included in the latest install of the libreadline-dev pachage. The libreadline-dev will have installed the latest versions.

Navigate to the /lib/x86_64-linux-gnu/ directory and list the two file names to see what version was installed as can be seen below

List libreadline.so libraries 
```
root@kali:/lib/x86_64-linux-gnu# ls -l libreadline.so.*

lrwxrwxrwx 1 root root 18 Dec 8 06:58 libreadline.so.8 -> libreadline.so.8.1
-rw-r--r-- 1 root root 346312 Dec 8 06:58 libreadline.so.8.1
```

List libhistory.so libraries 
```
root@kali:/lib/x86_64-linux-gnu# ls -l libhistory.so*

lrwxrwxrwx 1 root root 17 Dec 8 06:58 libhistory.so.8 -> libhistory.so.8.1
-rw-r--r-- 1 root root 51672 Dec 8 06:58 libhistory.so.8.1
```

As the above output indicates version 8 for both libraries (libreadline.so.8 ) (libhistory.so.8) has been installed. All we need to do now is create a symlink from version 8 to version 6 as can be seen below.

## Create symlink for libreadline.so.8 and libhistory.so.8 to libreadline.so.6 and libhistory.so.6
```
root@kali:/lib/x86_64-linux-gnu# sudo ln -s libreadline.so.8 libreadline.so.6
root@kali:/lib/x86_64-linux-gnu# sudo ln -s libhistory.so.8 libhistory.so.6
```

## Rerun NFSShell 
Now that the symlinks have been created nfsshell should run without issue.
```
root@kali:/opt/nfsshell# ./nfsshell_64
nfs>
```

