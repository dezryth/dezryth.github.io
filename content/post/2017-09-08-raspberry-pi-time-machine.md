---
title: Set up a Raspberry Pi 3 Time Machine!
author: Scott
type: post
date: 2017-09-09T04:17:55+00:00
url: /2017/09/08/raspberry-pi-time-machine/
categories:
  - Apple
  - Linux
  - Technology
tags:
  - macOS
  - pi
  - raspberry-pi
  - time-machine

---
# RaspberryPi Time Machine

This week I received a new MacBook Pro 2017 to use as my development machine when &#8216;on the go&#8217; (or on the couch, to be more accurate). I also received a larger SSD to replace the boot drive on Desktop PC, and suddenly had a 120GB SSD with no job. I decided to put it to work to hold backups of my new mobile dev machine in the event of the worst case scenario. Now, I could have just tossed it in my external enclosure and plugged it in every once in a while for backups, but I know I&#8217;m too lazy to remember to do that often enough, and backing up wirelessly over the network is just so much more convenient and sounded like the kind of project I&#8217;d have a little fun with. Below is how the process went. If you stumbled upon this, you should be able to follow along with a few possible differences depending on the model of Pi you have. Make sure to read the following before moving to the instructions, as you&#8217;ll appreciate avoiding all the issues I ran into figuring this out. Thanks to [HTG][1] for getting me most of the way there.

# Requirements:

Raspberry Pi **with Raspbian Jessie &#8211;
  
** You may be able to get this to work at some point in the future, or with some modifications, but some of the commands in my instructions WILL NOT work in Raspbian stretch when installing Netatalk.

External Drive

SD/Micro SD Card and reader

# Instructions:

Install Raspbian Jessie Lite onto your PI. I recommend Win32 Disk Imager to write your image to your SD card if you have access to a Windows PC. Otherwise, google around for an equivalent for macOS.

**! WAIT !**
  
Before you go tuck your little headless computer away make sure to enable the SSH server. The SSH server is no longer enabled by default.

Format your external drive on your macOS machine to work with time machine.

Launch disk utility. Select your drive. Give it a name and set the format to &#8216;Mac OS Extended, and scheme to GUID Partition Map. Click Erase.

<img class="alignnone size-full wp-image-128" src="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs1.png?resize=650%2C331&#038;ssl=1" alt="" width="650" height="331" srcset="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs1.png?w=650&ssl=1 650w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs1.png?resize=300%2C153&ssl=1 300w" sizes="(max-width: 650px) 100vw, 650px" data-recalc-dims="1" />

Open finder and right click your drive. Select &#8216;Get Info&#8217;.

<img class="alignnone size-full wp-image-129" src="https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs2.png?resize=533%2C281&#038;ssl=1" alt="" width="533" height="281" srcset="https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs2.png?w=533&ssl=1 533w, https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs2.png?resize=300%2C158&ssl=1 300w" sizes="(max-width: 533px) 100vw, 533px" data-recalc-dims="1" />

Set &#8216;everyone&#8217; to &#8216;Read & Write&#8217;, and check &#8216;Ignore ownership on this volume&#8217;

<img class="alignnone size-full wp-image-130" src="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs3.png?resize=589%2C232&#038;ssl=1" alt="" width="589" height="232" srcset="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs3.png?w=589&ssl=1 589w, https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/hfs3.png?resize=300%2C118&ssl=1 300w" sizes="(max-width: 589px) 100vw, 589px" data-recalc-dims="1" />

Now unmount the drive and go plug it into your Raspberry Pi.

Update your repositories and update your packages.

<pre class="line-numbers"><code class="language-bash">sudo apt-get update
sudo apt-get upgrade</code></pre>

Install packages hfsprogs and hfsplus:

<pre><code class="language-bash">sudo apt-get install hfsprogs hfsplus</code></pre>

Create the following directory:

<pre><code class="language-bash">sudo mkdir -p /media/tm</code></pre>

Modify the permissions of the directory with this command:

<pre><code class="language-bash">sudo chown pi:pi /media/tm</code></pre>

Configure Pi to mount drive on start up.
  
Find your drives UUID (Universally Unique Identifier) with this command:
  
<img class="alignnone wp-image-126 size-full" src="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finding-uuid.png?resize=815%2C269&#038;ssl=1" alt="" width="815" height="269" srcset="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finding-uuid.png?w=815&ssl=1 815w, https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finding-uuid.png?resize=300%2C99&ssl=1 300w, https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finding-uuid.png?resize=768%2C253&ssl=1 768w" sizes="(max-width: 815px) 100vw, 815px" data-recalc-dims="1" />

Modify your fstab file to automatically mount your external drive on boot-up:

<pre><code class="language-bash">sudo nano /etc/fstab</code></pre>

<img class="alignnone wp-image-127" src="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/fstab.png?resize=815%2C346&#038;ssl=1" alt="" width="815" height="346" srcset="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/fstab.png?w=941&ssl=1 941w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/fstab.png?resize=300%2C127&ssl=1 300w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/fstab.png?resize=768%2C326&ssl=1 768w" sizes="(max-width: 815px) 100vw, 815px" data-recalc-dims="1" />

The line you want to add will look like this. Feel free to copy, paste, and modify with your UUID.

<pre><code class="language-bash">UUID="abc68498-4ff7-3e78-8131-4982b8dcc1aa" /media/tm hfsplus force,rw,user,auto 0 0</code></pre>

**! BE CAREFUL !**
  
Make sure you enter this correctly, as messing up here and not realizing will corrupt your fstab file and if you reboot before correcting it, your pi will not boot correctly. In the event of this, you&#8217;ll have to edit your fstab file from another Linux system to fix your mistake.
  
Test that your drive mounts correctly after this modification:

<pre><code class="language-bash">sudo mount -a</code></pre>

If you don&#8217;t get an error here, then you&#8217;re set.

Install Netatalk:
  
Use the following command to install all the dependencies you need at once
  
`sudo aptitude install build-essential libevent-dev libssl-dev libgcrypt11-dev libkrb5-dev libpam0g-dev libwrap0-dev libdb-dev libtdb-dev default-libmysqlclient-dev avahi-daemon libavahi-client-dev libacl1-dev libldap2-dev libcrack2-dev systemtap-sdt-dev libdbus-1-dev libdbus-glib-1-dev libglib2.0-dev libio-socket-inet6-perl tracker libtracker-sparql-1.0-dev libtracker-miner-1.0-dev`

Download the latest version of [Netatalk][2] by making sure you have the correct version number with the following command

<pre><code class="language-bash">wget http://prdownloads.sourceforge.net/netatalk/netatalk-3.1.11.tar.gz</code></pre>

Unpack the file:

<pre><code class="language-bash">tar -xf netatalk-3.1.11.tar.gz</code></pre>

Switch to the new folder created, and run this command to configure Netatalk&#8217;s settings:

<pre class="line-numbers"><code class="language-bash">./configure \
--with-init-style=debian-systemd \
--without-libevent \
--without-tdb \
--with-cracklib \
--enable-krbV-uam \
--with-pam-confdir=/etc/pam.d \
--with-dbus-daemon=/usr/bin/dbus-daemon \
--with-dbus-sysconf-dir=/etc/dbus-1/system.d \
--with-tracker-pkgconfig-version=1.0</code></pre>

You are now going to build the source in this folder. This takes a little while.

<pre><code class="language-bash">make</code></pre>

&#8230;and install

<pre><code class="language-bash">sudo make install</code></pre>

Ensure it&#8217;s running with this command:

<pre><code class="language-bash">netatalk -V</code></pre>

We&#8217;ve now got to configure Netatalk

<pre><code class="language-bash">sudo nano /etc/nsswitch.conf</code></pre>

To get your Time Machine drive to show up in Finder&#8217;s sidebar you&#8217;ll need to add this to the line that begins with &#8216;hosts&#8217;:

<pre><code class="language-bash">mdns4 mdns</code></pre>

<img class="alignnone wp-image-134" src="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/nsswitchconf.png?resize=875%2C368&#038;ssl=1" alt="" width="875" height="368" srcset="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/nsswitchconf.png?w=951&ssl=1 951w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/nsswitchconf.png?resize=300%2C126&ssl=1 300w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/nsswitchconf.png?resize=768%2C323&ssl=1 768w" sizes="(max-width: 875px) 100vw, 875px" data-recalc-dims="1" />

Edit apfd.service

<pre><code class="language-bash">sudo nano /etc/avahi/services/afpd.service</code></pre>

&#8230;and add this block of text to the file:

<pre class="line-numbers"><code class="language-markup">&lt;?xml version="1.0" standalone='no'?&gt;&lt;!--*-nxml-*--&gt;
&lt;!DOCTYPE service-group SYSTEM "avahi-service.dtd"&gt;
&lt;service-group&gt;
    &lt;name replace-wildcards="yes"&gt;%h&lt;/name&gt;
    &lt;service&gt;
        &lt;type&gt;_afpovertcp._tcp&lt;/type&gt;
        &lt;port&gt;548&lt;/port&gt;
    &lt;/service&gt;
    &lt;service&gt;
        &lt;type&gt;_device-info._tcp&lt;/type&gt;
        &lt;port&gt;0&lt;/port&gt;
        &lt;txt-record&gt;model=TimeCapsule&lt;/txt-record&gt;
    &lt;/service&gt;
&lt;/service-group&gt;</code></pre>

Now set up your external drive as a network share.

<pre><code class="language-bash">sudo nano /usr/local/etc/afp.conf</code></pre>

Change the two sections that start with [Global] and [My Time Machine Volume] to this text:

<pre class="line-numbers"><code class="language-markup">[Global]
mimic model = TimeCapsule6,106

[Time Machine]
path = /media/tm
time machine = yes</code></pre>

Now you need to start the network services with these commands:

<pre class="line-numbers"><code class="language-bash">sudo service avahi-daemon start
sudo service netatalk start</code></pre>

**! IMPORTANT !**
  
Make sure you set your Pi to start these services every time your Pi boots with these commands:

<pre class="line-numbers"><code class="language-bash">sudo systemctl enable avahi-daemon
sudo systemctl enable netatalk</code></pre>

_We&#8217;re almost done._

Now hop on to your Mac and you should see your Pi under &#8216;shared&#8217; inside of Finder.
  
<img class="alignnone size-full wp-image-131" src="https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder1.png?resize=650%2C379&#038;ssl=1" alt="" width="650" height="379" srcset="https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder1.png?w=650&ssl=1 650w, https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder1.png?resize=300%2C175&ssl=1 300w" sizes="(max-width: 650px) 100vw, 650px" data-recalc-dims="1" />

Hit &#8216;Command + K&#8217; to connect to the server with your Pi&#8217;s IP address (which you should have given a static IP from your router)
  
<img class="alignnone size-full wp-image-132" src="https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder2.png?resize=600%2C298&#038;ssl=1" alt="" width="600" height="298" srcset="https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder2.png?w=600&ssl=1 600w, https://i2.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder2.png?resize=300%2C149&ssl=1 300w" sizes="(max-width: 600px) 100vw, 600px" data-recalc-dims="1" />

Now go to System Preferences > Time Machine and select the drive as your Time Machine backup.
  
<img class="alignnone size-full wp-image-133" src="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder3.png?resize=650%2C355&#038;ssl=1" alt="" width="650" height="355" srcset="https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder3.png?w=650&ssl=1 650w, https://i1.wp.com/scottrchristian.com/wp-content/uploads/2017/09/finder3.png?resize=300%2C164&ssl=1 300w" sizes="(max-width: 650px) 100vw, 650px" data-recalc-dims="1" />

&#8230; And you&#8217;re done!

<img class="alignnone size-full wp-image-136" src="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/completedtimemachinesetup.png?resize=980%2C645&#038;ssl=1" alt="" width="980" height="645" srcset="https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/completedtimemachinesetup.png?w=1344&ssl=1 1344w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/completedtimemachinesetup.png?resize=300%2C197&ssl=1 300w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/completedtimemachinesetup.png?resize=768%2C505&ssl=1 768w, https://i0.wp.com/scottrchristian.com/wp-content/uploads/2017/09/completedtimemachinesetup.png?resize=1024%2C674&ssl=1 1024w" sizes="(max-width: 980px) 100vw, 980px" data-recalc-dims="1" />

Now to get a bigger drive!

## You&#8217;ve now got a fully functional Raspberry Pi Time Machine!

Thanks again to [HTG][1] for the initial guide I followed and for not getting upset over me borrowing your macOS screenshots.

&nbsp;

 [1]: https://www.howtogeek.com/276468/how-to-use-a-raspberry-pi-as-a-networked-time-machine-drive-for-your-mac/
 [2]: http://netatalk.sourceforge.net/