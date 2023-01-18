# Type Safety
**Dart is type safe language** In dart all data types define a set of operations and you can perform only those operatiions on the data. for 
examples
 > In case of boolean you can perform operation like assignment and comparison but you can not add two boolean variables.

 > In case of you have an integer variable then you can not perform operations like toUpper or toLower on those variables.

Dart uses **Static type check** and **run time check** to ensure type safety in a code.

> Because of these two tests dart is considered as a **Sound Type System**

## **Sound Type System**
so whenever you right any code, before you run the code then dart perfrom compile time type check(Static type check) and if it founds any  error then it will throw an error. 
You can skip this check by making a variable of type **dynamic**
> dynamic age=20

Now if you run your code then the dart compiler wont throw any error. Sound Great.

Suppose later on you call toUpper() on age varaiable at this point dart run time check will kick in and throw an exception.

So with the combination of these two check dart provide sound type system.

**Here one must come inyour mind. How dart is able to determine which operations are allowed on a dynamic data at run time.**
> Answer is : **Type Inference**

## **Type Inference**
Dart is intelligen enough to determine the type of a variable based on the data.

you can create a variable using key word **var** and dart will automaticall infer its type based on value.

>**Note: Var**  is not a data type it is just a keyword that tells dart static type checker to determine the type of variable base on its value. Wheras **Dynamic** is a data type.

see below example for better understanding.**Dynamic**
```dart
void main() {
  dynamic data = 2;
  print(data.runtimeType);  //output : int
  data = 'name';
  print(data.runtimeType); //output : String
}
```

**Var**
```dart
void main() {
  var data = 2;
  print(data.runtimeType);  //output : int
  data = 'name'; //throw error and wont even let you run
  
}
```
Sor from above two code its clear the basic diffrence between Dynamic and var. 



