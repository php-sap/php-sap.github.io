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
 
 Module Author            | Module      | supported PHP versions | composer require
 ------------------------ | ----------- | ---------------------- | -------------------------
 [Eduard Koucky][koucky]  | `saprfc`    | PHP 4.x - 5.5.x        | `composer require php-sap/saprfc-koucky`
 [Piers Harding][harding] | `sapnwrfc`  | PHP 5.x                | `composer require php-sap/saprfc-harding`
 [Gregor Kralik][kralik]  | `sapnwrfc`  | PHP 7.x                | `composer require php-sap/saprfc-kralik`

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
    ->setParam('INPUT_PARAM', 'some input value')
    ->invoke();
```
 
## Documentation

* [Usage overview](usage)
    - [Configure a connection](saprfc-config)
    - [Establish a connection](saprfc-connection)
    - [Invoke a function call](saprfc-function)
* [DateTime conversions for SAP](datetime)

[koucky]: http://saprfc.sourceforge.net/ "SAPRFC extension module for PHP"
[harding]: https://github.com/piersharding/php-sapnwrfc "SAP RFC Connector using the SAP NW RFC SDK for PHP"
[kralik]: https://github.com/gkralik/php7-sapnwrfc "SAP NW RFC SDK extension for PHP7"
