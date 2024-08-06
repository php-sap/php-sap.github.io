## Interfaces

The following PHP/SAP interfaces are available in namespace `phpsap\interfaces`. See the [php-sap/interfaces repository on github][interfaces].

### Configuration

The configuration interfaces are used to describe connection configuration parameters and their (un)serialization. They are separated into a generic configuration management interface, a common SAP configuration, that contains connection parameters that are common to both connection types (A and B), and connection type specific SAP configuration.

[Continue to the configuration interfaces...](interfaces-config)

### API

The API interfaces are used to describe the API of the SAP remote function. They are separated by their number of attributes. An element, has only type and name attributes, while a value adds a direction and an optional flag and an array adds Sub-members which can only be elements.

[Continue to the API interfaces...](interfaces-api)

### Exceptions

The exception interfaces are used to describe the result of methods in case of errors. They are all children to the overall `ISapException` which should make it easy to generally catch exceptions. Please keep in mind, to throw `InvalidArgumentException` or `LogicException` in case of programming errors as they have nothing to do with the SAP remote function call.

[Continue to the exception interfaces...](interfaces-exceptions)

### Function

The function interface is used to describe how a SAP remove function call is created and invoked.

[Continue to the function interface...](interfaces-function)

[interfaces]: https://github.com/php-sap/interfaces "Interfaces for implementing the PHP/SAP API."
