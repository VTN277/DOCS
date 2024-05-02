## Tạo form request
```
php artisan make:request <tên class bạn muốn tạo>
```
### authorize():
xác định người dùng nào có quyền thực hiện request này
### rules():
```
public function rules(): array
{
	return [
		'password' => 'required',
	];
}
```
### attributes()
custom name default của field
```
public function attributes()
{
	return [
		'password' => 'Your Custom Name',
		// Add more custom attribute names as needed
	];
}

```
### message():
config message server trả về
```
public function messages(): array
{
	return [
		'password.required' => __('messages.FRE00001'),
	];
}
```
## Triển khai
- Request.php
```
class Request extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     */
    public function authorize(): bool
    {
        return true;
    }

    protected function failedValidation(Validator $validator)
    {
        $errors = (new ValidationException($validator))->errors();
        throw new HttpResponseException(response()->json([
            'errors' => $errors,
        ], Response::HTTP_OK));
    }
}
```
- CreateNewUserRequest.php
```
class CreateNewUserRequest extends Request
{
    public function rules(): array
    {
        return [
            'password' => 'required',
        ];
    }

    public function attributes()
    {
        return [
            'password' => 'Your Custom Name',
            // Add more custom attribute names as needed
        ];
    }

    public function messages(): array
    {
        return [
            'password.required' => __('messages.FRE00001'),
        ];
    }
}
```
- messages.php
```
<?php

return [
    'MSG00001' => '該当の検索結果が見つかりません。',
	'FRE00001' => ':attribute を入力して下さい。',
];

```