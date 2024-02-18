# Getter and Setter in Dart
**Getter** and **Setter** provides an explicit read and write access to an object properties. In dart **get** and **set** keywords are used to create getter and setter.

**Getter**: It reads the value of an property and act as an accessor.It is mostly used to access the private properties of class.

**Setter**: It updates the property and act as a mutator.It is mostly used to update the private properties of class.

```dart
class Student{
  String? _name;
  String? _gender;
  int? _age;
  
  String get name => _name != null ? _name!:'';
  
  String get gender => _gender != null ? _gender!:'';
  
  int get age => _age != null ? _age! : 0;
  
  set setName(String name)=>this._name=name;
  set setGender(String gender)=>this._gender=gender;
  set setAge(int age){
    if(age<0)throw new Exception("Age can't be 0");
   
    _age=age;
  }
  
}

void main(){
  Student ram=new Student();
  
  ram.setName='Ram';
  ram.setGender='Male';
  ram.setAge=50;
  
  print(' ${ram.name} ${ram.age}'); // Ram 50
}
```