- `$request->path();` => path
- `$request->url();` => url (not query string)
- `$request->fullUrl();` => fullUrl
- `request->is('admin/*')` check path
- `$request->all();`
- `$request->input('name');`
- `$request->input('name', 'Lò Thị Vi Sóng');` if null => default
- `$names = $request->input('products.*.name');` lay name cua all phan tu mang products
- `$request->boolean('archived');` kiem tra input archieved la true (1, "1", true, "true", "on", "yes") or false
- `$request->only(['username', 'password']);`
- ` $request->except(['credit_card']);`
- `$request->query();` => query string
- `$request->query('name');` => gia tri query stirng cu the
- `$request->query('name', 'Lò Thị Vi Sóng')` if null => default
- `$request->has('name')`
- `$request->has(['name', 'email'])`
- `$request->hasAny(['name', 'email']))` === `$request->has('name') || $request->has('email'`
    ```
    $request->whenHas('name', function ($input) {
        //
    });
    ```
- `$request->filled('name')` return true if name exist and filled 
    ```
    $request->whenFilled('name', function ($input) {
        //
    });
    ```
### Make flash session    
- `$request->flash();`
- `$request->flashOnly(['username', 'email']);`
- `$request->flashExcept('password');`