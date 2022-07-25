# Readme

## Important concepts

- Everything is an object, even numbers and functions
- Dart is strongly typed but type annotations are optional (e.g. initialize a number with 'var') as Dart can infer types
- Null safety: variables cannot contain ```null``` unless explicitly said they can. A variable can be made nullable by putting a ? at the end of the type. For example, ```int?``` might be an integer or ```null``` (like an optional type in other languages). Similarly, ! can be used to force an unwrap of a ? type
- Dart has generic types like List<int>, List<Object>
- No public, private, protected attributes. Instead, _ marks it as private

## ```late``` keyword

The ```late``` keyword can be used to lazily initialize a variable. Example:

```
late String temperature = readThermometer();
```
Maybe the function ```readThermometer()``` is very expensive, i.e. takes a long time to execute. If the variable temperature is never used it is never initialized and therefore the function is never called. 

## Datatypes

Dart has the types ```int``` and ```double```. A variable can also be declared as ```num```. In this case it can hold both integers and doubles. 
