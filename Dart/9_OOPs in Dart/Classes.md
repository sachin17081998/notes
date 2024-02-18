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
## Default constructor
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

## Constant Constructor

Constant constructor creates a constant object. A constant object is an object whose value cannot be changed. A constant constructor is declared using const keyword.

All properties of the class must be final
> Note: Constant constructor creates objects whose values cannot be changed. It improves the performance of the program.

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
## Construnctor with Optional Parameters

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

## Constructor With Named Parameters

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

## Constructor with default values
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
