## Usage overview

Use `composer require` to add PHP/SAP to your project. Depending on the
 [PHP module](php-modules) you need to require a certain PHP/SAP composer
 package.

The namespace of the library is `\phpsap\saprfc`.

In order to call a SAP remote function, you need to
 [configure](saprfc-config) a [connection](saprfc-connection) and then invoke a
 [function](saprfc-function).

In case you want to use the SAP remote function API like any other PHP class,
 you can [extend `AbstractRemoteFunctionCall`](abstract-rfc) and you will get
 implicit documentation of your SAP remote function call parameters and the
 return value(s).

[Continue reading on how to configure a connection](saprfc-config).
