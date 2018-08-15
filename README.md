# Eloquent Cache

> Easily cache your Laravel's Eloquent models.

## Requirements

- PHP 7.2

- Laravel 5.6

## Installation

Install via [composer](https://getcomposer.org/) :

`composer require authentik/eloquent-cache`

## How it works

- When eloquent fetches models by ID (without eager-loading), the JSON representations of the model instances are cached.

- Subsequently, when eloquent fetches models by ID, the cached JSON representation will be converted into an instance;

## Usage

Use the `Cacheable` trait in the models you want to cache.

You can optionally define another TTL or tag name.

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Authentik\EloquentCache\Cacheable;

class Category extends Model
{
    use Cacheable;


    /*
     * You can optionally override the following functions:
     */
    
    // Time To Live in minutes (default value: 0 => no TTL)
    public function getCacheTTL() {
        return 60;
    }

    // default value: the lowercase name of the model
    public function getCacheTagName() {
        return 'cat';
    }

    // Cache busting will automatically invalidate the cache when model instances are updated or deleted.
    // default value: true
    public function isCacheBustingEnabled() {
        return false;
    }
}
```

- To manually cache a model instance, use the `cache` method.

```php
Category::find(1)->cache();
```

- To invalidate the cache for a model instance, use the `refresh` method.

```php
Category::find([1, 2, 3])->each->refresh();
```

- To invalidate the cache for all instances of a model, use the `flush` method.

```php
Category::flush();
```