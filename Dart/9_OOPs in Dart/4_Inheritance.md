# **Inheritance in Dart**

Inhertance in general is sharing of behaviour between two classes. Dart is fully object oriented language that means everthing in dart is an object and everthing in dart by default extends the Object class.

So if write something like:
class Animal{}  => class Animal extends Object{}

in dart you can inherit any other class  with the help of **extends** key word.

Example:
```dart
class Animal{
  final String name;

  Animal({required this.name});

  void whatAmI() => print('I\'m an animal!');
}

class Bird extends Animal{
  Bird(String name):super(name:name);
}
```
In above example Nird class inherits from the Animal class. Animal class has one attribue, one constructor and on function. Bird class can access all this members of Animal class.

Now whenever bird class object is created, dart will automatically create a Animal class object and since here it does not know from where the data will come for animal class so you have to manually define the constructor of Bird class using super key word so that when bird class construct is called it will automatically call the animal class constructor with respective data.

>In class Hierarchy,
> 1. **this** is for current class
> 2. **super** is for class right above current class in the inhertance tree

## Overririding super class functions

Example:
```dart
class Animal{
  final String name;

  Animal({required this.name});

  void whatAmI() => print('I\'m an animal!');
}

class Bird extends Animal{
  Bird(String name):super(name:name);

  @override
  void whatAmI()=>Print('I\'m an Bird!');
}
```

>${\textsf{\color{red}Note:}}$ Dart doest not allow multiple inheritance. so one question comes here is that since all classes extends object class by default then in above example Is Bird class  extending both Animal and Object class? 

>The answer is No. Because when Bird class extends the Animal class the dart internally replace the Object class with Animal class in inhertance tree. Inheritance tree will look something like this

>Bird -> Animal, Animal -> Object => Bird -> Object also


## Covariant

covariant is a keyword that's used to specify a type parameter that can be safely widened in the inheritance hierarchy. In other words, it allows a child class to use a more specific type than the one used in the parent class, without causing any issues with the type system

```dart
class Animal {
  final String name;

  Animal({required this.name});

  void whatAmI() => print('I\'m $name');

  void chase(Animal e) => print('$e is getting chased');
}

class Mouse extends Animal {
  Mouse(String name) : super(name: name);

  @override
  void whatAmI() => print('I\'m a $name!');
}

class Cat extends Animal {
  Cat(String name) : super(name: name);

  @override
  void whatAmI() => print('I\'m a $name!');

  @override
  void chase(covariant Mouse d) =>
      print('${d.name} is getting chased by $name!');
}

class Dog extends Animal {
  Dog(String name) : super(name: name);

  @override
  void whatAmI() => print('I\'m a $name!');
}

void main(List<String> arguments) {
  Cat cat = Cat('Tom');
  Dog dog = Dog('Scooby');
  Mouse mouse = Mouse('Jerry');
  cat.chase(mouse);  //output: Jerry is getting chased by Tom!
 cat.chase(dog);   // this wil give compile time error: The argument type 'Dog' can't be assigned to the parameter type 'Mouse'
}
```

Changing the function signature to chase(covariant Mouse x) (where Mouse is a subclass of Animal) does three things:
1. Allows you to omit the x is Mouse check, as it is done for you.
2. Creates a compile time error if any Dart code calls Cat.chase(x) where x is not a Mouse or its subclass — if known at compile time.
3. Creates a runtime error in other cases.

**Why is it called 'covariant'?**

Covariant is a fancy type theory term, but it basically means 'this class or its subclasses'. Put another way, it means types that are equal or lower in the type hierarchy.

You are explicitly telling Dart to tighten the type checking of this argument to a subclass of the original. For the first example: tightening Animal to Mouse; for the second: tightening Object to Point.

Useful related terms are contravariant, which means types equal or higher in the type hierarchy, and invariant, which means exactly this type


 ![Inheritance Checklist](images/inheritance_checklist.png 'Inheritance Checklist')



 ## Why Dart does not allow multiple inheritance

 ![Why SIngle Inheritance](images/inheritance2.png 'Inheritance')

 In the above diagram famous problem **Deadly Diamond of Death** that comes with multiple inheritance is represented.

 Inheritance hierarchy for above example

 Performer (base class) 

 Guitarist(subclass) ---> Performer (base class)
 
 Drummer (subclass) ---> Performer (base class) 

now a Musician can be both Guitarist and a Drummer

```dart
            |---> Guitarist 
 Musician --|
            |---> Drummer
```
Now in above scenario Dart is unable to identify which perform method should be called for musician class. 