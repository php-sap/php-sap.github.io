## Usage: configure and invoke a SAP remote function call

In order to call a SAP remote function, an instance of
`\phpsap\saprfc\SapRfc` has to be created. Before we do that, let's take a look
on the different elements of a SAP remote function call.

1. The name. You need to know which SAP remote function to call.
2. The parameters. A remote function call might need parameters. These are
   provided as array.
3. The connection configuration. You need to know how to connect to the SAP
   remote system on which you'd like to call the remote function.
4. The remote function API. Which parameters are needed to call the remote
   function and what values can you expect to be returned. Usually you don't
   need to know that. In that case it is determined automagically for you.

These are the four parameters of the `SapRfc` constructor. Except for the name,
all parameters are optional an can be set at a later time.

The class `SapRfc` implements the [JsonSerializable interface
][jsonserializable]. The remote function call name, its parameters and API can
therefore be stored as JSON by using `json_encode` on the `SapRfc` instance.

Let's start with an example.

```php
<?php
use phpsap\classes\Config\ConfigTypeA;
use phpsap\DateTime\SapDateTime;
use phpsap\saprfc\SapRfc;
/**
 * The imaginary SAP remote function requires a
 * date as input and will return a date as output.
 */
$function = new SapRfc('MY_COOL_SAP_REMOTE_FUNCTION');
/**
 * The instance in `$function` has no way to establish a connection
 * because it lacks the necessary configuration. Let's add that
 * missing configuration now.
 */
$configuration = new ConfigTypeA([
    ConfigTypeA::JSON_ASHOST => 'sap.example.com',
    ConfigTypeA::JSON_SYSNR  => '999',
    ConfigTypeA::JSON_CLIENT => '001',
    ConfigTypeA::JSON_USER   => 'username',
    ConfigTypeA::JSON_PASSWD => 'password'
]);
$function->setConfiguration($configuration);
/**
 * SAP has a different idea than ISO about formatting dates and times. However
 * PHP/SAP has your back with \phpsap\DateTime\SapDateTime extending DateTime. 
 */
$inputDate = new DateTime('2019-12-31');
$function->setParams([
    'IV_DATE' => $inputDate->format(SapDateTime::SAP_DATE)
]);
/**
 * Now let's encode this remote function call.
 */
echo json_encode($function, JSON_PRETTY_PRINT) . PHP_EOL;
```

Output

```json
{
  "name": "MY_COOL_SAP_REMOTE_FUNCTION",
  "params": {
    "IV_DATE": "20191231"
  },
  "api": [
    {
      "type": "date",
      "name": "IV_DATE",
      "direction": "input",
      "optional": false
    },
    {
      "type": "date",
      "name": "OV_DATE",
      "direction": "output",
      "optional": false
    }
  ]
}
```

**Attention**: An encoded SAP remote function call contains no connection
configuration. Access data and servers may change more easily than the name,
the API and the parameters.

Now let's unfreeze this SAP remote function call.

```php
<?php
use phpsap\saprfc\SapRfc;
$json = '{"name":"MY_COOL_SAP_REMOTE_FUNCTION","params":{"IV_DATE":"20191231"},"api":[{"type":"date","name":"IV_DATE","direction":"input","optional":false},{"type":"date","name":"OV_DATE","direction":"output","optional":false}]}';
$function = SapRfc::jsonDecode($json);
$configuration = new ConfigTypeA([
    ConfigTypeA::JSON_ASHOST => 'sap2.example.com',
    ConfigTypeA::JSON_SYSNR  => '999',
    ConfigTypeA::JSON_CLIENT => '001',
    ConfigTypeA::JSON_USER   => 'usrnm',
    ConfigTypeA::JSON_PASSWD => 'psswrd'
]);
$function->setConfiguration($configuration);
/**
 * To actually call the SAP remote function, use invoke()
 */
$result = $function->invoke();
```

---

[Go back to usage overview](usage)
 | [Go back to configuring a connection](saprfc-config)

[jsonserializable]: http://php.net/manual/en/class.jsonserializable.php
