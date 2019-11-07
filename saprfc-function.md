## Usage: invoke a function call

A function object can only be created using `prepareFunction()` on a connection
 object.

In order to assign values to the parameters required by the remote function,
 you can either set these values separately using `setParam()` or you can
 submit an associative array of all parameters to the `invoke()` method.

```php
<?php
use phpsap\saprfc\SapRfcConfigA;
use phpsap\saprfc\SapRfcConnection;

$config = new SapRfcConfigA([
    'ashost' => 'sap.example.com',
    'sysnr' => '001',
    'client' => '002',
    'user' => 'username',
    'passwd' => 'password'
]);

//leave out try+catch for simplification
$connection = new SapRfcConnection($config);

try {
    $remoteFunction = $connection->prepareFunction('MY_COOL_SAP_REMOTE_FUNCTION');
} catch (\phpsap\interfaces\exceptions\IConnectionFailedException $exception) {
    printf(
      'Establishing a connection failed. Message: %s',
      $exception->getMessage()
    );
    die();
} catch (\phpsap\interfaces\exceptions\IUnknownFunctionException $exception) {
    printf(
       'The requested remote function does not exist. Message: %s',
       $exception->getMessage()
    );
    die();
}

//get result from remote function call
try {
    $result = $remoteFunction->invoke();
} catch (\phpsap\interfaces\exceptions\IFunctionCallException $exception) {
    printf(
        'The remote function call failed. Message: %s',
        $exception->getMessage()
    );
}
```

### Setting and removing function call parameters

You can add and overwrite the parameters of a remote function call using
 `setParam()`. In order to remove all parameters set, use `reset()`.

In case you submit an associative array of remote function call parameters to
 the `invoke()` command, all parameters of the same name will get overwritten.

All parameters set either by `setParam()` or `invoke()` are stored in the
 remote function object until `reset()` is called.

```php
<?php
use phpsap\saprfc\SapRfcConfigA;
use phpsap\saprfc\SapRfcConnection;

$config = new SapRfcConfigA([
    'ashost' => 'sap.example.com',
    'sysnr' => '001',
    'client' => '002',
    'user' => 'username',
    'passwd' => 'password'
]);

//leave out try+catch for simplification
$connection = new SapRfcConnection($config);
$remoteFunction = $connection->prepareFunction('MY_COOL_SAP_REMOTE_FUNCTION');

//set remote function parameters IV_RED to 'green' and IV_BLUE to 'yellow'
$remoteFunction->setParam('IV_RED', 'green');
$remoteFunction->setParam('IV_BLUE', 'yellow');

//overwrite remote function parameter IV_RED with 'yellow'
$remoteFunction->setParam('IV_RED', 'yellow');

//remove the parameters IV_RED and IV_BLUE
$remoteFunction->reset();

//set remote function parameters IV_RED to 'red' and IV_BLUE to 'blue'
$remoteFunction->setParam('IV_RED', 'red');
$remoteFunction->setParam('IV_BLUE', 'blue');

/**
 * Invoke a remote function call using the parameters IV_RED='red' and
 * IV_BLUE='blue'.
 */ 
try {
    $result1 = $remoteFunction->invoke();
} catch (\phpsap\interfaces\exceptions\IFunctionCallException $exception) {
    printf(
        'The remote function call failed. Message: %s',
        $exception->getMessage()
    );
}

//overwrite remote function parameter IV_RED with 'green'
$remoteFunction->setParam('IV_RED', 'green');

/**
 * Invoke a remote function call using the parameters IV_RED='green' and
 * IV_BLUE='blue'.
 */ 
try {
    $result2 = $remoteFunction->invoke();
} catch (\phpsap\interfaces\exceptions\IFunctionCallException $exception) {
    printf(
        'The remote function call failed. Message: %s',
        $exception->getMessage()
    );
}
```

---

[Go back to usage overview](usage)
 | [Go back to configuring a connection](saprfc-config)
 | [Go back to establishing a connection](saprfc-connection)
