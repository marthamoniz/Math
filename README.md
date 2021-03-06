AzaMath
=======

Anizoptera CMF mathematic component.

https://github.com/Anizoptera/Math

[![Build Status][TravisImage]][Travis]


Table of Contents
-----------------

1. [Introduction](#introduction)
2. [Requirements](#requirements)
3. [Installation](#installation)
4. [Examples](#examples)
   * [Numeral systems conversions](#example-1---numeral-systems-conversions)
   * [Custom numeral system](#example-2---custom-numeral-system)
   * [Arbitrary precision arithmetic](#example-3---arbitrary-precision-arithmetic)
   * [Input filtration](#example-4---input-filtration)
   * [Do some operations and then convert to base62](#example-5---do-some-operations-and-then-convert-to-base62)
5. [Tests](#tests)
6. [Credits](#credits)
7. [License](#license)
8. [Links](#links)


Introduction
------------

Provides functionality to work with large numbers with arbitrary precision (using BCMath).
and universal convertor between positional numeral systems (supported bases from 2 to 62 inclusive, and custom systems; pure PHP realisation, but can use GMP and core PHP functions for speed optimization).

Features:

* Functionality to work with large numbers (integers, floats) with arbitrary precision (requires [BCMath][]). Can work with floats (E notation too!) and without loosing precision (as far as possible). It supports all basic arithmetic, exponentiation, square root, modulus, bit shift, rounding, comparison, and some other operations.
* Very simple math for big integers (only integers!, has a native PHP realization and can use BCMath or GMP for speedup).
* Universal number (and huge number!) convertor between positional numeral systems (supported bases from 2 to 62 inclusive, and systems with custom alphabet; pure PHP realisation, but can use [GMP][] and [core PHP](http://php.net/math) functions for speed optimization). Negative and huge integers are supported.
* Convenient, fully documented and test covered API.


Requirements
------------

* PHP 5.3.3 (or later);
* [BCMath (Binary Calculator Arbitrary Precision Mathematics)][BCMath] - Required only to work with arbitrary precision arithmetic operations;
* [GMP (GNU Multiple Precision)][GMP] - Recommended. Used to speed up number systems conversions and arbitrary precision arithmetic operations;


Installation
------------

The recommended way to install AzaMath is [through composer](http://getcomposer.org).
You can see [package information on Packagist][ComposerPackage].

```JSON
{
    "require": {
        "aza/math": "~1.0"
    }
}
```


Examples
--------

You can use [examples/example.php](examples/example.php) to run all examples.

#### Example #1 - Numeral systems conversions

```php
$res = NumeralSystem::convert('WIKIPEDIA', 36, 10);
echo $res . PHP_EOL; // 91730738691298

$res = NumeralSystem::convert('9173073869129891730738691298', 10, 16);
echo $res . PHP_EOL; // 1da3c9f2dd3133d4ed04bce2

$res = NumeralSystem::convertTo('9173073869129891730738691298', 62);
echo $res . PHP_EOL; // BvepB3yk4UBFhGew

$res = NumeralSystem::convertFrom('BvepB3yk4UBFhGew', 62);
echo $res . PHP_EOL; // 9173073869129891730738691298
```

#### Example #2 - Custom numeral system

```php
// Add new system with custom alphabet
// Each char must appear only once.
// It should use only one byte characters.
$alphabet = '!@#$%^&*()_+=-'; // base 14 equivalent
$system   = 'StrangeSystem';
NumeralSystem::setSystem($system, $alphabet);

$number = '9999';
$res = NumeralSystem::convertTo($number, $system);
echo $res . PHP_EOL; // $)!$

$res = NumeralSystem::convertFrom($res, $system);
echo $res . PHP_EOL; // 9999


// Full binary alphabet
$system = 'binary';
NumeralSystem::setSystem($system, $alphabet);
// Examples with it
$var = 'example';
$expected_hex = ltrim(sha1($var), '0'); // sha1 can return hex value padded with zeros
$expected_bin = sha1($var, true);       // raw sha1 hash (binary representation)
$result_hex   = NumeralSystem::convert($expected_bin, $system, 16);
$result_bin   = NumeralSystem::convert($expected_hex, 16, $system);
echo $expected_hex . PHP_EOL; // c3499c2729730a7f807efb8676a92dcb6f8a3f8f
echo $result_hex . PHP_EOL;   // c3499c2729730a7f807efb8676a92dcb6f8a3f8f
echo ($expected_bin === $result_bin) . PHP_EOL; // 1
```

#### Example #3 - Arbitrary precision arithmetic

```php
// Create new big number with the specified precision for operations - 20 (default is 100)
$number = new BigNumber('118059162071741130342591466421', 20);

// Divide number
$number->divide(12345678910);
echo $number . PHP_EOL; // 9562792207086578954.49764831288650451382

// Divide again and round with the specified precision and algorithm
// Three round algorithms a supported: HALF_UP, HALF_DOWN, CUT.
// You can use them as BigNumber::ROUND_* or PHP_ROUND_HALF_UP, PHP_ROUND_HALF_DOWN.
// Default is HALF_UP.
$number->divide(9876543210)->round(3, PHP_ROUND_HALF_DOWN);
echo $number . PHP_EOL; // 968232710.955

// Comparisions
$number = new BigNumber(10);
echo ($number->compareTo(20) < 0) . PHP_EOL; // 1
echo $number->isLessThan(20) . PHP_EOL; // 1

$number = new BigNumber(20);
echo ($number->compareTo(10) > 0) . PHP_EOL; // 1
echo $number->isGreaterThan(10) . PHP_EOL; // 1

$number = new BigNumber(20);
echo ($number->compareTo(20) === 0) . PHP_EOL; // 1
echo $number->isLessThanOrEqualTo(20) . PHP_EOL; // 1
```

#### Example #4 - Input filtration

```php
// The arguments of all functions are also filtered.
$number = new BigNumber("9,223 372`036'854,775.808000");
echo $number . PHP_EOL; // 9223372036854775.808
```

#### Example #5 - Do some operations and then convert to base62

```php
$number = new BigNumber('9223372036854775807');
$number = $number->pow(2)->convertToBase(62);
echo $number . PHP_EOL; // 1wlVYJaWMuw53lV7Cg98qn
```


Tests
-----

Tests are in the `Tests` folder and reach 100% code-coverage.
To run them, you need PHPUnit.
Example:

    $ phpunit --configuration phpunit.xml.dist

Or with coverage report:

    $ phpunit --configuration phpunit.xml.dist --coverage-html code_coverage/


Credits
-------

AzaMath is a part of [Anizoptera CMF][], written by [Amal Samally][] (amal.samally at gmail.com) and [AzaGroup][] team.<br/>
Arbitrary precision arithmetic part is partially based on [Moontoast Math Library](https://github.com/moontoast/math).


License
-------

Released under the [MIT](LICENSE.md) license.


Links
-----

* [Composer package][ComposerPackage]
* [Last build on the Travis CI][Travis]
* [Project profile on the Ohloh](https://www.ohloh.net/p/AzaMath)
* (RU) [AzaMath — Cистемы счисления (включая кастомные) + арифметика произвольной точности на PHP](http://habrahabr.ru/post/168935/)
* Other Anizoptera CMF components on the [GitHub][Anizoptera CMF] / [Packagist](https://packagist.org/packages/aza)
* (RU) [AzaGroup team blog][AzaGroup]



[BCMath]: http://php.net/bcmath
[GMP]:    http://php.net/gmp

[Anizoptera CMF]:  https://github.com/Anizoptera
[Amal Samally]:    http://azagroup.ru/about/#amal
[AzaGroup]:        http://azagroup.ru/
[ComposerPackage]: https://packagist.org/packages/aza/math
[TravisImage]:     https://secure.travis-ci.org/Anizoptera/Math.png?branch=master
[Travis]:          https://travis-ci.org/Anizoptera/Math
