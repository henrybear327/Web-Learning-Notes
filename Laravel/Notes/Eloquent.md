# Using Tinker

These commands are commands following the Laracast videos.

* run `php artisan tinker`
    * Basic manipulation
        * `$article = new App\Article;`
        * `$article->title = 'Second article';`
        * `$article->published_at = Carbon\Carbon::now()`
        * `$article->save()`
    * Basic query
        * `App\Article::all()->toArray()`
        * `$article = App\Article::find(1)`
        * `$article = App\Article::where('body', '=', 'First body')->get()`
        * `$article = App\Article::where('body', '=', 'First body')->first()`
    * Basic mass create
        * `$article = App\Article::create(['title' => 'Third', 'body' => 'third', 'published_at' => Carbon\Carbon::now()]);`
            * Add `$fillable` in the model first!
    * Basic update 1
        * `$article = App\Article::find(2);`
        * `$article->toArray()`
        * `$article->title = 'Hey';`
        * `$article->save()`
    * Basic update 2
        * `$article = App\Article::find(2);`
        * `$article->update(['body'=>'updated again']);`
