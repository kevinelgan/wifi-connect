> Easy WiFi setup for Chatterbox from your mobile phone or laptop

WiFi Connect is a utility for dynamically setting the WiFi configuration on Chatterbox via a captive portal. 
WiFi credentials are specified by connecting with a mobile phone or laptop to the access point that WiFi Connect creates.

![How it works](./docs/images/how-it-works.png?raw=true)

How it works
------------

WiFi Connect interacts with NetworkManager, which should be the active network manager on the device's host OS.

### 1. Advertise: Device Creates Access Point

WiFi Connect detects available WiFi networks and opens an access point with a captive portal. Connecting to this access point with a mobile phone or laptop allows new WiFi credentials to be configured.

### 2. Connect: User Connects Phone to Device Access Point

Connect to the opened access point on the device from your mobile phone or laptop. The access point SSID is, by default, `chatterbox`. It can be changed by setting the `--portal-ssid` command line argument or the `PORTAL_SSID` environment variable 
By default, the network is unprotected, but a WPA2 passphrase can be added by setting the `--portal-passphrase` command line argument or the `PORTAL_PASSPHRASE` environment variable.

### 3. Portal: Phone Shows Captive Portal to User

After connecting to the access point from a mobile phone, it will detect the captive portal and open its web page. Opening any web page will redirect to the captive portal as well.

If captive portal page does not show up access `192.168.42.1` in your browser

### 4. Credentials: User Enters Local WiFi Network Credentials on Phone

The captive portal provides the option to select a WiFi SSID from a list with detected WiFi networks and enter a passphrase for the desired network.

### 5. Connected!: Device Connects to Local WiFi Network

When the network credentials have been entered, WiFi Connect will disable the access point and try to connect to the network. If the connection fails, it will enable the access point for another attempt. If it succeeds, the configuration will be saved by NetworkManager.

---

For a complete list of command line arguments and environment variables check out our [command line arguments](./docs/command-line-arguments.md) guide.

The full application flow is illustrated in the [state flow diagram](./docs/state-flow-diagram.md).

***

Manual Installation
------------


WiFi Connect depends on NetworkManager, but by default Raspbian Stretch uses dhcpcd as a network manager. 
The provided installation shell script disables dhcpcd, installs NetworkManager as the active network manager and downloads and installs Chatterbox WiFi Connect.

Run the following in your terminal, then follow the onscreen instructions:

`bash <(curl -L https://github.com/GigundoAI/wifi-connect/raw/master/scripts/raspbian-install.sh)`


Supported boards / dongles
--------------------------

WiFi Connect has been successfully tested using the following WiFi dongles:

Dongle                                     | Chip
-------------------------------------------|-------------------
[TP-LINK TL-WN722N](http://bit.ly/1P1MdAG) | Atheros AR9271
[ModMyPi](http://bit.ly/1gY3IHF)           | Ralink RT3070
[ThePiHut](http://bit.ly/1LfkCgZ)          | Ralink RT5370

It has also been successfully tested with the onboard WiFi on a Raspberry Pi 3.

Given these results, it is probable that most dongles with *Atheros* or *Ralink* chipsets will work.

The following dongles are known **not** to work (as the driver is not friendly with access point mode or NetworkManager):

* Official Raspberry Pi dongle (BCM43143 chip)
* Addon NWU276 (Mediatek MT7601 chip)
* Edimax (Realtek RTL8188CUS chip)

Dongles with similar chipsets will probably not work.
