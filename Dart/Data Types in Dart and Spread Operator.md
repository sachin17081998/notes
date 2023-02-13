# Data Types In Dart and Spread Operator

1. Numbers
2. Strings
3. Booleans
4. Lists
5. Maps
6. Sets
7. Runes
8. Null
   

> ## **Lists**
List data type is similar to arrays in other programming languages. List is used to representing a collection of objects. It is an ordered group of objects. The core libraries in Dart are responsible for the existence of the List class, its creation, and manipulation.
List are of two type:

1. Fixed Length List
2. Growable List
   
### ***Fixed Lingth List***
syntax:
>List ? list_Name = List.filled(number of elements, E, growanle:boolean);

```dart

void main()
{
    List? gfg = List.filled(5, null, growable: false);
    gfg[0] = 'Geeks';
    gfg[1] = 'For';
    gfg[2] = 'Geeks';
 
    // Printing all the values in List
    print(gfg);
 
    // Printing value at specific position
    print(gfg[2]);
}
```
## ***Growable List***
```dart
void main()
{
    var gfg = [ 'Geeks', 'For' ];
 
    // Printing all the values in List
    print(gfg);
 
    // Adding new value in List and printing it
    gfg.add('Geeks'); // list_name.add(value);
    print(gfg);
}
```


***Example on List**
```dart
void main(){

  List data=[1,2,3,4,5,6,7,8];
  
  //properties avalable in list
  print(data.length);
  print(data.reversed);
  print(data.isEmpty);
  print(data.last);
  print(data.first);

   //methods avalable in list
  data.add(9);
  print(data.contains(5));
  data.insert(0,0);
  
}
```


> ## **Maps**

The Map object is a simple key/value pair. Keys and values in a map may be of any type. A Map is a dynamic collection. In other words, Maps can grow and shrink at runtime.

```dart
void main() { 
   var details = {'Usrname':'tom','Password':'pass@123'}; 
   details['Uid'] = 'U1oo1'; 
   print(details); 
} 
```
output: {Usrname: tom, Password: pass@123, Uid: U1oo1}


```dart
void main(){

  Map data={ 'one':1,'two':2,'three':3};
  
  //properties avalable in map
  print(data.length);   //3
  print(data.keys);     //(one, two, three)
  print(data.values);   //(1, 2, 3)
  print(data.isEmpty);  //false
  print(data.isNotEmpty);//true
  
  
  //adding data in map
  data['four']=4;

   //methods avalable in list
  data.remove('three');
  data.forEach((k,v)=>print('$k =>$v'));
  /*
one =>1
two =>2
four =>4
*/
  data.clear(); 
}
```

> ## **Sets**

Sets in Dart is a special case in List where all the inputs are unique i.e it doesn’t contain any repeated input. It can also be interpreted as an unordered array with unique inputs. The set comes in play when we want to store unique values in a single variable without considering the order of the inputs. The sets are declared by the use of a set keyword

syntax:
var variable_name = <variable_type>{};
or,
Set <variable_type> variable_name = {};

> ## **Runes**

with runes you can find unicode values of a string.
```dart
String value="a";
print(value.runes); //97
```

# *** Spread Operator***
In Dart, Spread Operator (…) and Null-aware Spread Operator (…?) are used for inserting multiple elements in a collection like Lists, Maps, etc.

```dart
void main(){

  Map mapData={ 'one':1,'two':2,'three':3};
  List listData=[1,2,3,4,5,6];
  Set setData={3,421,53,1};
  
  Map mapData2={ 'six':6,'seven':7,};
  List listData2=[10,11,12];
  Set setData2={6,41,53,1};
  
  Map mapData3={...mapData,'four':4,'five':5,...mapData2};
  List listData3=[...listData,7,8,9,...listData2];
  Set setData3={...setData,1,5,53,...setData2};
  
  print(listData3);
  //[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
  print(mapData3);
  //{one: 1, two: 2, three: 3, four: 4, five: 5, six: 6, seven: 7}
  print(setData3);
  //{3, 421, 53, 1, 5, 6, 41}
  
}
```