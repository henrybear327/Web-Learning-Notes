# [laravelcollective/html](https://laravelcollective.com/docs/5.2/html#installation) package

## Notes

Don't use `illuminate/html` for Laravel 5.2, the [`bindshared()` problem](http://stackoverflow.com/questions/34414389/fatalerrorexception-in-htmlserviceprovider-php-line-36-laravel) hasn't been solved at the time of writing(1/19/16).

## Installation

* Install using composer
    * `composer require laravelcollective/html`
* Add ServiceProvider
    * Add `Collective\Html\HtmlServiceProvider::class,` to `providers` array of `config/app.php`
* Add Facade
    * Add
    ```
    'Form' => Collective\Html\FormFacade::class,
    'Html' => Collective\Html\HtmlFacade::class,
    ```
    to `aliases` array of `config/app.php`

* Creating form

# [Form](https://laravelcollective.com/docs/5.2/html#installation)

## Open

```
{!! Form::open(array('url' => 'foo/bar')) !!}
    //
{!! Form::close() !!}
```

The other ways of specifying the url are
```
echo Form::open(array('route' => 'route.name'))
echo Form::open(array('action' => 'Controller@method'))
```

Passing in parameters
```
echo Form::open(array('route' => array('route.name', $user->id)))
echo Form::open(array('action' => array('Controller@method', $user->id)))
```

For file upload
```
echo Form::open(array('url' => 'foo/bar', 'files' => true))
```

By default, a POST method will be assumed; however, you are free to specify another method: `echo Form::open(array('url' => 'foo/bar', 'method' => 'put'))`

Since HTML forms only support `POST` and `GET`, `PUT` and `DELETE` methods will be spoofed by automatically adding a `_method` hidden field to your form.

## Example

```
{!! Form::open() !!}
    <div class="form-group">
        {!! Form::label('title', 'Title:') !!}
        {!! Form::text('title', null, ['class' => 'form-control']) !!}
    </div>

    <div class="form-group">
        {!! Form::label('body', 'Body:') !!}
        {!! Form::textarea('body', null, ['class' => 'form-control']) !!}
    </div>

    <div class="form-group">
        {!! Form::submit('Add Article', ['class' => 'form-control']) !!}
    </div>
{!! Form::close() !!}
```

## CSRF

If you use the `Form::open` method with `POST`, `PUT` or `DELETE` the CSRF token will be added to your forms as a hidden field automatically.

## Error

* `MethodNotAllowedHttpException`
    * Double check the route!!!!
