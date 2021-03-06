
node-wifi
===================

![node-wifi](https://raw.githubusercontent.com/friedrith/assets/master/node-wifi/logo.png)

**BE CAREFUL: This documentation is related to the version 2.0 which is planned to be released in few days and not the version currently on npm. The version 2.0 is totally retrocompatible and the interfaces of the functions are exactly the same. Most changes are about the promises and several fields like channel, security_flags or bssid**


The node-wifi module allows mac, windows and linux users to interact with surrounding wifi networks through various methods.

These methods include scanning for wifi access points and connecting to these access points.

We wish to be clear in saying that this module is inspired from [node-wifi-control](https://github.com/msolters/wifi-control-node) but with some slight modifications to certain functions such as the various OS-specific parsers for terminal output as we noticed that these parsers did not work well on certain operating systems.

The module manages :

* Connect for linux | mac | windows
* Scan for linux | mac | windows
* List the current wifi connections for linux | mac | windows
* Disconnect for linux | windows

> As everything with hardware dependency, weird behaviors may happen depending of your configuration. You should never hesitate to notify us about a specificity of your OS/Hardware/Wifi card/whatever.

----------

Install
-------------

```javascript
// Use as a module
npm install node-wifi

// Use as a CLI
npm install node-wifi -g
```

Getting started
-------------

```javascript
var wifi = require('node-wifi');

// Initialize wifi module
// Absolutely necessary even to set interface to null
wifi.init({
    iface : null // network interface, choose a random wifi interface if set to null
});

// Scan networks
wifi.scan(function(err, networks) {
    if (err) {
        console.log(err);
    } else {
        console.log(networks);
        /*
        networks = [
            {
              ssid: '...',
              bssid: '...',
              mac: '...', // equals to bssid (for retrocompatibility)
              channel: <number>,
              frequency: <number>, // in MHz
              signal_level: <number>, // in dB
              security: 'WPA WPA2' //
              security_flags: '...' // encryption protocols (format currently depending of the OS)
              mode: '...' // network mode like Infra (format currently depending of the OS)
            },
            ...
        ];
        */
    }
});

// Connect to a network
wifi.connect({ ssid : "ssid", password : "password"}, function(err) {
    if (err) {
        console.log(err);
    }
    console.log('Connected');
});

// Disconnect from a network
// not available on all os for now
wifi.disconnect(function(err) {
    if (err) {
        console.log(err);
    }
    console.log('Disconnected');
});

// Disconnect from a network
// not available on all os for now
wifi.getCurrentConnections(function(err, currentConnections) {
    if (err) {
        console.log(err);
    }
    console.log(currentConnections);
    /*
    // you may have several connections
    [
        {
            iface: '...', // network interface used for the connection, not available on macOS
            ssid: '...',
            bssid: '...',
            mac: '...', // equals to bssid (for retrocompatibility)
            channel: <number>,
            frequency: <number>, // in MHz
            signal_level: <number>, // in dB
            security: '...' //
            security_flags: '...' // encryption protocols (format currently depending of the OS)
            mode: '...' // network mode like Infra (format currently depending of the OS)
        }
    ]
    */
});

// All functions also return promise if there is no callback given
wifi.scan().then(function (networks) {
  // networks
}).catch(function (error) {
  // error
})


```

Use as CLI
-------------

```javascript
wifi --scan

wifi --connect --ssid <ssid> --password <password> [--iface <wlan0>]

wifi --disconnect

wifi --current
```

Dependencies
-------------

Linux:
* network-manager
