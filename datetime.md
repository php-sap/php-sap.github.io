## DateTime conversions for SAP

SAP defines week, date, time and timestamp formats, that differ from the ISO
 ones. Parsing these formats is currently not supported by
 [PHPs DateTime class][phpdatetime].

Format    | SAP                                        | ISO
--------- | ------------------------------------------ | --------------------------------------
week      | `<calendar-week-year><calendar-week>`      | `<calendar-week-year>W<calendar-week>`
date      | `<year><month><day>`                       | `<year>-<month>-<day>`
time      | `<hour><minute><second>`                   | `<hour>-<minute>-<second>`
timestamp | `<year><month><day><hour><minute><second>` | `<seconds-since-1970-01-01>`

[SapDateTime][sapdatetime] extends [PHPs DateTime class][phpdatetime] by these
 SAP-specific formats:

* `SAP_WEEK`
* `SAP_DATE`
* `SAP_TIME`
* `SAP_TIMESTAMP`

## Usage

```sh
composer require php-sap/datetime:^1.1
```

### Parse a SAP week string into a DateTime object. 

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = SapDateTime::createFromFormat(SapDateTime::SAP_WEEK, '201846');
echo $dateTime->format('o \w\e\ek W') . PHP_EOL;
/**
 * Output: 2018 week 46
 */
```

### Format a DateTime object as SAP week string

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = new SapDateTime('2018-10-19 08:09:10');
echo $dateTime->format(SapDateTime::SAP_WEEK) . PHP_EOL;
/**
 * Output: 201842
 */
```

### Parse a SAP date string into a DateTime object

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = SapDateTime::createFromFormat(SapDateTime::SAP_DATE, '20181101');
echo $dateTime->format('Y-m-d') . PHP_EOL;
/**
 * Output: 2018-11-01
 */
```

### Format a DateTime object as SAP date

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = new SapDateTime('2018-12-31 09:10:11');
echo $dateTime->format(SapDateTime::SAP_DATE) . PHP_EOL;
/**
 * Output: 20181231
 */
```

### Parse a SAP time string into a DateTime object

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = SapDateTime::createFromFormat(SapDateTime::SAP_TIME, '132001');
echo $dateTime->format('H:i:s') . PHP_EOL;
/**
 * Output: 13-20-01
 */
```

### Format a DateTime object as SAP time

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = new SapDateTime('21:45:05');
echo $dateTime->format(SapDateTime::SAP_TIME) . PHP_EOL;
/**
 * Output: 214505
 */
```

### Parse a SAP timestamp into a DateTime object

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = SapDateTime::createFromFormat(SapDateTime::SAP_TIMESTAMP, '20181019080910');
echo $dateTime->format('Y-m-d H:i:s') . PHP_EOL;
/**
 * Output: 2018-10-19 08:09:10
 */
```

### Format a DateTime object as SAP timestamp

```php
<?php
use phpsap\DateTime\SapDateTime;
$dateTime = new SapDateTime('2018-12-31 09:10:11');
echo $dateTime->format(SapDateTime::SAP_TIMESTAMP) . PHP_EOL;
/**
 * Output: 20181231091011
 */
```

[phpdatetime]: http://php.net/manual/en/class.datetime.php "PHP: DateTime - Manual"
[sapdatetime]: https://github.com/php-sap/datetime "PHP/SAP DateTime repository on GitHub"
