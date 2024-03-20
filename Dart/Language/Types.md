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
