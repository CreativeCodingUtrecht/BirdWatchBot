# Resolving WiFi persistance issues

Managed guest WiFi networks might cause connection issues where the BirdNET-Pi looses its internet connectivity. The system keeps running and detecting birds, but there is no data being sent over the internet to services such as BirdWeather and AppRise notification services such as Telegam and Signal.

This recipe tries to resolve these connectivity issues.

## Improve WiFi stability

Check the name of your WiFi connection, this will give you the SSID you can use in the following commands (denoted as `YOUR_WIFI_SSID`).

```
nmcli connection show
```

Disable Protected Management Frames (PMF). Some WPA2 setups kick devices if PMF is enabled but not supported correctly.

```
sudo nmcli connection modify "YOUR_WIFI_SSID" wifi-sec.pmf disable
```

Disable MAC Randomization. Prevents the Raspberry Pi from appearing as a new device every time it reconnects.

```
sudo nmcli connection modify "YOUR_WIFI_SSID" 802-11-wireless.mac-address-randomization never
```

Force Aggressive DHCP Renewals. If the DHCP lease is very short (e.g., 300 seconds), this ensures the Raspberry Pi always renews before expiration.

```
sudo nano /etc/NetworkManager/dispatcher.d/99-force-dhcp-renew
```

Paste the following script:

```
#!/bin/bash
# Force a DHCP renew whenever NetworkManager brings the interface up
if [ "$2" = "up" ]; then
    dhclient -v -r wlan0
    dhclient -v wlan0
fi
```

And make the script executable.

```
sudo chmod +x /etc/NetworkManager/dispatcher.d/99-force-dhcp-renew
```

Add a Keep-Alive Ping. Some managed WiFi networks disconnect devices they think are idle. A small cron job will keep the connection active.

```
crontab -e
```

Add this line:

```
*/2 * * * * ping -c 1 8.8.8.8 > /dev/null 2>&1
```

Then restart NetworkManager to make changes active

```
sudo systemctl restart NetworkManager
```

Or simply reboot the system. 

```
sudo reboot
```

