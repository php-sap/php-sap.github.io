**ATTENTION: THIS IS WORK IN PROGRESS AND NOT TO BE USED UNTIL THIS MESSAGE
 DISAPPEARS!**

## About PHP/SAP

PHP/SAP defines a layer of abstraction between actual SAP remote function calls
 and your PHP-Code. As to why this project has been created and which goals it
 wants to achieve, you can read the [motivation](motivation).

[![PHP/SAP](res/php-sap.svg)](res/php-sap.svg)

The [interfaces](interfaces) define a common denominator on how to configure a
 connection to SAP, prepare a SAP remote function call, and invoke a SAP remote
 function call.

The [common classes and exceptions](common) add logic that is not specific to
 the underlying PHP module.

The module specific implementations contain code that is specific to [the
 underlying PHP-module](php-modules).

## TL;DR

Especially short explanation for people in a hurry.

Use `composer require` to add PHP/SAP to your project. Depending on the
 [PHP module](php-modules) you need to require a certain PHP/SAP composer
 package.

```php
<?php
use phpsap\saprfc\SapRfcConfigA;
use phpsap\saprfc\SapRfcConnection;

$result = (new SapRfcConnection(new SapRfcConfigA([
  'ashost' => 'sap.example.com',
  'sysnr' => '001',
  'client' => '002',
  'user' => 'username',
  'passwd' => 'password'
])))
    ->prepareFunction('MY_COOL_SAP_REMOTE_FUNCTION')
    ->invoke(['INPUT_PARAM' => 'value']);
```
 
## Documentation

* [Usage overview](usage)
    - [Configure a connection](saprfc-config)
    - [Establish a connection](saprfc-connection)
    - [Invoke a function call](saprfc-function)
