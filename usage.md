## Usage overview

Use `composer require` to add PHP/SAP to your project. Depending on the
 [PHP module](php-modules) you need to require a certain PHP/SAP composer
 package.

 Module Author  | Module      | supported PHP versions | composer require
 -------------- | ----------- | ---------------------- | ------------------------
 Eduard Koucky  | `saprfc`    | PHP 4.x - 5.5.x        | `php-sap/saprfc-koucky`
 Piers Harding  | `sapnwrfc`  | PHP 5.x                | `php-sap/saprfc-harding`
 Gregor Kralik  | `sapnwrfc`  | PHP 7.x                | `php-sap/saprfc-kralik`

The namespace of the library is `\phpsap\saprfc`, no matter which library you use.

In order to call a SAP remote function, you need to
[configure the connection parameters](saprfc-config) and then
[configure the SAP remote function call](saprfc-function).

[Continue reading on how to configure the connection parameters](saprfc-config).

[koucky]: http://saprfc.sourceforge.net/ "SAPRFC extension module for PHP"
[harding]: https://github.com/piersharding/php-sapnwrfc "SAP RFC Connector using the SAP NW RFC SDK for PHP"
[kralik]: https://github.com/gkralik/php7-sapnwrfc "SAP NW RFC SDK extension for PHP7"
