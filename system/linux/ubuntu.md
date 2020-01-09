# Ubuntu / Xubuntu / Ubuntu Studio

For notes on downloading and creating an image, see:

    linux-distributions.txt

This guide picks up once a base operating system has been installed. (or live run)

If you're installing the system on a virtual machine, see here for notes on those configurations:

    (virtualization)[virtualization.txt]

If on a base / root / hypervisor type machine, install Virtual Box:

    virtualization-hypervisor.txt

If you encrypted your home drive, it's a good idea to note the key shown with "ecryptfs-unwrap-passphrase"

```
sudo apt-get update
sudo apt-get -y install openssh-server
sudo apt-get install -y python
ifconfig

ip address show
```

set up a shared directory with all machines. I use /c... use anything you like
```
export SHARED=/c
sudo mkdir -p /c
sudo chmod 770 /c
sudo chown -R charles: /c
```

Drag link to /c in File Manager

## Ansible

might be a good point to explore ansible here???

if you don't have ansible available elsewhere, install it now (ansible.txt)
if you do have ansible available, install the requirements to be a

https://github.com/city-of-bloomington/system-playbooks

if you have ansible installed on another virtual machine, can use that for the bootstrap. edit hosts.txt:

    cd ansible
    vi hosts.txt

then on control machine run:

    ansible-playbook system-2.yml -i hosts.txt --ask-pass --ask-become-pass



## Choose a Windows Manager

https://i3wm.org/
i3 - improved tiling wm

via:
https://i3wm.org/docs/repositories.html

```
/usr/lib/apt/apt-helper download-file http://debian.sur5r.net/i3/pool/main/s/sur5r-keyring/sur5r-keyring_2019.02.01_all.deb keyring.deb SHA256:176af52de1a976f103f9809920d80d02411ac5e763f695327de9fa6aff23f416

sudo dpkg -i ./keyring.deb
# this throws an error as sudo
sudo echo "deb http://debian.sur5r.net/i3/ $(grep '^DISTRIB_CODENAME=' /etc/lsb-release | cut -f2 -d=) universe" >> /etc/apt/sources.list.d/sur5r-i3.list
sudo apt update
sudo apt install i3
```

# Configs

TODO:
way to boot live with custom configs?

There are some general system settings that are good to take care of first:
(it may be possible to copy the ~/.configs and ~/.mozilla folder from a previous installation, but eventually those will grow out of sync with newer releases)

rm -rf ~/.config
cp -r /c/out-data/.config/ ~/.config

#this does not get startup or session items, but gets most other configurations

rm -rf ~/.mozilla
cp -r /c/out-data/.mozilla/ ~/.mozilla

#cp /media/charles/CHARLES/charles/.hgrc ~/
cp /c/charles/.hgrc ~/.hgrc

cp /c/charles/.gitconfig ~/.gitconfig

# to store passwords:
git config --global credential.helper store
# these are stored in plaintext, so it could be unsecure
# https://stackoverflow.com/questions/5343068/is-there-a-way-to-skip-password-typing-when-using-https-on-github

## Manually:

Set clock, including time format. Enable Date and Month in menu bar.
%a *%Y.%m.%d %H:%M

For multiple monitors, configure them in Settings -> Display

Change Desktop background / wallpaper.
Open Settings from menu -> Desktop
Start->Settings Manager->Desktop

    Style: None
    #7C7C7C ! (or lighter: #C7C7C7)

if you choose a picture here, it will show up on the login screen!

*2013.05.14 20:14:59
All Settings -> Window Manager
move window actions ("minimize", "close", etc) to a different position:
Settings Manager->Windows Manager
under the Style tab, change "Button layout"
X + _ Title v

Stay in Window Manager settings
Under the "Keyboard" tab, clear the window operations menu setting

Then, under All Settings -> Keyboard settings -> Application Shortcuts find:
xfce4-popup-whiskermenu
(xfce4-appfinder also works, but requires tab to select)
and set that to Alt+Space

Create a new shortcut ("+Add") with the command 'xflock4', and the shortcut "Shift+Ctrl+Delete"
This is to be more in line with the way lock screen works on Macs. Not my first choice in combinations, but unfortunately Macs are not as configurable

*2015.11.03 09:57:37
Settings -> Workspaces
Add workspaces (start with 4 - 10)

Optionally, add workspace switcher to Panel
Settings -> Panel -> Items -> Add (right icon) -> Workspace Switcher (bottom of list)

*2015.11.07 15:18:05
be sure a Terminal is already open, then:
Settings -> Session and Startup -> Session -> Save Session

*2016.05.18 21:17:13
to start Task Manager automatically requires a different tactic...
save session will not pick it up.

Settings -> Session and Startup -> Application Autostart
Add a new one:
Task Manager
xfce4-taskmanager


Clean up favorite applications in the launcher (xfce4-popup-whiskermenu):
  - remove old favorites
  - add new:

Can also consider adding favorites to the desktop


Add "Places" to panel so that recent items can be cleaned up / removed:

"I fiured this out.
Add "places" to your panel. It has the function to clear out all recent documents."

via:
https://duckduckgo.com/?q=xfce+clear+recent+files&t=canonical&ia=web
xfce clear recent files at DuckDuckGo
https://forum.xfce.org/viewtopic.php?id=7163
[Solved] How to delete recent documents in xfce? / General discussion / Xfce Forums
https://www.linuxquestions.org/questions/slackware-14/recent-files-in-xfce-711915/
Recent files in XFCE



# This is a useful application for adjusting monitor brightness

sudo apt install ddccontrol gddccontrol ddccontrol-db i2c-tools

sudo gddccontrol

https://duckduckgo.com/?q=control+screen+brightness+desktop+monitor+linux&t=canonical&ia=qa
control screen brightness desktop monitor linux at DuckDuckGo
https://www.reddit.com/r/linux/comments/3gfa7z/desktop_monitor_brightness_control_script/
Desktop monitor brightness control script : linux
https://github.com/zeroping/ddccontrol-backlight
zeroping/ddccontrol-backlight: Bash script for linking ddccontrol to /sys/class/backlight for desktop monitors
https://duckduckgo.com/?q=ddcontrol&t=canonical&ia=web
ddcontrol at DuckDuckGo
https://github.com/ddccontrol/ddccontrol
ddccontrol/ddccontrol: DDC Control


## Password Managers
https://www.google.com/search?q=open+source+password+manager&oq=open+source+passw&aqs=chrome.0.0j69i57j0l4.6551j1j7&client=ubuntu&sourceid=chrome&ie=UTF-8
open source password manager - Google Search
https://hackernoon.com/the-best-password-manager-for-you-747b92c43d18
The Best Password Manager for You – Hacker Noon
https://www.passwordstore.org/
Pass: The Standard Unix Password Manager
https://keepass.info/
KeePass Password Safe


# Steam Controller

working instructions available:
/c/charles/hardware/controllers/steam.md

sudo pip3 install libusb1
sudo pip3 install --upgrade pip
cd /c/
mkdir downloads
cd downloads/
which git
git clone https://github.com/ynsta/steamcontroller.git
cd steamcontroller
sudo python3 setup.py install
sudo vi /etc/udev/rules.d/99-steam-controller.rules
ls /etc/udev/rules.d/
sudo vi /etc/udev/rules.d/99-steam-controller.rules
sudo udevadm control --reload
python3 sc-xbox.py start
sc-xbox.py start
sc-xbox.py stop
sc-desktop.py start
sc-desktop.py start
sc-desktop.py stop
sc-dump.py
sudo sc-dump.py
sudo sc-desktop.py start

also remember to:
  - replace batteries
  - turn the controller on (hold down steam button for 3+ seconds)
  - make sure the usb dongle is plugged in and on

  - using sudo is important


### Application configurations:


set task manager default settings:
  - show all processes
  - sort by cpu utilization


*2015.11.22 12:29:31 terminal
only a subtle tweak has been required...
setting background to be the same color as emacs:
Change terminal profile settings"
"Edit"->"Profile Preferences..."
Under "Colors" tab,
For "Background color", use color picker to match emacs setting (#1D1F21)


[Inkscape Notes](inkscape/inkscape.md)


## FreeCAD
sudo apt-get install freecad

results in error on 18.04

via:
https://techoverflow.net/2018/06/03/how-to-fix-freecad-no-module-named-webgui-on-ubuntu-18-04/

    sudo add-apt-repository ppa:freecad-maintainers/freecad-stable
    sudo apt-get update

sudo apt install freecad


#apply browser configurations sooner rather than later
#this way they'll be available for snapshots in virtual machines

#either from scratch (see browsers.txt)
#then copy browser settings for future installs:
cp -r ~/.mozilla $ARCHIVEPATH/.mozilla
cp -r ~/.mozilla /c/public/downloads/.mozilla

#or copy previous:
#cp -r /c/public/downloads/.mozilla ~/
#cp -r /media/charles/CHARLES/configs/.mozilla/ ~/.mozilla


launch Software Updater
Settings... -> Automatically check for updates: Every two weeks, (Never?)
(depends on the use case)
#16.04 adds new option to download and install security updates automatically
# going to try this out first to see how it impacts things


#GitHub:

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

#screencasting

#via https://en.wikipedia.org/wiki/Comparison_of_screencasting_software:
#https://obsproject.com/download
sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt-get update && sudo apt-get install obs-studio

#see also streaming.txt


# Install Javascript / Node / Vue
/c/charles/technical/javascript/node.txt

/c/charles/technical/javascript/vue/start-here.txt


TODO:
ansible script for spotify
prefer to keep this one on a VM related to music / vinyl / shopping if possible
not a standard / core system component

via:

https://www.spotify.com/us/download/linux/

# 1. Add the Spotify repository signing keys to be able to verify downloaded packages
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys BBEBDCB318AD50EC6865090613B00F1FD2C19886 0DF731E45CE24F27EEEB1450EFDC8610341D9410

# 2. Add the Spotify repository
echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list

# 3. Update list of available packages
sudo apt-get update

# 4. Install Spotify
sudo apt-get install spotify-client


# configure printer





Connect to a network. (Wireless, Wired, your choice!)
May need to press "Auto Etherenet" for wired connection

Add "Incomplete Language Support"


Install Audacity

sudo apt-get install audacity

sudo apt-get install mercurial

sudo apt-get install vlc

sudo apt-get install mixxx

sudo apt-get install inkscape

sudo apt-get install blender
sudo apt-get install gimp

cd /c/public
git clone https://github.com/charlesbrandt/templates
#previously:
git clone https://github.com/charlesbrandt/system
git clone https://github.com/charlesbrandt/moments

sudo apt-get install build-essential python3-dev

sudo apt-get install python3-pip
#or:
cd /c/public/moments
sudo python3 get-pip.py

sudo pip3 install uwsgi
sudo apt-get install nginx
#for notes on configuring, see sortable_list project
/c/public/sortable_list/nginx-sortable_list.conf

#to install local packages on python3, use:
sudo pip3 install -e .





# keyboard shortcut for timestamps (system wide!)

https://www.semicomplete.com/projects/xdotool/

   sudo apt-get install xdotool

https://unix.stackexchange.com/questions/36922/keyboard-shortcut-to-send-text-strings-to-program

   xdotool type 'text'

   xdotool type "$(date +'# %Y.%m.%d %H:%M')"

### 2018.11.04 00:25:29
see screenshots... this attempt doesn't work so far

https://duckduckgo.com/?q=xfce+keyboard+shortcut+output+text&t=canonical&ia=qa
xfce keyboard shortcut output text at DuckDuckGo
https://unix.stackexchange.com/questions/36922/keyboard-shortcut-to-send-text-strings-to-program
Keyboard Shortcut To Send Text Strings To Program - Unix & Linux Stack Exchange
https://www.semicomplete.com/projects/xdotool/xdotool.xhtml
404 Page not found - semicomplete
https://duckduckgo.com/?q=xdotool&t=canonical
xdotool at DuckDuckGo
https://www.semicomplete.com/projects/xdotool/
xdotool - fake keyboard/mouse input, window management, and more - semicomplete


#Ardour

sudo apt-get install ardour

http://ardour.org/


consider starting with:
http://ubuntustudio.org/tour/audio/

todo
separate music system notes



TODO:
medley?

# go ahead with media application configurations next
ansible-playbook system-3.yml -i hosts.txt --ask-become-pass














TODO:
try out atom editor?
sudo apt-cache search atom | grep edit
#no package found... available via apt-get?
sudo apt-get install atom

Trying atom (alway emacs if that doesn't work, mousepad doesn't cut it):
sudo add-apt-repository ppa:webupd8team/atom
sudo apt-get update
sudo apt-get install atom
Ctrl+, to open Settings pane to add packages (online)

install:
atomic-emacs
atom-keyboard-macros

via:
http://askubuntu.com/questions/468359/installing-atom-text-editor-on-32-bit-ubuntu




# Open VLC
# Tools -> Preferences
# Uncheck:
#   - Integrate video in interface
#   - Use only one instance when started from file manager

# View -> Playlist
# Close browser tree in playlist view.
# Uncheck "Docked Playlist







Ansible requires Python installed on the host machine, as well:

    bash
    sudo apt-get install python


#sshd
#just get this one... will help with remote / automated installations
sudo apt-get update
sudo apt-get -y install openssh-server

ifconfig
#to find out ip on guest
#ssh from host to guest first to test (sometimes old keys left around)
ssh 192.168.56.103

if you haven't already, generate local keys:
    ssh-keygen -t rsa

#on host, use ssh-copy-id to log in automatically, e.g.:
ssh-copy-id 192.168.56.103

ssh 127.0.0.1
ssh-copy-id 127.0.0.1



#for a host OS instance, a few steps need to be done manually:
sudo apt-get install -y python-pip python2.7-dev libffi-dev
#ansible
sudo pip install ansible markupsafe

#if you have an ansible host somewhere else, these steps were formalized via:
ansible-playbook ansible.yml -i hosts.txt --ask-become-pass


#(optionally, on guests) shutdown, one more snapshot:
SSH Config


#should be able to use ansible from here
cd /c/public/system/ansible
add new machine ip to hosts.txt
ansible-playbook system.yml -i hosts.txt --ask-become-pass

#might need a restart before running this one:
ansible-playbook system-2.yml -i hosts.txt --ask-become-pass



#don't forget these manual steps:

#back up downloaded files, if you want
export DATE=$(date +%Y%m%d)
rsync -av /home/charles/Downloads/ubuntu/ /c/downloads/ubuntu-$DATE

#to save space, you can clean up downloaded .deb files, however...
#keep downloaded directories around...
#they're used as checks by ansible to see if actions are required
#to save space and clean up downloaded files:

sudo find ~/Downloads/ubuntu/* -iname "*.deb" -exec rm \{\} \;


df -k
16.04x is ~ 4.1GB used at this point

#Snapshot here

Base Configuration

Includes:
 - manual configurations
 - ansible configurations (through system-2, up to media)
 - updates



df -k
16.04x is ~ 4.8GB used at this point

#No need for snapshots here....
#at this point you can start making clones for the common configurations




*2016.05.01 08:43:15
notes on installing virtual_box moved to virtualization.txt
(usually only need this for host machines)



#SAVE THIS FILE!

cd $USBPATH/technical
hg stat
hg add
hg ci -m "updates for new ubuntu system installation"
hg clone $USBPATH/technical $LOCALPATH/technical
cd $LOCALPATH/technical/system
python /c/mindstream/mindstream/launch.py -c /c/technical system




#If on a system with multiple operating systems, but you don't want
#ubuntu to be the default operating system, install startup manager:
sudo apt-get install -y -d startupmanager
mkdir startupmanager
sudo mv /var/cache/apt/archives/*.deb startupmanager
#remember to run startup manager after installation and choose the correct operating system


Archived
----------

Initially, it is a good idea to develop the following steps on a *live* or *virtual* instance in case you install something you didn't mean to.  Eventually things should stabilize enough that you can skip to the locally installed instance and go from there.

Try using 32bit os on virtual machines... sounds like that is more efficient from a memory stand point... shouldn't need to go over 4gb of ram in a VM anyway. However, docker requires 64bit to run in a VM, so that could be a good reason to stick with that.