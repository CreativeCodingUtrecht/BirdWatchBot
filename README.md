# BirdWatchBot Workshop

BirdWatchBot is a hands-on workshop created by [Creative Coding Utrecht](https://creativecodingutrecht.nl).

## Introduction

### What is BirdNET-Pi?
The [BirdNET-PI](https://github.com/Nachtzuster/BirdNET-Pi) is a realtime acoustic bird classification system for the Raspberry Pi 5, 4B, 400, 3B+, and 0W2. 

BirdNET-Pi is built on the [BirdNET framework](https://github.com/kahst/BirdNET-Analyzer). It is able to recognize bird sounds from a USB microphone or sound card in near realtime and share its data with the rest of the world.

### Share your bird data with the word
[BirdWeather](https://www.birdweather.com/) is a pioneering visualization platform that harnesses the [BirdNET](https://birdnet.cornell.edu/) artificial neural network to monitor bird vocalizations globally through 2000 active audio stations (and growing). 

The living library of bird vocalizations can be found on [BirdWeather's Live Map](https://app.birdweather.com).

### Share your bird data with the word

The BirdNET-PI uses Apprise which allows the device to send notifications. You can send these notifications to multiple destinations, including Telegram and Discord. 

## Installation steps

### Set up your Raspberry Pi

#### Sourcing the required hardware
For this workshop you'll need: 

* Raspberry Pi 3B+ or higher with power supply
* SD card of at least 16GB 
* USB-lavalier microphone. Check the [BirdNET-Pi forum](https://github.com/mcguirepr89/BirdNET-Pi/discussions/39 ) for suggestions. 
* Your own laptop to login remotely to the Raspberry Pi 

To work directly with the Raspberry Pi in the workshop space or at home, you'll need:
* HDMI-monitor
* HDMI cable for your model Raspberry Pi (either HDMI or Mini-HDMI).
* USB keyboard

#### Flash your SD card
Flash your SD card with the latest Raspberry Pi OS Lite (Bookworm, 64-bit). Since the BirdNET-Pi will not be connected to a keyboard and screen it typically makes sense to install Raspberry Pi OS Lite (Bookworm, 64-bit).

The default login credentials for an SD card provided at the workshop space are:

* Username: `birdnet`
* Password `birdnet`

The instructions for this step can be found in the [Getting Started section of the Raspberry Pi documentation](https://www.raspberrypi.com/documentation/computers/getting-started.html#installing-the-operating-system).

#### Connect and power up your Raspberry Pi

Insert your SD card with Raspberry Pi OS in the microSD slot of the Raspberry Pi. 

Connect the Raspberry Pi to a screen using a (Mini) HDMI cable.

Connect the USB microphone to the Raspberry Pi. 

Then, insert your power supply in the power port of the Raspberry Pi (either Micro USB or USB C).

Optionally connect a USB keyboard to the Raspberry Pi.

After its first time booting up, the Raspberry Pi will display a login screen that also contains the IP address of this device. 

```
Debian GNU/Linux 12 birdnet-pi tty1

My IP address is 172.31.21.191 fe80::f5d5:3236:b3c3:565
```

This confirms that the Raspberry Pi works. In this example, the Raspberry Pi is available on the local network using its IP address `172.31.21.191`. Write this IP address down for later reference.

You can now choose to further configure the Raspberry Pi using the keyboard while connected to the screen, or to login remotely with SSH (Secure Shell) from your own laptop. 

_More detailed instructions for this step can be found in the [Getting Started section of the Raspberry Pi documentation](https://www.raspberrypi.com/documentation/computers/getting-started.html#setting-up-your-raspberry-pi)._

#### Remote login (SSH) to your Raspberry Pi 
In the previous step you determined the IP address of the Raspberry Pi (e.g. `172.31.21.191`). You can login remotely to the Raspberry Pi using an SSH client. 

For instance, on Linux or MacOS, connect to a Raspberry Pi with IP address `172.31.21.191` by typing:

```
ssh birdnet@172.31.21.191
```

When you connect to the Raspberry Pi for the first time it will ask you whether you trust the device. You can answer this question with `yes`. 

```
The authenticity of host '172.31.21.191 (172.31.21.191)' can't be established.
ED25519 key fingerprint is SHA256:XGuoIFXoTS0cxGO3TzIz8Izu50daY1ZIJ58u+2LUWWY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

After typing `yes`, you'll see a message similar to the following: 

```
Warning: Permanently added '172.31.21.191' (ED25519) to the list of known hosts.
```

_On Windows, you'll need to install and use an SSH client such as [PuTTY](https://www.putty.org/) to connect to the Raspberry Pi. Make sure to provide both the username `birdnet` and the IP address (e.g. `172.31.21.191`)._

You can then provice the password. By default, this is set to `birdnet`.

```
birdnet@172.31.21.191's password:
Linux birdnet-pi 6.6.51+rpt-rpi-2712 #1 SMP PREEMPT Debian 1:6.6.51-1+rpt3 (2024-10-08) aarch64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Jan  9 16:46:18 2025
birdnet@birdnet-pi:~ $
```

You can change the default password with the `passwd` command:

```
birdnet@birdnet-pi:~ $ passwd
Changing password for birdnet.
Current password: ****** 
New password: ****** 
Retype new password: ****** 
passwd: password updated successfully
```

### Install BirdNET-Pi
In 2024, [@Nachtzuster](https://github.com/Nachtzuster/BirdNET-Pi) forked and began building on [@mcguirepr89's original implementation](https://github.com/mcguirepr89/BirdNET-Pi) to further update and improve BirdNET-Pi. 

The following steps describe how to install BirdNET-Pi and start identifying birds and sharing the results with the world (i.e. BirdWeather).

#### Run the BirdNET-Pi installation script

Install the software on your Raspberry Pi using the following command that uses the [@Nachtzuster](https://github.com/Nachtzuster/BirdNET-Pi) fork of BirdNET-Pi:

```
curl -s https://raw.githubusercontent.com/Nachtzuster/BirdNET-Pi/main/newinstaller.sh | bash
```

Grab yourself a drink. The installation will take a while (at least 10 minutes). During the installation, make sure to not close your SSH client (e.g. your terminal or PuTTY) and to keep the Raspberry Pi connected to power.

#### Open the BirdNET-Pi dashboard
When the installation is complete, you can connect the BirdNET-Pi dashboard using your browser. To do this, type your IP address in the address bar. 

For instance:

```
http://172.31.21.191
```

#### Test your microphone
In the right hand corner of the BirdNET-Pi dashboard, click on "Live Audio". You can then provide the default login credentials for your BirdNET-Pi:

* Username: `birdnet`
* Password: empty (just press enter)

If everything works well, you should here the sound that being recorded by the Raspberry Pi in real-time.

#### Update the location (latitude/longitude)
In the menu of the BirdNET-Pi dashboard, click on `Tools`. Then, select `Settings`.

In this page, under `Location`, you can provide the location where your BirdNET-Pi will be placed (e.g. your home location). You can use [this Latitude and Longitude Finder](https://www.latlong.net/) to figure out your coordinates.

When you're ready, make sure to click on `Update Settings` at the bottom of the page.

#### Test installation

Now look up a nice recording of bird sounds. For instance, use [this video](https://www.youtube.com/watch?v=D1hBlvv9Y5A) for blackbird (merel) sounds. And play it near the microphone of your Raspberry Pi.

Detected bird sounds will become visible on the BirdNET-Pi dashboard under `Overview`.

### Connect to BirdWeather

#### Create an account on BirdWeather
First, [create a free account](https://app.birdweather.com/login) on BirdWeather.

You can then [create and manage your own station](https://app.birdweather.com/account/stations). In this process, you'll receive a `BirdWeather ID` that can be used to send BirdNET-Pi observations to Bird Weather.

#### Configure the BirdWeather ID on the BirdNET-Pi
In the menu of the BirdNET-Pi dashboard, click on `Tools`. Then, select `Settings`.

Configure your BirdWeather ID under `BirdWeather`.

When you're ready, make sure to click on `Update Settings` at the bottom of the page.

#### Test the connection to BirdWeather
Open [BirdWeather](https://app.birdweather.com/) and search for your own station. 

Since we're in the workshop space, play some bird sound recordings near your microphone. 

### Receive notifications when a bird is detected
The BirdNET-PI uses Apprise to send notifications. You can send these notifications to multiple destinations such as Telegram and Discord. 

In the menu of the BirdNET-Pi dashboard, click on `Tools`. Then, select `Settings`. Notifications can be configured under `Notifications`. 

BirdNET-Pi allows you to configure when you want to receive notifications. You can also change the notification text. 

When you're ready, make sure to click on `Update Settings` at the bottom of the page.

#### Telegram
On Telegram, BirdNET-Pi can send a notification to a Telegram Bot. So, let's create one!

Check out the detailed [Apprise documentation for using Telegram](https://github.com/caronc/apprise/wiki/Notify_telegram) for instructions on how to create your own Bot on Telegram.

You now need to figure out the `chat_id` to configure the Telegram notification correctly. You can use the following URL for this:

```
https://api.telegram.org/bot{YOURBOTTOKEN}/getUpdates
```

For instance, if your Bot token is `7760312342:AAXx5HAxiANK9w-qXucHEl42sEe6GWcc36c` you can use the following URL:

```
https://api.telegram.org/bot7760312342:AAXx5HAxiANK9w-qXucHEl42sEe6GWcc36c/getUpdates


#### Telegram Group
Do you want to follow detections with multiple people? Our suggestion is to create a public Telegram Group to collect BirdNET-Pi notifications. To achieve this, add your Telegram Bot to this group and use the `chatid` of the group when configuring the Apprise notification via Telegram on your BirdNET-Pi.

### Prepare the Raspberry Pi for your own WiFi at home

Start the Raspberry Pi configuration tool on the Raspberry Pi:

```
sudo raspi-config
```

(When asked, type your Raspberry Pi password)

Select `1 System Options`, then `S1 Wireless LAN`. Enter your own WiFi SSID (network name) and passphrase (WiFi password). After selecting `OK`, the Raspberry Pi will try to connect to this network. Time to take it home!

At home, you should be able to connect to the Raspberry Pi using the address `birdnet.local`. This allows you to skip the step where you determine the IP address using a screen.

If this doesn't work right away, connect your Raspberry Pi to a monitor and keyboard and run the Raspberry Pi configuration tool to set up your WiFi correctly.