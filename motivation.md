## The motivation

I inherited a project running on PHP 5.5.x containing undocumented code dealing
 with SAP remote function calls. As a way to connect, [Eduard Kouckys SAPRFC
 module][koucky] had been used. In order to switch to a more current PHP
 version, I had rewrite the PHP code and replace the PHP module too. The
 question was: How can I be sure, that the code of this project dealing with
 SAP remote function calls will not be the source of errors?

That lead to the following requirements:

1. Any code dealing with the actual PHP module has to be simple in order to
   reduce the risk of errors.
2. The code in my project dealing with SAP remote function calls has to work
   for several [PHP modules](php-modules) and it still has to work, in case
   of a switch to a different way of communicating with SAP.
3. Any parameters of a SAP remote function call have to be defined as methods
   as a way of documenting the API of a SAP remote function call via the
   editors autocomplete functionality.
4. The configuration has to be as interchangeable as possible between the PHP
   modules.

### Example 1: connect->prepare->invoke

```php
<?php

use phpsap\saprfc\SapRfcConnection;
use phpsap\saprfc\SapRfcConfigA;

//stored configuration, JSON encoded
$config = '{
    "ashost": "sap.example.com",
    "sysnr": "001",
    "client": "01",
    "user": "username",
    "passwd": "password"
}';
//establish a connection using the stored configuration
$connection = new SapRfcConnection(new SapRfcConfigA($config));
//prepare a function call
$function = $connection->prepareFunction('remoteFunctionName');
//call the remote function
$result = $function->invoke(['paramName' => 'value']);
```

### Example 2: extend remote function call

```php
<?php

use phpsap\common\AbstractRemoteFunctionCall;
use phpsap\interfaces\IConfig;
use phpsap\saprfc\SapRfcConnection;

class ZMyCoolSapRemoteFunction extends AbstractRemoteFunctionCall
{
    /**
     * Create a connection instance using the given config.
     * @param \phpsap\interfaces\IConfig $config
     * @return \phpsap\saprfc\SapRfcConnection
     */
    protected function createConnectionInstance(IConfig $config)
    {
        return new SapRfcConnection($config);
    }

    /**
     * Define the name of the remote function.
     * @return string
     */
    public function getName()
    {
        return 'Z_My_Cool_Sap_Remote_Function';
    }

    /**
     * Create the SAP remote function call parameters as code.
     * @param int $value
     * @return $this
     */
    public function paramA($value)
    {
        if (!is_int($value)) {
            throw new InvalidArgumentException('Expected value to be int.');
        }
        return $this->setParam('paramA', $value);
    }
}
```
```php
<?php

use phpsap\saprfc\SapRfcConfigA;

//stored configuration, JSON encoded
$config = '{
    "ashost": "sap.example.com",
    "sysnr": "001",
    "client": "01",
    "user": "username",
    "passwd": "password"
}';
//create a new SAP remote function instance using the stored configuration
$zMyCoolSapRemoteFuntion = new ZMyCoolSapRemoteFunction(new SapRfcConfigA($config));
//set the parameter for the SAP remote function
$zMyCoolSapRemoteFuntion->paramA(123);
//call the SAP remote function
$result = $zMyCoolSapRemoteFuntion->invoke();
```

[koucky]: http://saprfc.sourceforge.net/ "SAPRFC extension module for PHP"
