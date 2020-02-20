# Very short description of the package

[![Latest Version on Packagist](https://img.shields.io/packagist/v/breadthe/php-contrast.svg?style=flat-square)](https://packagist.org/packages/breadthe/php-contrast)
[![Build Status](https://img.shields.io/travis/breadthe/php-contrast/master.svg?style=flat-square)](https://travis-ci.org/breadthe/php-contrast)
[![Quality Score](https://img.shields.io/scrutinizer/g/breadthe/php-contrast.svg?style=flat-square)](https://scrutinizer-ci.com/g/breadthe/php-contrast)
[![Total Downloads](https://img.shields.io/packagist/dt/breadthe/php-contrast.svg?style=flat-square)](https://packagist.org/packages/breadthe/php-contrast)

Provides various utilities for working with color contrast.

## Installation

You can install the package via composer:

```bash
composer require breadthe/php-contrast
```

## Usage

Import the class.

```php
//use Breadthe\PhpContrast\Contrast;
use Breadthe\PhpContrast\HexColor;
use Breadthe\PhpContrast\HexColorPair;
use Breadthe\PhpContrast\TailwindColor;
```

## Check the contrast ratio between 2 colors

```php
$hexColorPair = HexColor::make(HexColor::make('000000'), HexColor::make('ffffff'));
$hexColorPair->ratio; // 21
$hexColorPair->fg; // '#000000'
$hexColorPair->bg; // '#ffffff'
```

## Get a random color pair (with the resulting ratio), with minimum 3:1 contrast ratio

```php
$hexColorPair = HexColorPair::random();
$hexColorPair->ratio; // 3.8
$hexColorPair->fg->hex; // '#36097e'
$hexColorPair->fg->name; // null
$hexColorPair->bg->hex; // '#ed4847'
$hexColorPair->bg->name; // null
```

## Get a random color pair (with the resulting ratio), with minimum specified contrast ratio (but no less than 3:1)

**⚠️ Warning** For performance reasons, the minimum requested contrast ratio is capped at 4.5, although the generated pairs can go up to the theoretical maximum 21:1 ratio.

**⚠️ Caution** When chaining with `minContrast()`, make sure to use `getRandom()` instead of `random()`.

```php
$hexColorPair = HexColorPair::minContrast(4.5)->getRandom();
$hexColorPair->ratio; // 7.6
$hexColorPair->fg->hex; // '#0c402f'
$hexColorPair->fg->name; // null
$hexColorPair->bg->hex; // '#badd73'
$hexColorPair->bg->name; // null
```

## Get a random accessible sibling for the given color, with minimum specified contrast ratio (but no less than 3:1)

```php
// Minimum 3:1 contrast ratio
HexColorPair::sibling('0000000'); // [21, '0000000', 'ffffff']

// Minimum specified contrast ratio (no less than 3:1)
HexColorPair::minContrast(4.5)->getSibling('0000000'); // [21, '0000000', 'ffffff']
```

## Generate a random TailwindCSS color

```php
$twColor = TailwindColor::random();
$twColor->hex; // '#e2e8f0'
$twColor->name; // 'gray-300'
```

## Generate a pair of random accessible TailwindCSS colors

```php
$twColorpair = TailwindColor::randomPair();
$twColorpair->ratio; // 3.3
$twColorpair->fg->hex; // '#63b3ed'
$twColorpair->fg->name; // 'blue-400'
$twColorpair->bg->hex; // '#4a5568'
$twColorpair->bg->name; // 'green-700'
```

## Generate a pair of random accessible TailwindCSS colors, with minimum specified contrast ratio (but no less than 3:1)

**⚠️ Warning** For performance reasons, the minimum requested contrast ratio is capped at 4.5, although the generated pairs can go up to the theoretical maximum 21:1 ratio.

**⚠️ Caution** When chaining with `minContrast()`, make sure to use `getRandomPair()` instead of `randomPair()`.

```php
$twColorpair = TailwindColor::minContrast(4.5)->getRandomPair();
$twColorpair->ratio; // 7.0
$twColorpair->fg->hex; // '#faf5ff'
$twColorpair->fg->name; // 'purple-100'
$twColorpair->bg->hex; // '#9b2c2c'
$twColorpair->bg->name; // 'red-800'
```

## Merge the default Tailwind colors with a custom palette

You may extend the default Tailwind colors with your own custom palette. Here's an example of how to import a custom palette from a JSON file.

```json
{
    "background": "#FFFFFF",
    "headline": "#1f1235",
    "sub-headline": "#1b1425",
    "button": "#ff6e6c",
    "button-text": "#1f1235"
}
```

```php
$customPalette = json_decode(file_get_contents('custom-palette.json'), true);

$colors = TailwindColor::merge($customPalette)->getColors();
```

### Testing

``` bash
composer test
```

### Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

### Security

If you discover any security related issues, please email omigoshdev@protonmail.com instead of using the issue tracker.

## Credits

- [Omigosh Dev](https://github.com/breadthe)
- [All Contributors](../../contributors)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.

## PHP Package Boilerplate

This package was generated using the [PHP Package Boilerplate](https://laravelpackageboilerplate.com).
