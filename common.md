## Common classes and exceptions

The following common classes are available in namespace `phpsap\common`, and the exceptions are available in namespace `phpsap\exceptions`. See the [php-sap/common repository on github][common].

### Config

* `AbstractConfigContainer` A JSON encodeable configuration container implementing PSR-11 for retrieving configuration parameters.
* `AbstractConfig` Configure basic connection parameters for SAP remote function calls, that are common to both connection types (A, and B)  implementing `\phpsap\interfaces\IConfig`.
* `AbstractConfigA` Configure connection parameters for SAP remote function calls using a specific SAP application server (type A) implementing `\phpsap\interfaces\IConfigA`.
* `AbstractConfigB` Configure connection parameters for SAP remote function calls using load balancing (type B) implementing `\phpsap\interfaces\IConfigB`.

### Connection

* `AbstractConnection` Manages a PHP/SAP connection instance implementing `\phpsap\interfaces\IConnection`.

### Function

* `AbstractFunction` Manages a PHP/SAP remote function call implementing `\phpsap\interfaces\IFunction`.
* `AbstractRemoteFunctionCall` Proxy class to simplify PHP/SAP remote function calls implementing `\phpsap\interfaces\IFunction`.

### Exceptions

* `SapException` a generic exception inherited by all other exception.
* `ConfigKeyNotFoundException` in case a configuration key could not be found.
* `IncompleteConfigException` in case `generateConfig()` cannot get all mandatory keys for the configuration.
* `ConnectionFailedException` in case a connection could not be established.
* `UnknownFunctionException` in case the remote function doesn't exist.
* `FunctionCallException` in case the remote function call failed.


[common]: https://github.com/php-sap/common "Exceptions and abstract classes containing logic for PHP/SAP that is not specific to the underlying PHP module."
