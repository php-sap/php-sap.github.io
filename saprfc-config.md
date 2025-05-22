## Usage: configure a connection

PHP/SAP supports SAP remote system connection types A and B.

* `\phpsap\classes\Config\ConfigTypeA` configures connection parameters for SAP
  remote function calls using a specific SAP application server.
* `\phpsap\classes\Config\ConfigTypeB` configures connection parameters for SAP
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
//ConfigTypeA and ConfigTypeB common methods are in the CommonTrait
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

Of course the JSON encoded configuration is not a dead end. The trait
`\phpsap\classes\Config\Traits\JsonDecodeTrait` provides the method 
`jsonDecode($string)` for the classes `\phpsap\classes\Config\ConfigTypeA` and
`\phpsap\classes\Config\ConfigTypeB` which can be used to directly decode a JSON
encoded configuration into a configuration class.

```php
<?php
use phpsap\classes\Config\ConfigTypeB;
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
 * Let the JsonDecodeTrait determine the type of configuration
 * (A or B) of your formerly encoded JSON.
 */
$configA = ConfigTypeB::jsonDecode($jsonConfig);
/**
 * You can always create an instance of your config
 * using already decoded arrays.
 */
$anotherConfigA = new ConfigTypeA(json_decode($jsonConfig, true));
```

### Set configuration parameters.

You just learned that the constructor of the configuration classes accepts an
array. Now nothing's holding you back to manually build the same array.

```php
<?php
use phpsap\classes\Config\ConfigTypeA;
use phpsap\interfaces\Config\IConfigTypeA;
use phpsap\interfaces\Config\IConfiguration;

$config = new ConfigTypeA([
    IConfigTypeA::JSON_ASHOST => 'sap.example.com',
    IConfigTypeA::JSON_SYSNR  => '999',
    IConfiguration::JSON_CLIENT => '001',
    IConfiguration::JSON_USER   => 'username',
    IConfiguration::JSON_PASSWD => 'password'
]);
```

### Mandatory configuration parameters

**Type A:**

* `\phpsap\interfaces\Config\IConfigTypeA::JSON_ASHOST` The host name of a 
  specific SAP application server.
* `\phpsap\interfaces\Config\IConfigTypeA::JSON_SYSNR` The SAP system number.

**Type B:**

* `\phpsap\interfaces\Config\IConfigTypeB::JSON_MSHOST` The host name of the
  message server.

**Common:**

* `\phpsap\interfaces\Config\IConfiguration::JSON_CLIENT` The destination in
  RfcOpen.
* `\phpsap\interfaces\Config\IConfiguration::JSON_USER` The username to use for
  authentication.
* `\phpsap\interfaces\Config\IConfiguration::JSON_PASSWD` The password to use
  for authentication.

### Optional configuration parameters

**Type A:**

* `\phpsap\interfaces\Config\IConfigTypeA::JSON_GWHOST` The gateway host on an
  application server.
* `\phpsap\interfaces\Config\IConfigTypeA::JSON_GWSERV` The gateway server on an
  application server.

**Type B:**

* `\phpsap\interfaces\Config\IConfigTypeB::JSON_R3NAME` The name of SAP system.
* `\phpsap\interfaces\Config\IConfigTypeB::JSON_GROUP` The group name of the
  application servers.

**Common:**

* `\phpsap\interfaces\Config\IConfiguration::JSON_TRACE` The trace level,
  defined by the constants in `\phpsap\interfaces\Config\IConfiguration`:
    - `TRACE_OFF`
    - `TRACE_BRIEF`
    - `TRACE_VERBOSE`
    - `TRACE_FULL`
* `\phpsap\interfaces\Config\IConfiguration::JSON_CODEPAGE` Only needed it if
  you want to connect to a non-Unicode backend using a non-ISO-Latin-1 username
  or password. The RFC library will then use that codepage for the initial
  handshake, thus preserving the characters in username and password.
* `\phpsap\interfaces\Config\IConfiguration::JSON_LANG` The logon Language.
* `\phpsap\interfaces\Config\IConfiguration::JSON_DEST` The destination in
  RfcOpenConnection.
* `\phpsap\interfaces\Config\IConfiguration::JSON_SAPROUTER` If the connection
  needs to be made through a firewall using a SAPRouter, specify the SAPRouter
  parameters in the following format: `/H/hostname/S/portnumber/H/`

---

[Go back to usage overview](usage)
 | [Continue with configuring the SAP remote function call](saprfc-function)

[jsonserializable]: http://php.net/manual/en/class.jsonserializable.php
