# Functions In Dart
Function allows us to put a repetative code inside a block which then can be called to perform the task without riting the code again.
syntx:
### Defining a function
> return-type FunctionName (parametrs){
> function body
> }

### Calling a function
> functionName(arguments);

## ***Function Parametrs in Dart***

### 1. **Positional Parameters**

In positional parameter you must provide the arguments in the same order as you defined on parameters when you wrote the function.

```dart
void printInfo(String name,String gender){
  print('name:$name');
  print('gender:$gender');
}

void main(){
printInfo('tony','male'); //name:tony  gender:male

//problem:if u swap the arguments during calling then it may cause error

  printInfo('male','tony');  //gender:male  surname:tony
}
```
#### Default value to a positional parameter
```dart
void printInfo(String name,String gender, [String citizen='Indian']){

  print('name:$name gender:$gender citizen:$citizen');
}

void main(){
printInfo('tony','male','US'); //name:tony gender:male citizen:US

  printInfo('tony','male');  //name:tony gender:male citizen:Indian
}
```

### 2. **Named Parameters**
In named parameters we can pass arguments in any order.
```dart
void printInfo({String? name,String? gender}){

  print('name:$name gender:$gender');
}

void main(){
printInfo(name:'tony',gender:'male'); //name:tony gender:male

  printInfo(gender:'female');  //name:null gender:female
}
```

> **Note**:In named parameter you have to mark the parameter as nullable or required.

```dart
void printInfo({required String name,required String gender}){

  print('name:$name gender:$gender');
}

void main(){
printInfo(name:'tony',gender:'male'); //name:tony gender:male

  printInfo(gender:'female');  //show error
}
```


### 3. **Optional Parameters**
**square braces[]** are used to provide optional parameters/ while providing optional parameter either create it nullable or provide some default value

```dart
void printInfo( String name,[String? gender]){

  print('name:$name gender:$gender');
}

void main(){
printInfo('tony','male'); //name:tony gender:male

printInfo('tony');  //name:tony gender:null
}
```

# **Anonymous Functions in dart**

All the function that are defined above are called as **Named Functions**.

**Anonymous Function:** It is function that does not have name anf return type.

**syntax**
(parameter){ 
  
  function body

}

**Examples:**
```dart
void main(){
const fruits=['apple','banana','grapes'];

fruits.forEach((fruit){
  print(fruit);
});
}
```

second example

```dart
void main(){
var cube=(int num){ 
  return num*num*num;
};

var square=(int num)=>num*num;

print(cube(3));  //27
print(square(2));  //4
}
```