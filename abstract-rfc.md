## Usage: extending `AbstractRemoteFunctionCall`

Let's assume you have a SAP remote function called
 `MY_COOL_SAP_REMOTE_FUNCTION`, that takes a date as input value and returns
  the according week.

The class `MyCoolSapRemoteFunction` contains only code, that configures the API
 of your SAP remote function:

* `getName()` returns the SAP remote function name.
* `setDate()` sets the SAP remote function parameter.
* `getReturnTypecast()` casts the result of the SAP remote function call to a
  DateTime object.

```php
<?php

use phpsap\saprfc\AbstractRemoteFunctionCall;
use kbATeam\TypeCast\TypeCastValue;
use phpsap\DateTime\SapDateTime;

/**
 * Class MyCoolSapRemoteFunction
 *
 * Get the calendar week to a given date.
 */
class MyCoolSapRemoteFunction extends AbstractRemoteFunctionCall
{
    /**
     * Define the name of the remote function.
     * @return string
     */
    public function getName()
    {
        return 'MY_COOL_SAP_REMOTE_FUNCTION';
    }

    /**
     * Set the Date parameter for the remote function call.
     * @param \DateTime $dateTime The date to set.
     * @return $this
     */
    public function setDate(\DateTime $dateTime)
    {
        $this->setParam('IV_DATE', $dateTime->format(SapDateTime::SAP_DATE));
        return $this;
    }
    
    /**
     * Define typecasting for SAP remote function return value. 
     * @return \kbATeam\TypeCast\TypeCastValue
     */
    protected function getReturnTypecast()
    {
        return new TypeCastValue(function ($result) {
            return SapDateTime::createFromFormat(SapDateTime::SAP_WEEK, $result['E_WEEK']);
        });
    }
}
```

With this you will be able to call your SAP remote function in your PHP code
 without having to worry about modules or connections. Plus you get an implicit
 documentation about your SAP remote function in case your editor supports auto
 completion based on composers autoloader. You can even test the API of the SAP
 remote function using unit tests.

In case you're wondering about the SAP connection configuration here,
 [read how to configure a connection](saprfc-config).

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
$dateToWeek = new MyCoolSapRemoteFunction(new SapRfcConfigA($config));
/**
 * Set the parameter for the SAP remote function.
 * Your editor shows you the available parameters of the SAP remote function
 * and their value types.
 */
$dateToWeek->setDate(new DateTime('now'));
//call the SAP remote function
$week = $dateToWeek->invoke();
//echo "<year>W<week>"
echo $week->format('o\Ww') . PHP_EOL;
```

---

[Go back to usage overview](usage)