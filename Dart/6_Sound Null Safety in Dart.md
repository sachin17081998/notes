# **Sound Null Safety In Dart**

  ![Sound Null Safety](images/null_safety1.png 'Sound Null Safety')


 **Null Safety** prevents errors that result from unintentional access of variables set to null. For example, if a method expects an integer but receives null, your app causes a runtime error. This type of error, a null dereference error, can be difficult to debug.

 **Sound** means you will get null check error at while writing code not at run time.

 With sound null safety variables are ‘non-nullable’ by default: They can be assigned only values of the declared type (e.g. int i=42), and never be assigned null. You can specify that a type of a variable is nullable (e.g. int? i), and only then can they contain either a null or a value of the defined type.

Sound null safety changes potential runtime errors into edit-time analysis errors, by flagging when any non-nullable variable hasn’t been initialized with a non-null value or is being assigned a null. This allows you to fix these errors before deploying your app.

## **Null Safety Changes**
- Variable by default are not null, if you want to make any variable as nullable then you have to use (?) while defining the variable (datatype? variable-name). 
- run time error turns into edit time analysis error.
- Null Safety gaol is not to eliminate null from your code rather its goal is to prevent any unintentional occurrence of Null value.
- Null Safety Goal is to **have control** into **where**,**how** and **when** null can flow through your program.
- Implicit Downcasting was removed 



## **Dart Before and After Null Safety**

Since dart is an object oriented language so evry thing in dart is an object. All data types are classes that extends from an object class.

  ![Sound Null Safety](images/null_safety2.png 'Sound Null Safety')

  In the above diagram we can see all data types inherit from object class.

  Lets take example of inheritance and understand how the hierarch works.
  order of inheritance for Int class is:

  Object ---> Num --> Int->Null

  So according to L(from SOLID), parent class object can be replaced by child class object.
  So in place of Int we can put Null.

  Because of this Int can take both integer(1,2,..) and NULL value. 

**After Null Safety**

 ![Sound Null Safety](images/null_safety3.png 'Sound Null Safety')

 Now we can see that null does not directly inherit the int and other classes. So now in dart an Int class will only hold an integer value(1,2,3,...). 
 
 But now dart has to provide a way to assign null value to these new non-nullable classes.

 To solve this problem dart introduced a system in which if a variable is defined with question-mark '?'  then it can hold both null and not-null data.


![Sound Null Safety](images/null_safety4.png 'Sound Null Safety')


in the universe of dart there exist two first environment(object), can take only Not-Null data.

Second environment(object?) can take either Not-null data(object) or null data. so this doesn't mean a nullable data can directly access the methods of not-null class.

example:
```dart
void main(){
  String name ='starki';
  String? name2;
  print(name.toUpperCase());  //output:STARKI
  print(name2?.toUpperCase()); //output:null
}
```

in the above example we can se we need to use '?' while accessing the methods. if we don't use the '?' operator then we will get error.



## **Class Hierarchy in dart afte Null safety**

![Sound Null Safety](images/null_safety5.png 'Sound Null Safety')

Now Object class is not at top and Null class is not at bottom.

Rather at top we have object? class which is inherited by object and null class directly. 

At bottom we have a new type that is called Never.

## **Never**
This type can be used to interrupt the flow of the application by throwing some exception or error.

SO if you want to write a function that always throw an exception then you can use this Never type as return type of function.


Example:

```dart
Never foolishFunction(String type,Object value){
  throw ArgumentError('Expected $type but got ${value.runtimeType}');
}
void main(){
  foolishFunction('string',3);
}

//output
//Uncaught Error: Invalid argument(s): Expected string but got int
```

or

```dart
Never foolishFunction(String msg){
  throw Exception('Exception $msg happened');
}
void main(){
  foolishFunction('Hi..Hi..HA..HA..HA...');
}
//output
//Uncaught Error: Exception: Exception Hi..Hi..HA..HA..HA... happened
```


## **Implicit Downcasting**

So before Null Safety you could have written code like below:

```dart
void foolishFunction(String msg){
  print(msg);
}
void main(){
  Object m='Hi..Hi..HA..HA..HA...';
  foolishFunction(m);
}
```

so internally dart used to call the function using as  **foolishFunction(m as String)**

But after Null safety was introduced you have to explicitly use as keyword in code.


```dart
void foolishFunction(String msg){
  print(msg);
}
void main(){
  Object m='Hi..Hi..HA..HA..HA...';
  foolishFunction(m as String);
}
```


# Resources

1. [Official Dart Documentation ](https://dart.dev/null-safety/understanding-null-safety)

2. [Never type ](https://medium.com/flutter-community/dart-type-you-have-never-used-b5c63847b4e0)