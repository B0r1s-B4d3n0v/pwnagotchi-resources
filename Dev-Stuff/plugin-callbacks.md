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
            <td>Both</td>
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
            <td>Unknown</td>
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

`def on_ready(self, agent): # Manu & Auto mode`

Called when everything is ready and the main loop is about to start.




<br /><br /><hr /><br /><br />

# Shutdown Triggers

## on_unload <a name="on_unload"></a>

`def on_unload(self, ui): # Toggle to Disable in Web GUI Plugins page`

This will be triggered if the plugin gets unloaded (e.g. the user toggled the enable/disable switch). You should remove unneeded ui-elements here.



## on_rebooting <a name="on_rebooting"></a>

`def on_rebooting(self, agent): # Never`

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


