**About this library**

The library is an implementation of `zabix_sender` utility, which sends items data to zabbix server
using the zabbix `trapper` protocol. Because it's a pure NodeJs/javascript implementation, it doesn't
require the `zabix_sender` utility. So there is no any `child_process` involved!

**Basic usage example**

```javascript
var ZabbixSender = require('node-zabbix-sender');
var Sender = new ZabbixSender({host: 'zabbix.example.com'});

// Add items to request
Sender.addItem('webserver', 'httpd.running', 0);
Sender.addItem('dbserver', 'mysql.ping', 1);

// Add item with default host
Sender.addItem('httpd.logs.size', 1024);

// Send the items to zabbix trapper
Sender.send(function(err, res) {
    if (err) {
        throw err;
    }

    // print the response object
    console.dir(res);
});
```
**Instance options**

whenever you create a new instance of zabbix sender, you can pass an options object (e.g `new ZabbixSender(opts)`)
here are the options defaults:

```javascript
{
    host: 'localhost',
    port: 10051,
    timeout: 5000,
    with_timestamps: false,
    items_host: require('os').hostname()
}
```
- `host` and `port` are self-explanatory, the zabbix server host & port
- `timeout` is a socket timeout in milliseconds, when connecting to the zabbix server
- `with_timestamps` when you `addItem`, timestamp will be added as well
- `items_host` a target monitored host in zabbix. used when you don't specify the host when you `addItem`, see example above

**Instance methods**

    addItem([host], key, value)

adds an item to the request payload. The item data won't be sent until the `send` method invoked.

    send(callback)

sends all items that were added to the request payload. the callback function passes 2 arguments. `error` (if any) and response from zabbix server (trapper)

**License**

Licensed under the MIT License. See the LICENSE file for details.