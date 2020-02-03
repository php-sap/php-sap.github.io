## The motivation

I inherited a project running on PHP 5.5.x containing undocumented code dealing
 with SAP remote function calls. As a way to connect, [Eduard Kouckys SAPRFC
 module][koucky] had been used. In order to switch to a more current PHP
 version, I had to rewrite the PHP code and replace the PHP module too. The
 question was: How can I be sure, that the code of this project dealing with
 SAP remote function calls will not be the source of errors?

That lead to the following requirements:

1. Any code dealing with the actual PHP module has to be simple in order to
   reduce the risk of errors.
2. The code in my project dealing with SAP remote function calls has to work
   for several [PHP modules](php-modules) and it still has to work, in case
   of a switch to a different way of communicating with SAP.
3. The configuration has to be as interchangeable as possible between the PHP
   modules.

[koucky]: http://saprfc.sourceforge.net/ "SAPRFC extension module for PHP"
