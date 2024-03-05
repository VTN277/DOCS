### Hiển thị dữ liệu
- `{{ code  }}` - `{{}}` tương tự echo + htmlspecialchars (chống XSS) => không thể print HTML
- `{!! $name !!}` hiển thị nguyên bản dữ liệu
- `@json($array, $flag)` hiển thị json ($flag là kiểu encode)
- `@if(Auth::check())` => true
- `@unless(Auth::check())` => false
- `@isset($records)` tương tự `if(isset()) ` check biến tồn tại
- `@empty($records)` tương tự `if(empty())` check biến tồn tại hoặc null
- `@auth` !== `@guest` check user login
- `@production` check là môi trường production
- `@env('staging')` check
- `@hasSection('navigation')` check template cha có tồn tại session != `@sectionMissing('navigation')`
- `@include('view.name', [$variableName => $data])` nhúng blade into blade
### Slots  
- resources/views/components/todo.blade.php.
```
<html>
  <head>
    <title>{{ $title ?? 'Todo Manager' }}</title>
  </head>
  <body>
    <h1>Todos</h1>
    <hr/>
    {{ $slot }}
  </body>
</html>
```
- resources/views/home.blade.php
```
<x-todo>
    <x-slot name="title">
        Custom Title
    </x-slot>
    @foreach ($tasks as $task)
        <h3>{{ $task['name'] }}</h3>
    @endforeach
</x-todo>
```
- routes/web.php
```
use Illuminate\Support\Facades\Route;

Route::get('/', function () {
    $tasks = [
        ['name' => 'Task 1'],
        ['name' => 'Task 2'],
        ['name' => 'Task 3'],
        ['name' => 'Task 4'],
    ];

    return view('home', ['tasks' => $tasks]);
})->name('home');
```
- Kết quả:
- ![image](https://github.com/VTN277/DOCS/assets/67737894/889f5445-45ee-49a7-bd7d-58b3b90f3cc5)
### Kế thừa layout
- resources/views/layouts/app.blade.php
```
<head>
    <title>App Name - @yield('title')</title>
</head>
<body>
  @section('sidebar')
      This is the master sidebar.
  @endsection
  <div class="container">
      @yield('content')
  </div>
</body>
```
- resources/views/home.blade.php
```
@extends('layouts.app')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
```
