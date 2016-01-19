# Laravel-IDE-helper

Genius! Very convenient to use!

# [Installation](https://github.com/barryvdh/laravel-ide-helper)

* `composer require barryvdh/laravel-ide-helper`
* After updating composer, add the service provider to the `providers` array in `config/app.php`: `Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,`

# [Usage](https://laracasts.com/series/how-to-be-awesome-in-phpstorm/episodes/15)

Run the oneliner.

```
php artisan clear-compiled && php artisan ide-helper:generate && php artisan optimize
```

Or add
```
"scripts":{
    "post-update-cmd": [
        "php artisan clear-compiled",
        "php artisan ide-helper:generate",
        "php artisan optimize"
    ]
},
```
in `composer.json` to trigger these commands after each commit.
