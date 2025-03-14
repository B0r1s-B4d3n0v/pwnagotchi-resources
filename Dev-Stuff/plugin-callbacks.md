# Plugin Callbacks

> [!CAUTION]
> AI was removed in 2.9.2
> AI callbacks are deprecated

## Developing your own plugin

This is basically an updated mashup of info from [pwnagotchi.org](https://pwnagotchi.org/contributing/index.html) & [example.py](https://github.com/jayofelony/pwnagotchi/blob/v2.9.5.3/pwnagotchi/plugins/default/example.py) along with my own info.

If you want to develop your own plugin, you have the following callbacks available.\
Click the link to scroll down for example usage and more info.\
You can also see them in action using the `example.py` plugin.

<table>
    <thead>
        <tr>
            <th scope="col">Callback</th>
            <th scope="col">Boot Mode</th>
            <th scope="col">Fire Order</th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row" colspan="4"><h3 name="startup">Start-up Triggers</h3><p>Only fire once when plugin is loaded</p></th>
        </tr>
        <tr>
            <td><a href="#on_loaded">on_loaded</a></td>
            <td>Both</td>
            <td>1</td>
            <td>The plugin enabled and loaded.</td>
        </tr>
        <tr>
            <td><a href="#on_config_changed">on_config_changed</a></td>
            <td>Both</td>
            <td>2</td>
            <td>This will be triggered if the config has changed (also right after on_loaded). See notes below.</td>
        </tr>
        <tr>
            <td><a href="#on_ui_setup">on_ui_setup</a></td>
            <td>Both</td>
            <td>3</td>
            <td>Called to setup the ui elements.</td>
        </tr>
        <tr>
            <td><a href="#on_display_setup">on_display_setup</a></td>
            <td>Both</td>
            <td>4</td>
            <td>Called when the hardware display setup is done, display is an hardware specific object.</td>
        </tr>
        <tr>
            <td><a href="#on_ready">on_ready</a></td>
            <td>Auto</td>
            <td>5</td>
            <td>Called when everything is ready and the main loop is about to start.</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3>Shutdown Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_unload">on_unload</a></td>
            <td>Toggle</td>
            <td>0</td>
            <td>This will be triggered if the plugin gets unloaded (e.g. the user toggled the enable/disable switch). You should remove unneeded ui-elements here.</td>
        </tr>
        <tr>
            <td><a href="#on_rebooting">on_rebooting</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent is rebooting the board. I've never seen this one called.</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3>Time based Triggers - sort of</h3><p>These can be triggered at any time between on_loaded and on_unload and are repeatedly called</p></th>
        </tr>
        <tr>
            <td><a href="#on_ui_update">on_ui_update</a></td>
            <td>Both</td>
            <td>0</td>
            <td>Called when the ui is updated - roughly every second or so.</td>
        </tr>
        <tr>
            <td><a href="#on_internet_available">on_internet_available</a></td>
            <td>Both</td>
            <td>0</td>
            <td>This will be triggered roughly every 5 seconds during the time pwnagotchi has internet.</td>
        </tr>
        <tr>
            <td><a href="#on_epoch">on_epoch</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when an epoch is over (where an epoch is a single loop of the main algorithm).</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3>Event Triggers</h3><p>Triggered because of an event.</p></th>
        </tr>
        <tr>
            <td><a href="#on_association">on_association</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent is sending an association frame.</td>
        </tr>
        <tr>
            <td><a href="#on_channel_hop">on_channel_hop</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>called when the agent is tuning on a specific channel.</td>
        </tr>
        <tr>
            <td><a href="#on_deauthentication">on_deauthentication</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent is deauthenticating a client station from an AP.</td>
        </tr>
        <tr>
            <td><a href="#on_free_channel">on_free_channel</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when a non overlapping wifi channel is found to be free.</td>
        </tr>
        <tr>
            <td><a href="#on_handshake">on_handshake</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when a new handshake is captured, access_point and client_station are json objects if the agent could match the BSSIDs to the current list, otherwise they are just the strings of the BSSIDs.</td>
        </tr>
        <tr>
            <td><a href="#on_wifi_update">on_wifi_update</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent refreshed its access points list.</td>
        </tr>
        <tr>
            <td><a href="#on_unfiltered_ap_list">on_unfiltered_ap_list</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent refreshed an unfiltered access point list this list contains all access points that were detected BEFORE filtering.</td>
        </tr>
        <tr>
            <td><a href="#on_peer_detected">on_peer_detected</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when a new peer is detected.</td>
        </tr>
        <tr>
            <td><a href="#on_peer_lost">on_peer_lost</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when a known peer is lost.</td>
        </tr>
        <tr>
            <td><a href="#on_webhook">on_webhook</a></td>
            <td>Toggle</td>
            <td>0</td>
            <td>You can provide some web-functionality here. Will be triggered if the user opens /plugins/[pluginname].</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3>Status based Triggers</h3><p>Triggered because of an status change</p></th>
        </tr>
        <tr>
            <td><a href="#on_wait">on_wait</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent is waiting for t seconds.</td>
        </tr>
        <tr>
            <td><a href="#on_bored">on_bored</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the status is set to bored.</td>
        </tr>
        <tr>
            <td><a href="#on_excited">on_excited</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the status is set to excited.</td>
        </tr>
        <tr>
            <td><a href="#on_lonely">on_lonely</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the status is set to lonely.</td>
        </tr>
        <tr>
            <td><a href="#on_sad">on_sad</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the status is set to sad.</td>
        </tr>
        <tr>
            <td><a href="#on_sleep">on_sleep</a></td>
            <td>Auto</td>
            <td>0</td>
            <td>Called when the agent is sleeping for t seconds.</td>
        </tr>
   </tbody>
  <tfoot>
        <tr>
            <th scope="row" colspan="4"><h3>AI deprecated</h3><p>AI was deprecated in <a href="https://github.com/jayofelony/pwnagotchi/releases/tag/v2.9.2">v2.9.2</a>. These callbacks have been deprecated as well.</p></th>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_best_reward</td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_policy</span></td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_ready</span></td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_training_end</span></td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_training_start</span></td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_training_step</span></td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
        <tr>
            <td><span style="color:red">on_ai_worst_reward</span></td>
            <td></td>
            <td></td>
            <td>Deprecated.</td>
        </tr>
  </tfoot>
</table>



<hr /><br /><br /><br /><br />

# BetterCap Callbacks

<a name="bcap_verified"></a>
> [!CAUTION]
> I grepped BC code for "Events.Add" and these callbacks were the result. BC website does not reference some of these.\
> "Verified" is because it was either documented, or it was triggered in my test script.\
> "Undocumented | Unverified" - I did not verify the scope of the callbacks - so some of these may be scope limited. ¯\\\_(ツ)_/¯

Sources: <a href="https://www.bettercap.org/modules/core/events.stream/">bettercap.org</a> & `sudo grep -Rh "Events.Add" /home/pi/bettercap/`

<table>
    <thead>
        <tr>
            <th scope="col">Callback</th>
            <th scope="col">Verified</th>
            <th scope="col">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row" colspan="4"><h3 name="bluetooth_triggers">Bluetooth Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_device_new">on_bcap_ble_device_new</a></td>
            <td>★</td>
            <td>A new BLE device has been discovered.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_device_lost">on_bcap_ble_device_lost</a></td>
            <td>★</td>
            <td>A previously discovered BLE device is not in range anymore.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_connection_timeout">on_bcap_ble_connection_timeout</a></td>
            <td>★</td>
            <td>Connection to the specified BLE device timed out.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_device_characteristic_discovered">on_bcap_ble_device_characteristic_discovered</a></td>
            <td>★</td>
            <td>A new characteristic has been discovered for a BLE device.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_device_connected">on_bcap_ble_device_connected</a></td>
            <td>★</td>
            <td>Connected to the selected BLE device.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_device_disconnected">on_bcap_ble_device_disconnected</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_ble_device_service_discovered">on_bcap_ble_device_service_discovered</a></td>
            <td>★</td>
            <td>A new service has been discovered for a BLE device.</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3 name="gps_triggers">GPS Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_gps_new">on_bcap_gps_new</a></td>
            <td>★</td>
            <td>A new GPS Location has been obtained</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3 name="wifi_triggers">Wifi Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_ap_lost">on_bcap_wifi_ap_lost</a></td>
            <td>★</td>
            <td>A previously discovered WiFi access point is not in range anymore.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_ap_new">on_bcap_wifi_ap_new</a></td>
            <td>★</td>
            <td>A new WiFi access point has been discovered.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_bruteforce_success">on_bcap_wifi_bruteforce_success</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_client_deauthentication">on_bcap_wifi_client_deauthentication</a></td>
            <td>★</td>
            <td>WPA/WPA2 deauthentication frame has been detected.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_client_handshake">on_bcap_wifi_client_handshake</a></td>
            <td>★</td>
            <td>WPA/WPA2 key material has been captured.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_client_lost">on_bcap_wifi_client_lost</a></td>
            <td>★</td>
            <td>A previously discovered WiFi client station disconnected from its AP.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_client_new">on_bcap_wifi_client_new</a></td>
            <td>★</td>
            <td>A new WiFi client station has been discovered.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_client_probe">on_bcap_wifi_client_probe</a></td>
            <td>★</td>
            <td>A WiFi client station is sending a probe for an ESSID.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_wifi_deauthentication">on_bcap_wifi_deauthentication</a></td>
            <td>★</td>
            <td>A WiFi client has been deauthenticated</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3 name="can_triggers">Can Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_can_device_lost">on_bcap_can_device_lost</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_can_device_new">on_bcap_can_device_new</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_can_message">on_bcap_can_message</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3 name="hid_triggers">HID Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_hid_device_lost">on_bcap_hid_device_lost</a></td>
            <td>★</td>
            <td>A previously discovered wireless HID device is not in range anymore.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_hid_device_new">on_bcap_hid_device_new</a></td>
            <td>★</td>
            <td>A new wireless HID device has been discovered.</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3 name="http_triggers">HTTP|S Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_http_spoofed_request">on_bcap_http_spoofed_request</a></td>
            <td>★</td>
            <td>A HTTP request has been changed by a proxy module.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_http_spoofed_response">on_bcap_http_spoofed_response</a></td>
            <td>★</td>
            <td>A HTTP response has been changed by a proxy module.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_https_spoofed_request">on_bcap_https_spoofed_request</a></td>
            <td>★</td>
            <td>A HTTPS request has been changed by a proxy module.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_https_spoofed_response">on_bcap_https_spoofed_response</a></td>
            <td>★</td>
            <td>A HTTPS response has been changed by a proxy module.</td>
        </tr>
        <tr>
            <th scope="row" colspan="4"><h3 name="other_triggers">Other Triggers</h3></th>
        </tr>
        <tr>
            <td><a href="#on_bcap_mod_stopped">on_bcap_mod_stopped</a></td>
            <td>★</td>
            <td>A specific module stopped.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_sys_log">on_bcap_sys_log</a></td>
            <td>★</td>
            <td>Simple log message event.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_endpoint_lost">on_bcap_endpoint_lost</a></td>
            <td>★</td>
            <td>A previously discovered network host disconnected from this network.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_endpoint_new">on_bcap_endpoint_new</a></td>
            <td>★</td>
            <td>A new network host has been discovered.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_gateway_change">on_bcap_gateway_change</a></td>
            <td>★</td>
            <td>IPv4 or IPv6 gateway change detected.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_graph_edge_new">on_bcap_graph_edge_new</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_graph_node_new">on_bcap_graph_node_new</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_net_sniff_*">on_bcap_net_sniff_*</a></td>
            <td>★</td>
            <td>A new payload has been sniffed..</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_session_closing">on_bcap_session_closing</a></td>
            <td>★</td>
            <td>The session is stopping.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_session_started">on_bcap_session_started</a></td>
            <td>★</td>
            <td>The session started.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_session_stopped">on_bcap_session_stopped</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_syn_scan">on_bcap_syn_scan</a></td>
            <td>★</td>
            <td>An open port has been found on the target host.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_tick">on_bcap_tick</a></td>
            <td>★</td>
            <td>An event generated by the ticker module.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_update_available">on_bcap_update_available</a></td>
            <td>★</td>
            <td>An update is available.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_zeroconf_service">on_bcap_zeroconf_service</a></td>
            <td></td>
            <td>Undocumented | Unverified.</td>
        </tr>
        <tr>
            <td><a href="#on_bcap_mod_started">on_bcap_mod_started</a></td>
            <td>★</td>
            <td>A specific module started.</td>
        </tr>
</table>





<br /><br /><hr /><br /><br />

# Start-up Triggers
Only fire once when plugin is loaded


## on_loaded <a name="on_loaded"></a>

`def on_loaded(self): # Manu & Auto mode`

The plugin enabled and loaded.


## on_config_changed <a name="on_config_changed"></a>

`def on_config_changed(self, config): # Manu & Auto mode`

This will be triggered if the config has changed (also right after on_loaded).



## on_ui_setup <a name="on_ui_setup"></a>

`def on_ui_setup(self, ui): # Manu & Auto mode`

Called to setup the ui elements.



## on_display_setup <a name="on_display_setup"></a>

`def on_display_setup(self, display): # Manu & Auto mode`

Called when the hardware display setup is done, display is an hardware specific object.



## on_ready <a name="on_ready"></a>

`def on_ready(self, agent): # Auto mode`

Called when everything is ready and the main loop is about to start.




<br /><br /><hr /><br /><br />

# Shutdown Triggers

## on_unload <a name="on_unload"></a>

`def on_unload(self, ui): # Toggle to Disable in Web GUI Plugins page`

This will be triggered if the plugin gets unloaded (e.g. the user toggled the enable/disable switch). You should remove unneeded ui-elements here.



## on_rebooting <a name="on_rebooting"></a>

`def on_rebooting(self, agent): # Auto`

Called when the agent is rebooting the board. I've never seen this one called.




<br /><br /><hr /><br /><br />

# Time based Triggers - sort of
These can be triggered at any time between on_loaded and on_unload and are repeatedly called

## on_ui_update <a name="on_ui_update"></a>

`def on_ui_update(self, ui): # Manu & Auto mode`

Called when the ui is updated - roughly every second or so.



## on_internet_available <a name="on_internet_available"></a>

`def on_internet_available(self, agent): # Manu & Auto mode`

This will be triggered roughly every 5 seconds during the time pwnagotchi has internet.



## on_epoch <a name="on_epoch"></a>

`def on_epoch(self, agent, epoch, epoch_data): # Auto mode`

Called when an epoch is over (where an epoch is a single loop of the main algorithm).




<br /><br /><hr /><br /><br />

# Event Triggers
Triggered because of an event.

## on_association <a name="on_association"></a>

`def on_association(self, agent, access_point): # Auto mode`

Called when the agent is sending an association frame.



## on_channel_hop <a name="on_channel_hop"></a>

`def on_channel_hop(self, agent, channel): # Auto mode`

called when the agent is tuning on a specific channel.



## on_deauthentication <a name="on_deauthentication"></a>

`def on_deauthentication(self, agent, access_point, client_station): # Auto mode`

Called when the agent is deauthenticating a client station from an AP.



## on_free_channel <a name="on_free_channel"></a>

`def on_free_channel(self, agent, channel): # Auto mode`

Called when a non overlapping wifi channel is found to be free.



## on_handshake <a name="on_handshake"></a>

`def on_handshake(self, agent, filename, access_point, client_station): # Auto mode`

Called when a new handshake is captured, access_point and client_station are json objects if the agent could match the BSSIDs to the current list, otherwise are just the strings of the BSSIDs.



## on_wifi_updatep_list <a name="on_wifi_update"></a>

`def on_wifi_update(self, agent, access_points): # Auto mode`

Called when the agent refreshed its access points list.



## on_unfiltered_ap_list <a name="on_unfiltered_ap_list"></a>

`def on_unfiltered_ap_list(self, agent, access_points): # Auto mode`

Called when the agent refreshed an unfiltered access point list this list contains all access points that were detected BEFORE filtering.



## on_peer_detected <a name="on_peer_detected"></a>

`def on_peer_detected(self, agent, peer): # Auto mode`

Called when a new peer is detected.



## on_peer_lost <a name="on_peer_lost"></a>

`def on_peer_lost(self, agent, peer): # Auto mode`

Called when a known peer is lost.



## on_webhook <a name="on_webhook"></a>

`def on_webhook(self, path, request): # Button - Clicking the Web GUI Plugin Title on Plugins page`

You can provide some web-functionality here. Will be triggered if the user opens /plugins/[pluginname].\
Called when http://[host]:[port]/plugins/[plugin]/ is called\
must return a html page\
IMPORTANT: If you use "POST"s, add a csrf-token (via csrf_token() and render_template_string)


<br /><br /><hr /><br /><br />

# Status based Triggers
Triggered because of an status change

## on_wait <a name="on_wait"></a>

`def on_wait(self, agent, t): # Auto mode`

Called when the agent is waiting for t seconds.



## on_bored <a name="on_bored"></a>

`def on_bored(self, agent): # Auto mode`

Called when the status is set to bored.



## on_excited <a name="on_excited"></a>

`def on_excited(self, agent): # Auto mode`

Called when the status is set to excited.



## on_lonely <a name="on_lonely"></a>

`def on_lonely(self, agent): # Auto mode`

Called when the status is set to lonely.



## on_sad <a name="on_sad"></a>

`def on_sad(self, agent): # Auto mode`

Called when the status is set to sad.
            


## on_sleep <a name="on_sleep"></a>

`def on_sleep(self, agent, t): # Auto mode`

Called when the agent is sleeping for t seconds.








<br /><br /><br /><br /><hr /><br /><br /><br /><br />







# BetterCap Callbacks

<br />

# Bluetooth Triggers

## on_bcap_ble_device_new <a name="on_bcap_ble_device_new"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_ble_device_new(self, agent, event):`

A new BLE device has been discovered.


## on_bcap_ble_device_lost <a name="on_bcap_ble_device_lost"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_ble_device_lost(self, agent, event):`

A previously discovered BLE device is not in range anymore.


## on_bcap_ble_connection_timeout <a name="on_bcap_ble_connection_timeout"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_ble_connection_timeout(self, agent, event):`

Connection to the specified BLE device timed out.


## on_bcap_ble_device_characteristic_discovered <a name="on_bcap_ble_device_characteristic_discovered"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_ble_device_characteristic_discovered(self, agent, event):`

A new characteristic has been discovered for a BLE device.


## on_bcap_ble_device_connected <a name="on_bcap_ble_device_connected"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_ble_device_connected(self, agent, event):`

Connected to the selected BLE device.


## on_bcap_ble_device_disconnected <a name="on_bcap_ble_device_disconnected"></a>

`def on_bcap_ble_device_disconnected(self, agent, event):`

Undocumented | Unverified.


## on_bcap_ble_device_service_discovered <a name="on_bcap_ble_device_service_discovered"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_ble_device_service_discovered(self, agent, event):`

A new service has been discovered for a BLE device.



            
# GPS Triggers

## on_bcap_gps_new <a name="on_bcap_gps_new"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_gps_new(self, agent, event):`

A new GPS Location has been obtained.



            
# Wifi Triggers

## on_bcap_wifi_ap_lost <a name="on_bcap_wifi_ap_lost"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_ap_lost(self, agent, event):`

A previously discovered WiFi access point is not in range anymore.


## on_bcap_wifi_ap_new <a name="on_bcap_wifi_ap_new"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_ap_new(self, agent, event):`

A new WiFi access point has been discovered.


## on_bcap_wifi_bruteforce_success <a name="on_bcap_wifi_bruteforce_success"></a>

`def on_bcap_wifi_bruteforce_success(self, agent, event):`

Undocumented | Unverified.


## on_bcap_wifi_client_deauthentication <a name="on_bcap_wifi_client_deauthentication"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_client_deauthentication(self, agent, event):`

WPA/WPA2 deauthentication frame has been detected.


## on_bcap_wifi_client_handshake <a name="on_bcap_wifi_client_handshake"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_client_handshake(self, agent, event):`

WPA/WPA2 key material has been captured.


## on_bcap_wifi_client_lost <a name="on_bcap_wifi_client_lost"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_client_lost(self, agent, event):`

A previously discovered WiFi client station disconnected from its AP.


## on_bcap_wifi_client_new <a name="on_bcap_wifi_client_new"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_client_new(self, agent, event):`

A new WiFi client station has been discovered.


## on_bcap_wifi_client_probe <a name="on_bcap_wifi_client_probe"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_client_probe(self, agent, event):`

A WiFi client station is sending a probe for an ESSID.


## on_bcap_wifi_deauthentication <a name="on_bcap_wifi_deauthentication"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_wifi_deauthentication(self, agent, event):`

A WiFi client has been deauthenticated.



            
# Can Triggers

## on_bcap_can_device_lost <a name="on_bcap_can_device_lost"></a>

`def on_bcap_can_device_lost(self, agent, event):`

Undocumented | Unverified.


## on_bcap_can_device_new <a name="on_bcap_can_device_new"></a>

`def on_bcap_can_device_new(self, agent, event):`

Undocumented | Unverified.


## on_bcap_can_message <a name="on_bcap_can_message"></a>

`def on_bcap_can_message(self, agent, event):`

Undocumented | Unverified.



            
# HID Triggers

## on_bcap_hid_device_lost <a name="on_bcap_hid_device_lost"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_hid_device_lost(self, agent, event):`

A previously discovered wireless HID device is not in range anymore.


## on_bcap_hid_device_new <a name="on_bcap_hid_device_new"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_hid_device_new(self, agent, event):`

A new wireless HID device has been discovered.



            
# HTTP|S Triggers

## on_bcap_http_spoofed_request <a name="on_bcap_http_spoofed_request"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_http_spoofed_request(self, agent, event):`

A HTTP request has been changed by a proxy module.


## on_bcap_http_spoofed_response <a name="on_bcap_http_spoofed_response"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_http_spoofed_response(self, agent, event):`

A HTTP response has been changed by a proxy module.


## on_bcap_https_spoofed_request <a name="on_bcap_https_spoofed_request"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_https_spoofed_request(self, agent, event):`

A HTTPS request has been changed by a proxy module.


## on_bcap_https_spoofed_response <a name="on_bcap_https_spoofed_response"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_https_spoofed_response(self, agent, event):`

A HTTPS response has been changed by a proxy module.



            
# Other Triggers

## on_bcap_mod_stopped <a name="on_bcap_mod_stopped"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_mod_stopped(self, agent, event):`

A specific module stopped.


## on_bcap_sys_log <a name="on_bcap_sys_log"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_sys_log(self, agent, event):`

Simple log message event.


## on_bcap_endpoint_lost <a name="on_bcap_endpoint_lost"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_endpoint_lost(self, agent, event):`

A previously discovered network host disconnected from this network.


## on_bcap_endpoint_new <a name="on_bcap_endpoint_new"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_endpoint_new(self, agent, event):`

A new network host has been discovered.


## on_bcap_gateway_change <a name="on_bcap_gateway_change"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_gateway_change(self, agent, event):`

IPv4 or IPv6 gateway change detected.


## on_bcap_graph_edge_new <a name="on_bcap_graph_edge_new"></a>

`def on_bcap_graph_edge_new(self, agent, event):`

Undocumented | Unverified.


## on_bcap_graph_node_new <a name="on_bcap_graph_node_new"></a>

`def on_bcap_graph_node_new(self, agent, event):`

Undocumented | Unverified.


## on_bcap_net_sniff_* <a name="on_bcap_net_sniff_*"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_net_sniff(self, agent, event):`

A new payload has been sniffed..


## on_bcap_session_closing <a name="on_bcap_session_closing"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_session_closing(self, agent, event):`

The session is stopping.


## on_bcap_session_started <a name="on_bcap_session_started"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_session_started(self, agent, event):`

The session started


## on_bcap_session_stopped <a name="on_bcap_session_stopped"></a>

`def on_bcap_session_stopped(self, agent, event):`

Undocumented | Unverified.


## on_bcap_syn_scan <a name="on_bcap_syn_scan"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_syn_scan(self, agent, event):`

An open port has been found on the target host.


## on_bcap_tick <a name="on_bcap_tick"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_tick(self, agent, event):`

An event generated by the ticker module.


## on_bcap_update_available <a name="on_bcap_update_available"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_update_available(self, agent, event):`

An update is available.


## on_bcap_zeroconf_service <a name="on_bcap_zeroconf_service"></a>

`def on_bcap_zeroconf_service(self, agent, event):`

Undocumented | Unverified.


## on_bcap_mod_started <a name="on_bcap_mod_started"></a> [<a href="#bcap_verified">verified</a>]

`def on_bcap_mod_started(self, agent, event):`

A specific module started.
