# **OOP in Dart**

# Classes and Object
**Class** defines a blueprint for creating object. It contains properties and methods that objects can have.
> class name is always defined using PascalCase in dart

**Syntax**

Class ClassName{
  
  // properties or fields
  // method or function

}

**Objects**  is a self contained unit of code and data. A object is a instance of class that contain properties(variables) and methods(funtions)

**syntax:**
ClassName objectName=ClassName()





# Constructors
A constructor is an special method used to initilize an object of a class. It is called automatically when an object is created and initilizes the properties of the object.

**syntax:**

ClassName(parameters){

  //constructor body

}

**example**
```dart
class Animal{
  String? name;
  bool? isMamal;
  int? legs;
  
  Animal(String name,bool isMamal,int legs){
    this.name=name;
    this.isMamal=isMamal;
    this.legs=legs;
  }
  
  //public method
  void showAnimal(){
    print('$name ${_animalType()} and has $legs legs ');
  }
  
  //a private method
  String _animalType(){
    return isMamal! ? 'is a mamal':'is a reptile';
  }
}

void main(){
Animal lion=Animal('Lion',true,4);
lion.showAnimal();  //Lion is a mamal and has 4 legs 
}
```
## Generative constructors

To instantiate a class, use a generative constructor

```dart
class Point {
  // Initializer list of variables and values
  double x = 2.0;
  double y = 2.0;

  // Generative constructor with initializing formal parameters:
  Point(this.x, this.y);
}
```
## Diffrent ways to pass values to a constructor

### 1. Constructor With Named Parameters

Use a named constructor to implement multiple constructors for a class or to provide extra clarity:

```dart
class Animal{
  String? name;
  bool? isMamal;
  int? legs;
  
  //optional parametrs are inside [] braces
  Animal( {this.name, this.isMamal, this.legs}){
    print('$name Object Created');
  }
  
  //public method
  void showAnimal(){
    print('$name ${_animalType()} and has $legs legs ');
  }
  
  //a private method
  String _animalType(){
    return isMamal! ? 'is a mamal':'is a reptile';
  }
}

void main(){
Animal lion=Animal(name:'Lion',isMamal:true,legs:4);//Lion Object Created
lion.showAnimal();  //Lion is a mamal and has 4 legs 
// Animal snake=Animal('Snake',false); //will show error
// snake.showAnimal();  
}
```

### 2. Constructor with default values

```dart
class Animal{
  String? name;
  bool? isMamal;
  int? legs;
  
  //optional parametrs are inside [] braces
  Animal({this.name='N/A', this.isMamal=false, this.legs=0}){
    print('$name Object Created');
  }
  
  //public method
  void showAnimal(){
    print('$name ${_animalType()} and has $legs legs ');
  }
  
  //a private method
  String _animalType(){
    return isMamal! ? 'is a mamal':'is a reptile';
  }
}

void main(){
Animal lion=Animal(name:'Lion',isMamal:true,legs:4);//Lion Object Created
lion.showAnimal();  //Lion is a mamal and has 4 legs 
Animal snake=Animal(); // N/A Object Created
snake.showAnimal();  //N/A is a reptile and has 0 legs 
}
```

### 3. Construnctor with Optional Parameters

```dart
class Animal{
  String? name;
  bool? isMamal;
  int? legs;
  
  //optional parametrs are inside [] braces
  Animal(String name,[bool isMamal=true,int legs=0]){
    this.name=name;
    this.isMamal=isMamal;
    this.legs=legs;
  }
  
  //public method
  void showAnimal(){
    print('$name ${_animalType()} and has $legs legs ');
  }
  
  //a private method
  String _animalType(){
    return isMamal! ? 'is a mamal':'is a reptile';
  }
}

void main(){
Animal lion=Animal('Lion',true,4);
lion.showAnimal();  //Lion is a mamal and has 4 legs 
Animal snake=Animal('Snake',false);
snake.showAnimal();  //Snake is a reptile and has 0 legs
}
```
## Default constructor
If you don't declare a constructor, Dart uses the default constructor. The default constructor is a generative constructor without arguments or name

```dart
class Animal{
  String? name;
  bool? isMamal;
  int? legs;

  Animal(){
    name='lion';
    isMamal=true;
    legs=4;
  }
  
  //public method
  void showAnimal(){
    print('$name ${_animalType()} and has $legs legs ');
  }
  
  //a private method
  String _animalType(){
    return isMamal! ? 'is a mamal':'is a reptile';
  }
}

void main(){
  Animal lion=Animal();
  lion.showAnimal();//lion is a mamal and has 4 legs 

}
```

## Named Constructor

Use a named constructor to implement multiple constructors for a class or to provide extra clarity:

```dart
const double xOrigin = 0;
const double yOrigin = 0;

class Point {
  final double x;
  final double y;

  // Sets the x and y instance variables
  // before the constructor body runs.
  Point(this.x, this.y);

  // Named constructor
  Point.origin()
      : x = xOrigin,
        y = yOrigin;
  //Named Constructor with prameters
  Point.origin(double x,double y){
    this.x=x;
    this.y=y;
  }      
}

final x=Point.origin();
```
A subclass doesn't inherit a superclass's named constructor. To create a subclass with a named constructor defined in the superclass, implement that constructor in the subclass.




## Constant Constructor

If your class produces unchanging objects, make these objects compile-time constants. To make objects compile-time constants, define a const constructor with all instance variables set as final.

> Note: Constant constructor creates objects whose values cannot be changed. It improves the performance of the program. All the instance of a class with const constructor will point to same meomory.  For declaring a constant constructor all the fileds must be final in the class and the instance of the class has to be created using const keyword.

```dart
// class Animal{
//   String? name;
//   bool? isMamal;
//   int? legs;
  
//   optional parametrs are inside [] braces
//   Animal({this.name='N/A', this.isMamal=false, this.legs=0}){
//     print('$name Object Created');
//   }
  

  
//   //public method
//   void showAnimal(){
//     print('$name ${_animalType()} and has $legs legs ');
//   }
  
//   //a private method
//   String _animalType(){
//     return isMamal! ? 'is a mamal':'is a reptile';
//   }
// }

class Points{
  final int x;
  final int y;
  const Points(this.x,this.y);
  void showPoints(){
     print('x=$x and y=$y');

  }
}
void main(){

// p1 and p2 will have smae has code  
Points p1=const Points(2,3);
Points p2=const Points(2,3);

// p3 nd p4 will have diffrent hash code  
Points p3=Points(5,5);
Points p4=Points(5,5);
  
print('p1 hash code= ${p1.hashCode}'); //p1 hash code= 451659336
print('p2 hash code= ${p2.hashCode}'); //p1 hash code= 451659336
print('p3 hash code= ${p3.hashCode}'); //p3 hash code= 231533825
print('p4 hash code= ${p4.hashCode}'); //p4 hash code= 737892473
}
```

## Redirecting constructors

A constructor might redirect to another constructor in the same class. A redirecting constructor has an empty body. The constructor uses this instead of the class name after a colon (:).

```dart
class Point {
  double x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(double x) : this(x, 0);
}
```

## Factory Constructor

Till now all the constructors that we have learned have one single goal and that is to create an instance of the class. but what if you want to :
  * Instantiate a class using already existing instance
  * You want to do some trivial operations before instianting the class like check some condition
  * Or you want to instiantiate class using some new or existing instance of its subtype 

To perform all this operations dart provide us factory constructor.
```dart
//syntax
class ClassName {
  factory ClassName() {
    // TODO: return ClassName instance
  }

  factory ClassName {
    // TODO: return ClassName instance
  }
}
```

Rules of a factory constructor
* Factory constructor must return an instance of the class or sub-class.
* You can’t use this keyword inside factory constructor.
* It can be named or unnamed and called like normal constructor.
* It can’t access instance members of the class

Examples:

1. In first example we will not use the factory constructor
   
```dart 
class Area {
  final int length;
  final int breadth;
  final int area;

  // Initializer list 
 const Area(this.length, this.breadth) : area = length * breadth;
}

void main() {
  Area area = Area(10, 20);
  print("Area is: ${area.area}");  //output: Area is:200

  // notice that here is a negative value
  Area area2 = Area(-10, 20); //output: Area is:200
  print("Area is: ${area2.area}");
}
```
2. To avoide this situation we will create two cnstructors in our class. First one will be a private constructor that will instiantiate the class and second one will be a private constructor

```dart
class Area {
  final int length;
  final int breadth;
  final int area;

  // private constructor 
 const Area._internal(this.length, this.breadth) : area = length * breadth;
 
 
  // Factory constructor
  factory Area(int length, int breadth) {
    if (length < 0 || breadth < 0) {
      throw Exception("Length and breadth must be positive");
    }
    // redirect to private constructor
    return Area._internal(length, breadth);
  }
}

void main() {
  Area area = Area(10, 20);
  print("Area is: ${area.area}"); //output: Area is:200

  // notice that here is a negative value
  Area area2 = Area(-10, 20);
  print("Area is: ${area2.area}"); //Throws Exception
}
``` 

3. Using existing Instance: In this example below, there is class Person with a final field name. It also has a private constructor and a static _cache field. The class also has a factory constructor that checks if the _cache field contains a key that matches the name parameter. If it does, it returns the Person object associated with that key. Otherwise, it creates a new Person object, adds it to the _cache, and returns it.

```dart
class Person {
  // final fields
  final String name;

  // private constructor
  Person._internal(this.name);

  // static _cache field
  static final Map<String, Person> _cache = <String, Person>{};

  // factory constructor
  factory Person(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name]!;
    } else {
      final person = Person._internal(name);
      _cache[name] = person;
      return person;
    }
  }
}

void main() {
  final person1 = Person('John');
  final person2 = Person('Harry');
  final person3 = Person('John');

  // hashcode of person1 and person3 are same
  print("Person1 name is : ${person1.name} with hashcode ${person1.hashCode}");
  print("Person2 name is : ${person2.name} with hashcode ${person2.hashCode}");
  print("Person3 name is : ${person3.name} with hashcode ${person3.hashCode}");
}

/* Output:
Person1 name is : John with hashcode 1049968079
Person2 name is : Harry with hashcode 376975505
Person3 name is : John with hashcode 1049968079
*/
```

# Singelton Class

 As the name suggests, is a singleton class that permits only one instance of itself to get created. This singleton instance provides a global point of access to it. In simpler terms, utilizing the singleton class, we restrict the instantiation of a class to a single instance that can be used across different classes in the Dart program.

 One of the powerhouse advantages of implementing a Dart Singleton is the control over resource consumption. Often, creating many instances consumes a lot of resources, pressing a need for a design pattern where only one instance is created, and the same instance is shared with every class requesting it.

With the Singleton Pattern, every repeated call will return the same instance, thus aligning with instance conservation and resource optimization.


## How to create a singelton class

A singelton class can be created by defining a class that keeps track of sole instance by saving it in a static variable and every time a new instance is created it returns that same static instance.

```dart
// Singleton using dart factory
class SingletonExample {
 // static variable to create and store the instance during class load
 static final SingletonExample _instance = SingletonExample._internal();
 
 // private constructor is used to instantiate the class and it is made private beacuse we dont want any external instantiation
 SingletonExample._internal();

 
// factory constructor which return the existing staic instance of the class
 factory SingletonExample() {
   return _instance;
 }
}
 
void main() {
 SingletonExample obj1 = SingletonExample();
 SingletonExample obj2 = SingletonExample();
 print(obj1.hashCode);  //output:173353931
 print(obj2.hashCode);  //output:173353931
}
```

# Links for refrence
1. constructors: https://dart-tutorial.com/object-oriented-programming/factory-constructor-in-dart/
2. singelton:  https://www.dhiwise.com/post/enriching-flutter-architecture-dart-singleton