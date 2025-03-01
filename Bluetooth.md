# Pwnagotchi Bluetooth Troubleshooting
Troubleshooting Bluetooth on the pwnagotchi

This guide applies to Jayofelony's pwnagotchi 2.9.5.3 and maybe more ...\
https://github.com/jayofelony/pwnagotchi/releases

Before troubleshooting ... did you follow the instructions?\
https://github.com/jayofelony/pwnagotchi/wiki

This guide is for Jayofelony bt-tether 1.2\
https://github.com/jayofelony/pwnagotchi/blob/master/pwnagotchi/plugins/default/bt-tether.py

> [!CAUTION]
> Raspberry Pi *All of them* are known to have Bluetooth issues. It's sketchy at best. If it works, and when it works - it works. But if it don't, or won't ... well that's why you're here.

> [!TIP]
> If it worked xxx ago, but not working *now* - and you didn't change *anything* - do not fix what is not broken.\
> If it worked and nothings changed, more than likely the Pi BT drivers have shit the bed. Do **NOT** change anything. Give it time, or a full power cycle (a reboot - not a restart) I've heard of some reports of BT working Auto but not in manual mode. And vise-versa.

> [!CAUTION]
> If you are using 'EvilSocket', 'Aluminum-ice', or other pwnagotchi images; or are using other bt-tether plugins - my Quick Checks > config.toml section is **NOT** for you!!! The rest of the troubleshooting will apply - but the config.toml will be different!!


# 1 - Quick Checks
KISS - <ins>**K**</ins>eep <ins>**I**</ins>t <ins>**S**</ins>imple <ins>**S**</ins>tupid\
Check the easy - often overlooked - things first ...

## 1a - Phone
Is Bluetooth enabled?\
Is Bluetooth in "Discoverable" mode? (when needed)\
Is Bluetooth Tethering enabled?
   - Tethering is needed for internet access via your phones Bluetooth
   - Tethering is different than just "enabling" / connecting Bluetooth.

## 1b - Pwnagotchi
Is bt-tether plugin installed? (installed by default)\
Is bt-tether plugin by Jayofelony version 1.2?\
Is bt-tether plugin enabled? (disabled by default)\
Is bt-tether plugin `config.toml` set correctly?


# 2 - Settings
Enabling Bluetooth (and putting it in "discoverable" mode - when needed) should be obvious - if not, oof ... maybe Lego is more your speed :rofl:

## 2a - Bluetooth Tethering
### 2a - Android
* :gear: Open your phone's Settings.
* :signal_strength: Navigate to "Connections" -or- "Network & Internet".
* Select "Hotspot & tethering".
* Toggle the "Bluetooth tethering" option to on.

> [!TIP]
> While your here, grab your BlueTooth Mac Address
* :gear: Open your phone's Settings.
* :information_source: Navigate to "About Phone".
  - Write down your phones name - caps, spaces, punctuation - all of it.
* then go to "Status Information"
* find "Bluetooth Address" - write it down - you'll need it later.

### 2a - iPhone
LOL GL IDK iPhone :shrug: - maybe ask Siri to do it for you?\
The troubleshooting below is still correct - I just don't know the steps for drilling down the phones menus.

## 2b - Pwnagotchi
### Step 1
edit your `config.toml` whatever way you like.\
eg: `sudo nano /etc/pwnagotchi/config.toml`

> [!WARNING]
> You need to inspect this file carefully.\
> Look for `main.plugins.bt-tether.enabled`

If you found that line, skip the next step.\
If you did **not** find that line keep reading:

> [!WARNING]
> If you find `main.plugins.bt-tether.devices.android-phone.enabled`\
> or
> If you find `main.plugins.bt-tether.devices.ios-phone.enabled`\
> You should stop and re-evaluate the situation. These are old declarations **not** used by Jayofelony pwnagotchi 2.9.5.3 or with Jayofelony bt-tether 1.2.

- paste the following
```
main.plugins.bt-tether.enabled = true
main.plugins.bt-tether.share_internet = true
main.plugins.bt-tether.phone-name = "Badenov's S23 Ultra"
main.plugins.bt-tether.mac = "A1:B2:C3:D4:E5:F6"
main.plugins.bt-tether.phone = "android"
main.plugins.bt-tether.ip = "192.168.44.44"
```

> [!TIP]
> If you used `nano` and want to paste this, make yourself some room above and below the cursor and then right-click the mouse behind the cursor.
### Step 2 - finders - jump back in here ...
> [!NOTE]
> For those of you that found the pre-existing line, as well as those of you that had to copy / paste - everyone should be at the same point now.

Need to make `enabled` and `share_internet` both `true` just as the example above.\
eg:\
`main.plugins.bt-tether.enabled = true`\
`main.plugins.bt-tether.share_internet = true`

`phone-name` - should be exactly as it is displayed on your phone.\
If you have an Android: `Settings > About Phone`.\
If you have an iPhone: IDK ... ask Siri to show you?\
eg: `main.plugins.bt-tether.phone-name = "Badenov's S23 Ultra"`

`mac` - should be your phones **Bluetooth mac address**
* Not your phones "WiFi" mac address
* Not your phones "Ethernet" mac address
* Not your phones "IP" address
* Not your phones EID / FCC Cert / S Pen / Serial NUmber / Up Time / etc.

It should specifically have a **Bluetooth [mac] address**.\
eg: `main.plugins.bt-tether.mac = "A1:B2:C3:D4:E5:F6"`\
colons are required!

`phone` - you've got two options: `android` or `ios` - this is not a typo - there is no "iPhone" option! If you have an iPhone or iWhatever, use `ios`!\
eg: `main.plugins.bt-tether.phone = "android"`

`ip` - This is going to depend on what type of phone you have Android | iPhone
* Android: `192.168.44.44` 
* iPhone: `172.20.10.10`

eg: `main.plugins.bt-tether.ip = "192.168.44.44"`

### Step 3 - Save and try again
> [!NOTE]
> If you made changes - stop here, save your `config.toml` and do a complete power cycle of the pwnagotchi (not a restart - a full reboot!) and see if Bluetooth tethering works.

# 3 - Fu¢k that Blue'footed Bucket'assed Bluetooth 
We've all been there. Still more work to do ... :unamused:

## 3a - To get on a base level
Lets get you to a standard starting point.

### 3a - Phone
On your phone - Unpair and remove / forget pwnagotchi.

### 3a - pwnagotchi
You will need your phones Bluetooth mac address
* Android - Settings > About Phone > Status Information > Bluetooth Address
* iPhone - :shrug: - ask Siri or fiddle-dick with `bluetoothctl scan`

Below - you'll have to put each 'code line' individually into shell/SSH terminal - it sucks - but it is what it is ... and if you try to copy and paste this stuff in blocks into a shell/SSH terminal ... you get what you deserve ... you've been warned!

> [!CAUTION]
> The following mac address `A1:B2:C3:D4:E5:F6` needs to be replaced with your phones Bluetooth mac address.

> [!TIP]
> Copy and paste the whole next section into a text editor - NOT shell/SSH Terminal on your local PC. Do a 'Find & Replace' on the example mac address, and replace it with yours, to make copy and paste easier. Just remember, **1 line at a time**.

`sudo systemctl restart bluetooth.service`\
`bluetoothctl`\
`untrust A1:B2:C3:D4:E5:F6`\
`cancel-pairing A1:B2:C3:D4:E5:F6`\
`disconnect A1:B2:C3:D4:E5:F6`\
`remove A1:B2:C3:D4:E5:F6`

Errors such as "Unknown Device" / "Device not found" / "Device not connected" are ok and expected.

`power on`\
`pairable on`\
`discoverable on`\
`advertise on`\
`menu advertise`\
`name on`\
`back`\
Yes, "back" is an actual command you will need to enter.

This is the point where - if you do not have your BT Mac address you would scan for it. If you alrady have it you can skip this bit.

--- skip if you already have BT Mac ---

> [!TIP]
> After scan has started, and you see your phone listed, - even while the lines are rolling - just start typing scan off and hit enter.

`scan on`\
wait until you see your phones name, then (even while the list is still populating)\
`scan off`

It will give a report of all the devices it identified and you'll have a chance to copy your phones BT Mac address.

--- continue if you have BT Mac ----

Put your phone in "Discoverable Mode" - keep the screen on and in your hand - then paste the following (again, with your own Bluetooth mac address - not mine):\
`pair A1:B2:C3:D4:E5:F6`\
At this point you will be asked to confirm the pairing (on your phone and in terminal) - please do so - or this entire exercise was a waste.

Once paired, then copy and paste the following (again, with your own Bluetooth mac address - not mine):\
`trust A1:B2:C3:D4:E5:F6`\
`connect A1:B2:C3:D4:E5:F6`

:hot_face: Whew! Let's see how it went:\
`devices Paired`\
`devices Trusted`\
`devices Connected`\
Each of the above commands should produce the same response:
> Device A1:B2:C3:D4:E5:F6 Badenov's S23 Ultra

Now, lets exit.\
`exit`

> [!NOTE]
> Stop here and do a complete power cycle of the pwnagotchi (not a restart - a full reboot!) and see if Bluetooth tethering works.

# 4 - Conclusion

If you're tethered ... you're welcome - now pay me - lolz

## 4a - That's a Solid No-Go!
If your still having problems ...

Hop back into the Bluetooth controller\
`bluetoothctl`\
then\
`devices Paired`\
`devices Trusted`\
`devices Connected`\
Each of the above commands should produce the same response:
> Device A1:B2:C3:D4:E5:F6 Badenov's S23 Ultra

If it doesn't - feed your hamster to your goldfish and toss your cat out the window!\
Sorry, start back at the top - you missed something - or you have hardware issues.
