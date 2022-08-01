# Readme

A walkthrough of https://dart.dev/guides/language/language-tour

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

If a function consists just of a single expression it can be shortened (also called **arrow notation**):

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

Optional parameters can be given default values. For example the function from above would look like this:

```
void enableFlags({bool bold = false, bool hidden = false}) {...}
```

Every Dart program must have a ```main()``` function as an entry point. It can not return anything. Additionally, it can (but does not have to!) take a list of Strings (could be arguments for example if started as a CLI program).

As stated above, functions can be assigned to variables:

```
var loudify = (msg) => '${msg.toUpperCase()}';
```

Note, that here an anonymous function (also called lambda function) is used.
Another example of a lambda function:

```
const list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item')
});
```

If the anonymous function consists of just a single function the arrow notation can also be used here:

```
list.forEach((item) => print('${list.indexOf(item)}: $item'))
```

All functions have a return value! If not otherwise specified they return null.

## Operators

In Dart, variables can be incremented with either ```++var``` or ```var++```. The important difference here is that ```var++``` returns ```var``` whereas ```++var``` returns ```var + 1```.
The same holds for ```var--``` and ```--var```.

The operator ```as``` is used for typecasting (and library import, just like Python does), ```is``` is used to test if an object is of the specified type. On the contrary, ```is!``` tests if the object is **not** of the specified type (in Python this would be ```is not```).

The operator ```??=``` only assigns a value to the variable if it is ```null```, otherwise the variable remains unchanged.

Important: ```&``` and ```|``` are bit-wise operators, whereas ```&&``` and ```||``` are logical operators.

There is also the ternary operator that many other languages have:

```
condition ? expre01 : expre02
```

There is also another operator, defined as

```
expr01 ?? expre02
```

If expr01 is non-null its value is returned, otherwise expr02 is returned. One example for this is

```
String playerName(String? name) => name ?? 'Guest';
```

Another useful opeator is for the cascade notation: ```..``` and ```?..```. It allows to perform multiple actions on the same object. So, instead of writing

```
var paint = Paint();
paint.color = Colors.black;
paint.strokeCap = strokeCap.round;
paint.strokeWidth = 5.0;
```

this operator makes it possible to write

```
var paint = Paint()
  ..color = Colors.black
  ..strokeCap = StrokeCap.round
  ..strokeWidth = 5.0
```

Possibly, the object the cascade is performed on can be ```null```. To ensure that the cascade does not completely fail, the ```?..``` can be used. Important: this operator has just to be used for the **first** operator! Example:

```
querySelector('#confirm')
  ?..text = 'Confirm'
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'))
  ..scrollIntoView();
```

this code is equivalent to

```
var button = querySelector('#confirm');
button?.text = 'Confirm';
button?.classes.add('important');
button?.onClick.listen((e) => window.alert('Confirmed!'));
button?.scrollIntoView();
```

Note: cascades can also be nested!

Just two more operators:

 The first one is ```?[]```. Just like ```[]``` but the leftmost operator can be ```null```. Example: ```fooList?[1]```. This evaluates to ```null```

 Second, ```!``` can be used to force a typecast into a non-nullable type. Dangerous: if the type-cast fails a runtime exception is called.


## Control flow statements

If and else:
```
if (...) {

}else if (){

}else{

}
```

For loops:
```
for (var i = 0; i < 5; i++){
  ...
}
```

Not required, but very interesting example from the Dart tour:

```
var callbacks = [];
for (var i = 0; i < 2; i++) {
  // Very interesting construct: anonymous functions are
  // added to the callbacks list. The bracket () is empty
  // which means that the anonymous function does not take
  // any arguments. It consists of just a single line, that // is why the arow notation us used. After this for loop
  // the callbacks lists consists of print() functions
  // with a certain value i
  callbacks.add(() => print(i))
}

// Iterate through all of the elements in the callback list
// and again apply anonymous functions to each element.
// Every c is a print function, so c() is just a call of
// the print function with its i value.
callbacks.forEach((c) => c());
```

Convenience function: if the iteration is over an Iterable (such as List, Set, etc.) a for-each iteration (similar to Java) is possible:

```
for (final candidate in candidates) {
  candidate.interview();
}
```

Another very elegant solution from the Dart tour:

Instead of

```
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
```

(better) write

```
candidates
  .where((c) => c.yearsExperience >= 5)
  .forEach((c) => c.interview());
```

Switch and case works nearly exactly to the one in Java. Example:

```
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  default:
    executeUnknown();
}
```

Dart also supports fall-through:

```
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
  case 'NOW_CLOSED':
    executeNowClosed();
    break;
}
```

## Exception handling

An error can be easily thrown using the ```throw``` keyword, for example:

```
throw FormatException('Expected at least 1 section!')
```

Something kind of special (don't know if possible in other programming languages?) is that not only Exceptions can be thrown but basically any object, e.g.

```
throw 'Out of llamas!';
```

or even

```
throw 12;
```

Example for a (typically?) try-catch structure:

```
try {
  ...
} on OutOfLlamasException {
  ...
} on LlamaTooCuteException catch (e) {
  ...
} on Exception catch (e, s) {
  ...
} catch (e)
  ...
}
```

Note that a caught exception can have two parameters: ```e``` is the Exception itself whereas ```s``` is the stacktrace of the exception.

Another very interesting (and unique?) property of Dart is the ```rethrow``` keyword. This allows to propagate an exception to a higher level.

## Classes

In Dart, all classes except ```null``` descend from ```Object```. Furthermore, it has **mixin-based inheritance**. This means, that every class has exactly one superclass. It can implement extension methods to extend its functionality.

Very important: to create a new Object the ```new``` keyword is **purely optional**. This means that

```
var p1 = Point(2, 2);
```

has **exactly the same effect** as

```
var p2 = new Point(2, 2);
```

Some classes only have a **constant constructor**. They are created using the ```const``` keyword, for example:

```
var p = const ImmutablePoint(2, 2);
```

To get an object's type just use the ```Object``` property ```runtimeType```.

Example of declaring instance variables:

```
class Point {
  double? x;
  double? y;
  double z = 0;
}
```

All uninitialized instance variables have the value ```null```. Important: something like this

```
class Point {
  double x;
}
```

is **not** possible as the variable x has no value now. This snippet would throw an error during compilation. To fix this, either write

```
class Point {
  double? x;
}
```

or

```
class Point {
  late double x;
}
```

Non-final instance variables automatically have getter and setter methods.

The constructor of a class basically works exactly as in Java (lol). Example:

```
class Point {
  double x = 0;
  double y = 0;

  Point(double x, double y) {
    this.x = x;
    this.y = y;
  }
}
```

as everyone knows, this is a very common operation, that is why Dart has a shortcut for this. The above code can be rewritten as

```
class Point {
  double x = 0.0;
  double y = 0.0;

  Point(this.x, this.y);
}
```

**Constructors aren't inherited!**

Named constructors can be used to implement multiple constructors for a class. Extending the example from above:

```
class Point {
  double x = 0.0;
  double y = 0.0;

  Point(this.x, this.y);

  Point.funny(){
    this.x = -1.0;
    this.y = -1.0;
  }
}
```

To create an object with a named constructor call

```
var p = Point.funny();
```

Important: if the superclass doesn't have an unnamed, no-argument constructor, then you must manually call one of the constructors in the superclass. The superclass constructor must be defined after a colon before the constructor body. Example:

```
class Employee extends Person {
  Employee() : super.fromJson(fetchDefaultData());
}
```

Another important note: Arguments to the superclass constructor do not have access to ```this```. Instead, use the keyword ```super```. Example:

```
class Vector2d {
  final double x;
  final double y;

  Vector2d(this.x, this.y);
}

class Vector3d extends Vector2d {
  final double z;

  Vector3d(super.x, super.y, this.z);
}
```

There are also **redirecting constructors**. This is used if a constructor calls another constructor. Example:

```
class Point {
  double x, y;

  Point(this.x, this.y);

  Point.alongXAxis(double x) : this(x, 0);
}
```

It is also possible to define constant constructors. To do so, a const constructor has to be defined and all variables have to be made ```final```. Example:

```
class ImmutablePoint {
  final double x, y;

  const ImmutablePoint(this.x, this.y);
}
```

Finally, the ```factory``` keyword can be used when a constructor does not necessarily need to create a new instance every time but can for example retrieve one from a cache.

There are a lot of different kinds of methods.

Instance methods are methods that can access instance variables and the keyword ```this```. Example:

```
import 'dart:math'

class Point {
  final double x;
  final double y;

  Point(this.x, this.y);

  double distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx*dx + dy*dy);
  }

}
```

It is also possible to overload existing operators, like the ```+``` and ```-``` operator. Example:

```
class Vector {
  final int x,y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

}
```

This makes it now very easy to add and subtract vectors.

As already known, each instance variable has implicit getters and setters. It is possible to define new properties solely through getters and setters:

```
class Rectangle {
  double left, top, width, height;

  Recangle(this.left, this.top, this.width, this.height);

  // Define a new property 'right' through this getter
  double get right => left + width;

  set right(double value) => left = value - width;
}
```

ALso, ```abstract``` classes and methods can be defined, leaving the concrete implementation to the classes that extend this one. Example:

```
abstract class Doer {

  // Abstract method (no special keyword!)
  void doSomething();
}
```
