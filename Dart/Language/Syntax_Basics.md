### Variables
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
### 3. Late variables
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
### 4. Final and const
