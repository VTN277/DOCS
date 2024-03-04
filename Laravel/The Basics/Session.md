### 1. Cấu hình
- Trong file `config/session.php.`
- Một số sesion driver:
  - `file` (mặc định) - lưu trữ tại  `storage/framework/sessions`
  - `cookie` - mã hóa trước khi lưu vào cookie
  - `database` - lưu trữ vào databse
  - `mencached/redis` - lưu trữ vào trong bộ nhớ cache của ứng dụng
  - `dynamodb` - session lưu trữ trong AWS dynamodb
  - `array` - session lưu trữ trong một array PHP
- Các thông số trong `config/session.php`
  - `lifetime` - thời gian sống của session
  - `expire_on_close` session bị xóa khi đóng trình duyệt
  - `files` path lưu trữ session nếu session drive là `file`
### 2. Tương tác với session
#### Lấy dữ liệu
- thông qua session helper: `use Illuminate\Support\Facades\Session;`
- thông qua request íntance: `$request->session()->get('key')`
- lấy all session: `Session::all()`
#### Kiểm tra session
- `$request->session()->has('users')` - session tồn tại và != null
- `$request->session()->exists('users')` - sesion tồn tại
#### Lưu trữ session
- `session()->put($key, $value);`
```
$sessions = Session::put("cart.products", [['id' => 1, 'item' => 1]]);
```
- lấy session data và xóa luôn key khỏi session
```
$sessions = Session::pull("cart.products");
```
- tăng giảm giá trị session
```
$request->session()->increment('count'); 
$request->session()->increment('count', $incrementBy = 2);
$request->session()->decrement('count');
$request->session()->decrement('count', $decrementBy = 2);
```
#### Flash session
- session tồn tại trong một request, rồi bị xóa (dùng validate, flash notify)
```
$sessions = Session::flash($key, $value);
```
#### Xóa dữ liệu
```
// Forget a single key...
$request->session()->forget('name');

// Forget multiple keys...
$request->session()->forget(['name', 'status']);
```
#### khởi tạo lại session ID
```
$request->session()->regenerate();
```
 
