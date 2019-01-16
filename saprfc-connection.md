## Usage: establish a connection

A new connection object is created using a previously created configuration.

Upon creation of a connection object, the configuration is read from the
 configuration object. In case a mandatory configuration parameter is missing,
 an exception will be thrown.

Creating a connection object doesn't mean an actual connection is established.
 You can determine the actual connection status using `isConnected()` and you
 can use `connect()` to manually establish a connection.

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

try {
    $connection = new SapRfcConnection($config);
} catch (\phpsap\interfaces\exceptions\IIncompleteConfigException $exception) {
    printf(
        'Your SAP configuration is missing mandatory parameter(s). Message: %s',
        $exception->getMessage()
    );
    die();
}

//returns boolean false
$connection->isConnected();

//establish an actual connection to the SAP system
try {
    $connection->connect();
} catch (\phpsap\interfaces\exceptions\IConnectionFailedException $exception) {
    printf(
        'Establishing a connection failed. Message: %s',
        $exception->getMessage()
    );
}

//returns boolean true
$connection->isConnected();

//closes the connection
$connection->close();

//returns boolean false
$connection->isConnected();
```

### Ping a connection

You can determine whether a remote function call is possible by pinging a
 connection using `ping()`.

Calling `ping()` will automatically call `connect()` and can therefore throw an
 `IConnectionFailedException`.

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
    $pingResult = $connection->ping();
} catch (\phpsap\interfaces\exceptions\IConnectionFailedException $exception) {
    printf(
        'Establishing a connection failed. Message: %s',
        $exception->getMessage()
    );
}
```

### Prepare a remote function call

In order to call a SAP remote function you need to prepare the function call
 using `prepareFunction()`.

Calling `prepareFunction()` will automatically call `connect()` and can
 therefore throw an `IConnectionFailedException`.

In case the requested function does not exist, an `IUnknownFunctionException`
 is thrown.

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
    $function = $connection->prepareFunction('HELLO_WORLD');
} catch (\phpsap\interfaces\exceptions\IConnectionFailedException $exception) {
    printf(
        'Establishing a connection failed. Message: %s',
        $exception->getMessage()
    );
} catch (\phpsap\interfaces\exceptions\IUnknownFunctionException $exception) {
   printf(
       'The requested remote function does not exist. Message: %s',
       $exception->getMessage()
   );
}
```

### The connection ID

A connection object contains a connection ID, which is determined from its
 configuration. That means, that the same configuration will result in the same
 connection ID.

Using this you can compare two connection objects in order to find out, whether
 they use the same configuration. However if you call `connect()` on both
 objects, two separate actual connections to the SAP system will be
 established.

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
$connection1 = new SapRfcConnection($config);
$connection2 = new SapRfcConnection($config);

//$same will be boolean true
$same = ($connection1->getId() === $connection2->getId());
```

---

[Go back to usage overview](usage)
 | [Go back to configuring a connection](saprfc-config)
 | [Continue with invoking a function call](saprfc-connection)
