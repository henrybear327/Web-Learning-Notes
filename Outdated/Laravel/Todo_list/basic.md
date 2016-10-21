# [Basic Todo List](https://laravel.com/docs/5.2/quickstart)

## Database Migrations

Use the `make:migration` command to generate a new database migration for our tasks table:

`php artisan make:migration create_tasks_table --create=tasks`

## Eloquent Models

Eloquent is Laravel's default ORM (object-relational mapper). Eloquent makes it painless to retrieve and store data in your database using clearly defined "models". Usually, __each__ Eloquent model corresponds directly with a __single__ database __table__.

So, let's define a Task model that corresponds to our tasks database table we just created. Again, we can use an Artisan command to generate this model. In this case, we'll use the make:model command:

`php artisan make:model Task`

The model will be placed in the app directory of your application. By default, the model class is empty. We do not have to explicitly tell the Eloquent model which table it corresponds to because it will assume the database table is the __plural form__ of the model name. So, in this case, the Task model is assumed to correspond with the tasks database table. Here is what our empty model should look like:
```
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Task extends Model
{
    //
}
```

## Notes so far

* Create migration first, because it setup the tables needed.
* Create models, for future table manipulations. 
