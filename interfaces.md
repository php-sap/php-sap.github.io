## Interfaces

The following PHP/SAP interfaces are available in namespace `phpsap\interfaces`. See the [php-sap/interfaces repository on github][interfaces].

### Config

The configuration interfaces are separated into a generic SAP configuration, that contains connection parameters that are common to both connection types (A and B), and connection type specific SAP configuration.

* `IConfig` Configure basic connection parameters for SAP remote function calls, that are common to both connection types (A, and B) and require a `generateConfig()` method, that returns the specific configuration structure of the underlying PHP module.
* `IConfigA` Configure connection parameters for SAP remote function calls using a specific SAP application server (type A).
* `IConfigB` Configure connection parameters for SAP remote function calls using load balancing (type B).

### Connection

* `IConnection` Manage a PHP/SAP connection instance.

### Function

* `IFunction` Manage a PHP/SAP remote function call.

### Exceptions

* `ISapException` Generic SAP exception.
* `IIncompleteConfigException` The configuration is incomplete.
* `IConnectionFailedException` The SAP connection failed.
* `IUnknownFunctionException` The requested remote function could not be found.
* `IFunctionCallException` The SAP remote function call failed.

[interfaces]: https://github.com/php-sap/interfaces "Interfaces for implementing the PHP/SAP API."
