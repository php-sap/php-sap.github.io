## Usage: configure a connection

PHP/SAP supports SAP remote system connection types A and B.

* `\phpsap\saprfc\SapRfcConfigA` configures connection parameters for SAP
  remote function calls using a specific SAP application server.
* `\phpsap\saprfc\SapRfcConfigB` configures connection parameters for SAP
  remote function calls using load balancing.

Each class has a constant defining its type. In case you have a configuration
 instance, you can always determine its type.

```php
<?php
use phpsap\saprfc\SapRfcConfigA;
//creates an empty configuration object of type A
$config = new SapRfcConfigA();
//$type will be assigned value 'A'
$type = $config::TYPE;
```

### Common mandatory configuration parameters

* _client_ The destination in RfcOpen.
* _user_ The username to use for authentication.
* _passwd_ The password to use for authentication.

### Type A mandatory configuration parameters

* _ashost_ The host name of a specific SAP application server.
* _sysnr_ The SAP system number.

### Type B mandatory configuration parameters

* _mshost_ The host name of the message server.

### Type A optional configuration parameters

* _gwhost_
* _gwserv_

### Type B optional configuration parameters

* _r3name_ The name of SAP system.
* _group_ The group name of the application servers.

### Common optional configuration parameters

* _trace_ The trace level, defined by the constants in
  `\phpsap\interfaces\IConfig`:
    - `TRACE_OFF`
    - `TRACE_BRIEF`
    - `TRACE_VERBOSE`
    - `TRACE_FULL`
* _codepage_ Only needed it if you want to connect to a non-Unicode backend
  using a non-ISO-Latin-1 user name or password. The RFC library will then use
  that codepage for the initial handshake, thus preserving the characters in
  username/password.
* _lang_ The logon Language.
* _dest_ The destination in RfcOpenConnection.
* _saprouter_ If the connection needs to be made through a firewall using a
  SAPRouter, specify the SAPRouter parameters in the following format:
  `/H/hostname/S/portnumber/H/`

### JSON configuration

The configuration classes implement the
 [JsonSerializable interface][jsonserializable]. The configuration can
 therefore be stored as JSON by using `json_encode` on the configuration
 object. In return both configuration classes support importing configuration
 from JSON.

```php
<?php
use phpsap\saprfc\SapRfcConfigA;
//configuration previously exported with json_encode()
$configJson = '{
    "ashost": "sap.example.com",
    "sysnr": "001",
    "client": "002",
    "user": "username",
    "passwd": "password"
}';
//import JSON configuration
$config = new SapRfcConfigA($configJson);
//this would result in the same JSON as in $configJson
$configJson2 = json_encode($config);
```

### Array configuration

The configuration classes support importing configuration as associative array.
 In case you have `.ini` files, you can use [parse_ini_file][parse_ini_file] to
 import your configuration as associative array.

```php
<?php
use phpsap\saprfc\SapRfcConfigA;
//configuration as associative array
$configArray = [
    'ashost' => 'sap.example.com',
    'sysnr' => '001',
    'client' => '002',
    'user' => 'username',
    'passwd' => 'password'
];
//import configuration
$config = new SapRfcConfigA($configArray);
```

---

[Go back to usage overview](usage)
 | [Continue with establishing a connection](saprfc-connection)

[jsonserializable]: http://php.net/manual/en/class.jsonserializable.php
[parse_ini_file]: http://php.net/manual/de/function.parse-ini-file.php
