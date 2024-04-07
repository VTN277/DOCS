## Built-in types
- Numbers (int, double)
- Strings(String)
- Boolean(bool)
- Records((value 1, value2))
- Lists (List, a.k.a arrays)
- Sets (Set)
- Maps (Map)
- Runes (Runes; often replaceed by the characters API)
- Symbols (Symbol)
- The value of null (Null)
#### 1. Numbers
#### 2. Strings
- multi-line string
```
  var s1 = '''
  You can create
  multi-line strings like this one.
  ''';
```
- raw string
```
  var s = r'In a raw string, not even \n gets special treatment.';
```
#### 3. Booleans
- Dart's type safety means that you can't use code like `if (nonbooleanvalue)` or `assert(nonbooeleanvalue)`
```
  // Check for an empty string.
  var fullName = '';
  assert(fullName.isEmpty);

  // Check for zero.
  var hitPoints = 0;
  assert(hitPoints <= 0);

  // Check for null.
  var unicorn = null;
  assert(unicorn == null);

  // Check for NaN.
  var iMeantToDoThis = 0 / 0;
  assert(iMeantToDoThis.isNaN);
```
#### 4. Runes and grapheme clusters
#### 5. Symbols
___

## Records
- similar to struct
#### 1. Record syntax
- Key characteristics of Records:
  + Anonymous: Unlike class that have a predefinded name, records are defined line without specific name.
  + Immutable: Once created, the value within a record can not be changed
  + Aggregate type: (nhieu loai tong hop)
- *Records expression*
```
  var record = ('first', a: 2, b: true, 'last');
```
- *Records type annotaion*
```
  // Record type annotation in a variable declaration:
  (String, int) record;

  // Initialize it with a record expression:
  record = ('A string', 123);
```
```
  // Record type annotation in a variable declaration:
  ({int a, bool b}) record;

  // Initialize it with a record expression:
  record = (a: 123, b: true);
```
```
  (int a, int b) recordAB = (1, 2);
  (int x, int y) recordXY = (3, 4);

  recordAB = recordXY; // OK.
```
### 2. Record fields
```
  var record = ('first', a: 2, b: true, 'last');

  print(record.$1); // Prints 'first'
  print(record.a); // Prints 2
  print(record.b); // Prints true
  print(record.$2); // Prints 'last'
```
### 3. Record types
### 4. Record equality
- two records are equal if they have the same *shape* (set of fiends), their corresponding fields have the same values
```
  (int x, int y, int z) point = (1, 2, 3);
  (int r, int g, int b) color = (1, 2, 3);

  print(point == color); // Prints 'true'
```
```
  ({int x, int y, int z}) point = (x: 1, y: 2, z: 3);
  ({int r, int g, int b}) color = (r: 1, g: 2, b: 3);

  print(point == color); // Prints 'false'. Lint: Equals on unrelated types.
```
### 5. Multiple returns
___

## Collections
#### 1. Lists
```
var list = [1, 2, 3]
assert(list.length == 3)
assert(list[1] == 2)
```
- create a list that's a compile-time constant
```
var constantList = const[1, 2, 3];
//constantList[1] = 1; // this line will cause error
```
#### 2. Set
- is an unorder collection with unique items
```
var names = <String>{};
//Set<string> name {} //This work too
//var name = {}; //create a map not a set
```
```
var elements = <String>{};
elements.add('fluorine');
elements.addAll(halogens);
assert(elements.length == 5);
```
- To create a set that's a compile-time constant
```
final constantSet = const {
  'fluorine',
  'chlorine',
  'bromine',
  'iodine',
  'astatine',
};
// constantSet.add('helium'); // This line will cause an error.
```
#### 3. Maps
```
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};
```
```
var gifts = Map<String, String>();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';
```
```
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // Add a key-value pair
```
#### 4.Operators
##### 4.1 Spread operator
```
var list = [1, 2, 3];
var list2 = [0, ...list];
assert(list2.length = 5);
```
```
var list2 = [0, ...?list];
assert(list2.length == 1);
```
##### 4.2 Control-flow operators (toan tu dieu khien luong)
- collection if
```
var nav = ['Home', 'Furniture', 'Plants', if (promoActive) 'Outlet'];
```
```
var nav = ['Home', 'Furniture', 'Plants', if (login case 'Manager') 'Inventory'];
```
- collection for
```
var listOfInts = [1, 2, 3];
var listOfStrings = ['#0', for (var i in listOfInts) '#$i'];
assert(listOfStrings[1] == '#1');

```
