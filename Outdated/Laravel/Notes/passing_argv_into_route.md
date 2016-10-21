# Arguments passing

Views should be given what it needs!!! No DB queries in the view!!!

## sol 1

```
return view('pages.about')->with('name', $name);
```
## sol 2

```
return view('pages.about')->with([
    'first' => '<span style="color: red;">Henry</span>',
    'last' => 'Tseng 2'
]);
```

## sol 3

```
$data = [];
$data['first'] = '<span style="color: red;">Henry</span>';
$data['last'] = 'Tseng 3';
return view('pages.about', $data);
```

## sol 4

```
$first = '<span style="color: red;">Henry</span>';
$last = 'Tseng 4';
return view('pages.about', compact('first', 'last'));
```
