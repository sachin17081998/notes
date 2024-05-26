# ** Abstraction in Dart**

Abstraction in OOPS is used to hide unnecessary information and display only necessary information to the users interacting. It is essential to represent real-world objects in a simplified manner for users to interact easily.

In dart we can achive abstarction using
- Abstract Method
- Abstract Classes
- Interface

## Abstact Method

A method which does not have any body is called as abastract method


## Abstract Class
Abstract class is class that cannot be instantiated directly but serves as a blueprint or contract for other classes.It is primarily used as a base class to define a common behavior that its subclasses should implement.

An abstract class can be created using **abstract** keyword. Abstract classes can have both abostract and concrete methods.

Syntax:
```dart
abstract class class_name{

}
``` 

## Interface

An interface is a combination of abstract classes and methods that defines a contract which has to be fullfilled by any other class that implements the interface.


Example:
```dart
// Abstract class
abstract class AbstractClass {
  // Abstract method without a body
  void abstractMethod();
  
  // Non-abstract method
  void normalMethod() {
    print('I am a Normal Method');
  }
}

// Normal class
class NormalClass {
  // Method implementation
}

// Subclass inheriting from an abstract class
class SubClass extends AbstractClass {
  // Implementing the abstract method
  @override
  void abstractMethod() {
    print('I am a Abstract Method, implemented inside a sub-class');
  }
}

void main() {
  // Creating objects of normal classes
  NormalClass normalObj = NormalClass();
  
  // Creating objects of subclasses
  SubClass subObj = SubClass();
  
  // Instantiating normal class object
  normalObj.normalMethod();
  
  // Calling abstract method through subclass object
  subObj.abstractMethod();
  subObj.normalMethod();
  
  // Creating objects of abstract class (not allowed)
  // AbstractClass abstractObj = AbstractClass(); // Error: Cannot instantiate abstract class
}
```

Interafce can be crated using 2 ways in dart.

1. Implicit Interface: Example shown above is an explicit interface where we are using explicityly the abstract keyword to crate an interface.
2. Implicit Interface: When you define a class in dart then dart automatically treats as an implicit interface and every public method of class act as abstarct method.

>${\textsf{\color{red}Note:}}$ Every class in dart is an Interface by default. A class can implement multiple interaface but can extend only one class.


Example:
```dart
class Animal {
  final String name;

  Animal({required this.name});

  void whatAmI() => print('I\'m $name');

  void chase(Animal e) => print('$e is getting chased');
}

class Bird implements Animal{
  @override
  void chase(Animal e) {
    // TODO: implement chase
  }

  @override
  // TODO: implement name
  String get name => throw UnimplementedError();

  @override
  void whatAmI() {
    // TODO: implement whatAmI
  }
  
}
```

