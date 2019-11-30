# Hello World!

![Image](https://pbs.twimg.com/profile_images/1164590169288335360/iFtFEJMU_200x200.jpg)

It's Alex here.

Reach me out on:

<a href="https://twitter.com/imsysl"> Twitter </a> | <a href="https://www.instagram.com/alexsoysal/"> Instagram </a> | <a href="https://www.linkedin.com/in/iskendersoysal/"> LinkedIn </a> | iskendersoysal@protonmail.com

# Cygwin Setup

Don't have the privileges to install Ubuntu subsystem on your Windows computer at work but can't give up Bash?

Download <a href="https://www.cygwin.com/setup-x86_64.exe"> Cygwin </a>

Select 'lynx' package during setup.  

After installation run Cygwin and apply following commands in order to install commandline installer for Cygwin:

`lynx -source rawgit.com/transcode-open/apt-cyg/master/apt-cyg > apt-cyg`

`install apt-cyg /bin`

Usage is same as apt-get:
`apt-cyg install nano` 

# Look for a file/folder in Linux machine

`sudo find / | grep <string>`

# Shutdown Linux Machine

`sudo shutdown -h now`