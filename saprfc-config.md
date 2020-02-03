## Usage: configure a connection

PHP/SAP supports SAP remote system connection types A and B.

* `phpsap\classes\Config\ConfigTypeA` configures connection parameters for SAP
  remote function calls using a specific SAP application server.
* `phpsap\classes\Config\ConfigTypeB` configures connection parameters for SAP
  remote function calls using load balancing.

The configuration classes implement the [JsonSerializable interface
][jsonserializable]. The configuration can therefore be stored as JSON by using
 `json_encode` on the configuration object.

### Set configuration parameters and JSON encode them

**All possible configuration parameters are methods of the respective
configuration type.** That way you don't need to know their exact names, as long
as your editor supports auto-completion for classes loaded via composer.

```php
<?php
use phpsap\classes\Config\ConfigTypeA;
//Create an empty configuration object of type A.
$config = new ConfigTypeA();
//ConfigCommon methods are inherited by ConfigTypeA
$config->setAshost('sap.example.com')
       ->setSysnr('999')
       ->setClient('001')
       ->setUser('username')
       ->setPasswd('password');
//JSON encode the configuration.
echo json_encode($config, JSON_PRETTY_PRINT) . PHP_EOL;
```

Output

```json
{
  "ashost": "sap.example.com",
  "sysnr": "999",
  "client": "001",
  "user": "username",
  "passwd": "password"
}
```

### Get a configuration from JSON

Of course the JSON encoded configuration is not a dead end. The class
`phpsap\classes\Config\ConfigCommon` provides the method `jsonDecode($string)`
for directly decoding a JSON encoded configuration into a configuration class.

```php
<?php
use phpsap\classes\Config\ConfigCommon;
use phpsap\classes\Config\ConfigTypeA;
//Let's assume the JSON encoded config from before.
$jsonConfig = '{
  "ashost": "sap.example.com",
  "sysnr": "999",
  "client": "001",
  "user": "username",
  "passwd": "password"
}';
/**
 * Let ConfigCommon determine the type of configuration
 * (A or B) of your formerly encoded JSON.
 */
$configA = ConfigCommon::jsonDecode($jsonConfig);
/**
 * You can always create an instance of your config
 * using already decoded arrays ...
 */
$anotherConfigA = new ConfigTypeA(json_decode($jsonConfig, true));
// ... or objects.
$stillConfigA = new ConfigTypeA(json_decode($jsonConfig, false));
```

### Set configuration parameters.

You just learned that the constructor of the configuration classes accepts an
array. Now nothing's holding you back to manually build the same array.

**All possible configuration parameters are constants of the respective
configuration type.** That way you don't need to know their exact names, as long
as your editor supports auto-completion for classes loaded via composer.

```php
<?php
use phpsap\classes\Config\ConfigTypeA;
/**
 * ConfigCommon constants are inherited by ConfigTypeA.
 */
$config = new ConfigTypeA([
    ConfigTypeA::JSON_ASHOST => 'sap.example.com',
    ConfigTypeA::JSON_SYSNR  => '999',
    ConfigTypeA::JSON_CLIENT => '001',
    ConfigTypeA::JSON_USER   => 'username',
    ConfigTypeA::JSON_PASSWD => 'password'
]);
```

### Mandatory configuration parameters

**Type A:**

* `ConfigTypeA::JSON_ASHOST` The host name of a specific SAP application server.
* `ConfigTypeA::JSON_SYSNR` The SAP system number.

**Type B:**

* `ConfigTypeB::JSON_MSHOST` The host name of the message server.

**Common:**

* `ConfigCommon::JSON_CLIENT` The destination in RfcOpen.
* `ConfigCommon::JSON_USER` The username to use for authentication.
* `ConfigCommon::JSON_PASSWD` The password to use for authentication.

### Optional configuration parameters

**Type A:**

* `ConfigTypeA::JSON_GWHOST` The gateway host on an application server.
* `ConfigTypeA::JSON_GWSERV` The gateway server on an application server.

**Type B:**

* `ConfigTypeB::JSON_R3NAME` The name of SAP system.
* `ConfigTypeB::JSON_GROUP` The group name of the application servers.

**Common:**

* `ConfigCommon::JSON_TRACE` The trace level, defined by the constants in
  `ConfigCommon`:
    - `TRACE_OFF`
    - `TRACE_BRIEF`
    - `TRACE_VERBOSE`
    - `TRACE_FULL`
* `ConfigCommon::JSON_CODEPAGE` Only needed it if you want to connect to a
  non-Unicode backend using a non-ISO-Latin-1 username or password. The RFC
  library will then use that codepage for the initial handshake, thus
  preserving the characters in username and password.
* `ConfigCommon::JSON_LANG` The logon Language.
* `ConfigCommon::JSON_DEST` The destination in RfcOpenConnection.
* `ConfigCommon::JSON_SAPROUTER` If the connection needs to be made through a
  firewall using a SAPRouter, specify the SAPRouter parameters in the following
  format: `/H/hostname/S/portnumber/H/`

---

[Go back to usage overview](usage)
 | [Continue with configuring the SAP remote function call](saprfc-function)

[jsonserializable]: http://php.net/manual/en/class.jsonserializable.php
