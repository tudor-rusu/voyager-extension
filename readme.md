# Voyager Extension

[![Latest Version on Packagist][ico-version]][link-packagist]
[![Total Downloads][ico-downloads]][link-downloads]
[![Build Status][ico-travis]][link-travis]

The package extends the original [Voyager Admin Panel](https://github.com/the-control-group/voyager) with some new advantages and features.

## Features

- Integration of [laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/) by Spatie
- New field: Advanced ML Image, supports Title and Alt field. 
- New field: Advanced ML Files (including images), supports Sorting and unlimited attached custom fields with different types.
- New field: Select Dropdown Tree. Dropdown selection for Tree type structures (with parent_id).
- New field: Fields Group. JSON kind group of fields inside the one model field.
- New field: Page Layout. Allows to organize layout of widgets and content on a Page. Depends on Voyager Site package. 
- New extended Browse Bread appearance and options.
- Tree view mode mode for models have parent_id field

## Package installation

Via Composer

``` bash
$ composer require monstrex/voyager-extension
```

Publish config if you need:
```
$ php artisan vendor:publish --provider="MonstreX\VoyagerExtension\VoyagerExtensionServiceProvider" --tag="config"
```

To use Image fields you need publish and migrate [laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/) resources

``` bash
$ php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="migrations"
$ php artisan migrate
```

Optional you may would like to publish config [laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/) as well
``` bash
$ php artisan vendor:publish --provider="Spatie\MediaLibrary\MediaLibraryServiceProvider" --tag="config"
```

Configure
---

#### Config file

```php
/*
| Use original edit-add.blade.php or use extended one
*/
'legacy_edit_add_bread' => false,

/*
| CLone Record parameters
| @params: enabled - if action is available
|          reset_types - A value of these bread type fields will be cleared
|          suffix_fields - The suffix '(clone)' will be added to these fields content
*/
'clone_record' => [
    'enabled' => true,
    'reset_types' => ['image', 'multiple_images','file'],
    'suffix_fields' => ['title','name','slug'],
],
/*
| You can enable or disable the custom path generator for medialibrary images
| at MonstreX\VoyagerExtension\Generators\MediaLibraryPathGenerator
*/
'use_media_path_generator' => true,

```


#### Models

To use additional images fields you should to configure your models like this:

```php
namespace App;

use Illuminate\Database\Eloquent\Model;
use Spatie\MediaLibrary\HasMedia\HasMedia;
use Spatie\MediaLibrary\HasMedia\HasMediaTrait;

class Article extends Model implements HasMedia
{
   use HasMediaTrait;    
}

```
Also you can use any other advantages provided by [laravel-medialibrary](https://docs.spatie.be/laravel-medialibrary/) package.

Usage
---

The package provide some new type fields.

>### Field: Advanced ML Image

The field utilize **laravel-medialibrary** package to store single image. In addition this field can hold text attributes TITLE and ALT.
  
>### Field: Advanced ML Media Files

This field represents **laravel-medialibrary** collection with subsets of additional custom fields. Uses to store any media files. 
The collection can be sorted as you need using drag and drop. Select and group removing is implemented. 
By default it keeps two fields - **Title** and **Alt**. Changing a file inside a collection element is allowed. 
You can use the field like a collection of widgets or just like a sortable image collection. 
Elements of media collection can hold additional content fields using **BREAD Json Options**.

>Implemented fields types:
```json
{
    "extra_fields": {
        "content": {
            "type": "textarea",
            "title": "Description",
        },
        "code": {
            "type": "codemirror",
            "title": "HTML Widget"
        },
        "link": {
            "type": "text",
            "title": "URL"
        }        
    }
}
```
>Accepted files types template:
```json
{
  "input_accept": "image/*,.pdf,.zip,.js,.html,.doc,.xsxl"
}
```
By default uses "image/*" template.

> Retrieving field data on frontend side.
 
You can use any method provided by laravel-medialibrary package. The field name of is represented media gallery name.

```php
$image = Post->getFirstMedia('field_name');
$imageUrl = $image->getFullUrl();
$imageTitle = $image->getCustomProperty('title');
$imageAlt = $image->getCustomProperty('alt');
``` 
More details see in the original [laravel-medialibrary documentation](https://docs.spatie.be/laravel-medialibrary/v7/basic-usage/retrieving-media/).

>### Field: Advanced Fields Group Field

Is a simple JSON like fieldset. Support three field subtypes inside. 
Useful when you need implement the same group fields in different models.
BREAD Json Options:
```json
{
    "fields": {
        "seo_title": {
            "label": "SEO Title",
            "type": "text"
        },
        "meta_description": {
            "label": "META Description",
            "type": "text"
        },
        "meta_keywords": {
            "label": "META Keywords",
            "type": "text"
        }
    }
}
```   
Retrieving:
```blade
@if($seo = json_decode($Post->seo->fields))
  <title>{{ $seo->seo_title->value }}</title>
@endif
```


Localizations
---

New types of fields don't provide localization service used in Voyager. 
But you can use built-in localization helper and retrieve translated substring from a field content:

```php
$field_data = '{{en}}English title {{ru}}Russian title';
...
$field_title_en = str_trans($field_data);
$field_title_ru = str_trans($field_data,'ru');
``` 


To be described.

## Contributing

Please see [contributing.md](contributing.md) for details and a todolist.

## Security

If you discover any security related issues, please email author email instead of using the issue tracker.

## Credits

- [author name][link-author]
- [All Contributors][link-contributors]

## License

license. Please see the [license file](license.md) for more information.

[ico-version]: https://img.shields.io/packagist/v/monstrex/testpackage.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/monstrex/testpackage.svg?style=flat-square
[ico-travis]: https://img.shields.io/travis/monstrex/testpackage/master.svg?style=flat-square
[ico-styleci]: https://styleci.io/repos/12345678/shield

[link-packagist]: https://packagist.org/packages/monstrex/testpackage
[link-downloads]: https://packagist.org/packages/monstrex/testpackage
[link-travis]: https://travis-ci.org/monstrex/testpackage
[link-styleci]: https://styleci.io/repos/12345678
[link-author]: https://github.com/monstrex
[link-contributors]: ../../contributors
