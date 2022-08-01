# Readme

## Important concepts

- Everything is an object, even numbers and functions
- Dart is strongly typed but type annotations are optional (e.g. initialize a number with 'var') as Dart can infer types
- Null safety: variables cannot contain ```null``` unless explicitly said they can. A variable can be made nullable by putting a ? at the end of the type. For example, ```int?``` might be an integer or ```null``` (like an optional type in other languages). Similarly, ! can be used to force an unwrap of a ? type
- Dart has generic types like List<int>, List<Object>
- No public, private, protected attributes. Instead, _ marks it as private

## Variables

There are different ways to initialize a variable, for example a string:

```
var name01 = "Bob";
Object name02 = "Bob";
String name03 = "Bob";
```

Notice that Dart has automatic type inference but no dynamic typing. This means that the following statement is **not** possible and will throw an error during compilation:

```
var a = 12;
a = "Test";
```

Uninitialized variables always have a value of ```null```.

There is big difference between the keywords ```const``` and ```final```. A ```const``` variable is a compile-time constant, i.e. only values that are known at compile time can be ```const```. Something like

```
const a = 12;
```

is possible whereas something like

```
const DateTime now = new DateTime.now();
```

is not possible and will throw an error at compile time. Secondly, if there is a ```const``` inside a class than the whole class has to be declared as ```static const``` instead of just ```const```. Fields in a ```final``` object can be reassigned which is not possible with a ```const``` object. Therefore, this is possible:

```
final a = [1, 2, 3];
a[0] = 4;
```

whereas this is not possible:

```
const a = [1, 2, 3];
a = 4;
```

The ```late``` keyword can be used to lazily initialize a variable. Example:

```
late String temperature = readThermometer();
```
Maybe the function ```readThermometer()``` is very expensive, i.e. takes a long time to execute. If the variable temperature is never used it is never initialized and therefore the function is never called.

## Built-in types

There are two different types of numeric types: ```int``` and ```double```. Also, a variable can be declared to be of type ```num```. In this case, it can hold both ```double``` and ```int``` values:

```
num x = 1;
x += 2.5;
```

There is something similar to Python's f-Strings in Dart. Inside a string ```${<expression>}``` can be used to evaluate an expression. If the expression is just a variable the curly brackets are **not** required.

There is also something similar to list comprehension like in Python. Example: create a list of squares of the numbers from 0 to 10.

```
import 'dart:math';

var listOfStrings = [for (var i = 0; i <= 10; i++) pow(i, 2)];
```

There exists also something equivalent to ```set``` in Python:

```
var halogens = {'fluorine', 'chlorine', 'bromine', 'iodine', 'astatine'};
```

If an empty set should be created its type has to be explicily defined:

```
var names = <String>{};
```
or
```
Set<String> names = {};
```

Dictionaries here basically work exactly the same as in Python, they are just called ```Map``` here.

```
var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
}
```

or

```
var nobleGases = Map<int, String>();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
```

## Functions
Dart is a true (lol) object-oriented language. This means that even functions are objects. They can be assigned to variables or passed as argument to functions.

Example of a function:

```
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

the data type in the parameter list is just a type hint and therefore not required (although strongly recommanded!):

```
bool isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
```

If a function consists just of a single expression it can be shortened:

```
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
```

A function takes any number of *required positional* parameters followed by either *named* parameters or *optional positional* parameters (but not both!).

Named parameters have to be given in the format ```{param1, param2, ...}```. Example:

```
void enableFlag({bool? bold, bool? hidden}) {...}
```
This function than can be called like this:

```
enableFLags(bold: true, hidden: false);
```

Similarly, optional positional parameters can be passed by wapping them in ```[]```. 
