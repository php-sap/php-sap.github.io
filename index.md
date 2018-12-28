**ATTENTION: THIS IS WORK IN PROGRESS AND NOT TO BE USED UNTIL THIS MESSAGE DISAPPEARS!**

## What is this?

PHP/SAP defines an layer of abstraction between actual SAP remote function calls and your PHP-Code. Therefore PHP/SAP provides three types of interfaces for abstraction:
1. Configuring a connection to an SAP application server.
2. Connecting to an SAP application server
3. Calling a remote function on an SAP application server.

## The motivation

In case you have legacy code written for PHP <= 5.x using one of these PHP modules to call SAP remote functions, and you need to update this code to a PHP version >= 7.x, this project is for you.

There are currently several open source PHP modules available that are compiled using the SAP RFC SDK or SAP Netweaver RFC SDK. Each one of them handles the main tasks, connecting and calling a remote function, quite differently. To complicate things, none of them are available for old and new versions of PHP.

In order to clean this up, you need to remove the dependency of your PHP code on one of these modules. After that you will be able to switch between modules and PHP versions without the need to touch your code calling SAP remote functions.

```php
<?php
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

## PHP SAP RFC modules

I am aware of the following three PHP modules.

author        | module Name | supported PHP versions | composer package
------------- | ----------- | ---------------------- | ----------------------
Eduard Koucky | `saprfc`    | PHP 4.x - 5.5.x        | `phpsap/saprfc-koucky`
Piers Haring  | `sapnwrfc`  | PHP 5.x                | `phpsap/saprfc-harding`
Gregor Kralik | `sapnwrfc`  | PHP 7.x                | `phpsap/saprfc-kralik`
