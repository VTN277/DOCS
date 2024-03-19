## Variables
```
  var name = 'Bob'; 
  //tyer of the name variable inferred to a String object
  String name = 'Bob'
```
#### 1. Null safety
```
  String? name  // Nullable type. Can be `null` or string.

  String name   // Non-nullable type. Cannot be `null` but can be string.
```
#### 2. Default value
```
  int? lineCount;
  assert(lineCount == null);
```
- Khong can gan gia tri cho bien khi khoi tao nhung phai dam bao bien phai co value khi duoc su dung
```
  int lineCount;

  if (weLikeToCount) {
    lineCount = countLines();
  } else {
    lineCount = 0;
  }
  //lineCount luc nay co gia tri
  print(lineCount);
```
#### 3. Late variables
- Useful in these cases:
  + The value of the variable is not available at the time of declaration
  + Delay variable initialization until the variable is used to optimize performance
- Common use cases:
  + Top-level variable, declared outside the class or function  
```
  class User {
    late String name; // late variable

    void setName(String newName) {
      name = newName; // Initialization happens here
    }

    String greet() {
      return "Xin chào, $name!"; // Access late variable after initialization
    }
  }

  void main() {
    var user = User();
    user.setName("Alice");
    print(user.greet()); // print: Xin chào, Alice!
  }
```  
#### 4. Final and const
- the same: decrare constant
- **const**: it's value assigned at compile time
- **final**: it's value assigned at runtime
___
___

## Operators
#### 1. Arithmetic
| Operator          | Meaning                          |
| :---------------- | :------------------------------: |
| -expr             | reverse the sign of expresstion  |
| ~/                | devide, returning integer resutl |
#### 2.Type test operators
| Operator          | Meaning                          |
| :---------------- | :------------------------------: |
| is                | True if the object have the specified type        |
| is!               | True if the object doesn't have the specifed type |
```
(employee as Person).firstName = 'Bob';
```
```
if (employee is Person) {
  // Type check
  employee.firstName = 'Bob';
}
```
#### 3. Assignment operators
```
// Assign value to a
a = value;
// Assign value to b if b is null; otherwise, b stays the same
b ??= value;
```
#### 4. Conditional expressions
```
String playerName(String? name) => name ?? 'Guest';
```
#### 5. Cascade notation (.., ?..)
```
  var paint = Paint()
    ..color = Colors.black
    ..strokeCap = StrokeCap.round
    ..strokeWidth = 5.0;
```
- This previous example is equivalent to this code
```
  var paint = Paint();
  paint.color = Colors.black;
  paint.strokeCap = StrokeCap.round;
  paint.strokeWidth = 5.0;
```
- null-coalescing with `?..`
```
  var button = querySelector('#confirm');
  button?.text = 'Confirm';
  button?.classes.add('important');
  button?.onClick.listen((e) => window.alert('Confirmed!'));
  button?.scrollIntoView();
```
#### 6. Spread operators