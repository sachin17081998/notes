# **Generics in Dart**

Generics is a way to create a class, or function that can work with different types of data (objects). If you look at the internal implementation of List class, it is a generic class. It can work with different data types like int, String, double, etc. For example, List<int> is a list of integers, List<String> is a list of strings, and List<double> is a list of double values

## Why use generics?

Generics are often required for type safety, but they have more benefits than just allowing your code to run:
1. Properly specifying generic types results in better generated code.
2. You can use generics to reduce code duplication.

to better understand the power of generics lets implement a stack

```dart
//stack that can handle only integer data
import 'dart:io';

class Stack {
  List<int> _data = [];

  Stack(List<int>? data) {
    if (data != null) {
      _data = data;
    } else {
      _data = [];
    }
  }

  void push(int element) {
    _data.add(element);
  }

  bool canPop() {
    return _data.isNotEmpty;
  }

  void pop() {
    if (canPop()) {
      _data.removeLast();
    }
  }

  int? tos() {
    return canPop() ? _data[_data.length - 1] : null;
  }

  void traverseStack() {
    for (int element in _data) {
     stdout.write('$element ==> ');
    }
  }
}

void main() {

  Stack stack = Stack(null);
  stack.push(1);
  stack.push(2);
  stack.push(3);
  stack.traverseStack();
  stack.pop();
  print('\n_____________________________');
  stack.traverseStack();
}
/* output
1 ==> 2 ==> 3 ==> 
_____________________________
1 ==> 2 ==> 
*/
```

but what if we want our stack to operate with double or string or any object. So the current implementation can not handle such cases for that we need to use Generics and make out stack handle any kind of data.


```dart
import 'dart:io';

class Stack<E> {
  List<E> _data = [];

  Stack(List<E>? data) {
    if (data != null) {
      _data = data;
    } else {
      _data = [];
    }
  }

  void push(E element) {
    _data.add(element);
  }

  bool canPop() {
    return _data.isNotEmpty;
  }

  void pop() {
    if (canPop()) {
      _data.removeLast();
    }
  }

  E? tos() {
    return canPop() ? _data[_data.length - 1] : null;
  }

  void traverseStack() {
    for (E element in _data) {
      stdout.write('$element ==> ');
    }
  }

}

void main() {
  Stack stack = Stack(null);
  stack.push(1);
  stack.push(2);
  stack.push(3);
  stack.traverseStack();
  stack.pop();
  print('\n_____________________________');
  stack.traverseStack();
  print('\n_____________________________');

  Stack stackOfString = Stack(['a','B']);
  stackOfString.push(1);
  stackOfString.push(2);
  stackOfString.push(3);
  stackOfString.traverseStack();
  stackOfString.pop();
  print('\n_____________________________');
  stackOfString.traverseStack();
}

/* output
1 ==> 2 ==> 3 ==> 
_____________________________
1 ==> 2 ==> 
_____________________________
a ==> B ==> 1 ==> 2 ==> 3 ==> 
_____________________________
a ==> B ==> 1 ==> 2 ==> 
*/
```

waht if we want to add top elements of two stack. but in above implementation since we can take any element so addition  can not be possible because we can not add an object with an integer.

To solve this we have to limit our generic stack such that it can take only those element on which addition operation is possible.

for this we will use **extend** limit our stack to accept only num type elements.

```dart
import 'dart:io';

class Stack<E extends num> {
  List<E> _data = [];

  Stack(List<E>? data) {
    if (data != null) {
      _data = data;
    } else {
      _data = [];
    }
  }

  void push(E element) {
    _data.add(element);
  }

  bool canPop() {
    return _data.isNotEmpty;
  }

  void pop() {
    if (canPop()) {
      _data.removeLast();
    }
  }

  E? tos() {
    return canPop() ? _data[_data.length - 1] : null;
  }

  void traverseStack() {
    for (E element in _data) {
      stdout.write('$element ==> ');
    }
  }

  num? operator +(Stack<E> stack) {
    if (this.canPop() && stack.canPop()) {
      return this.tos()! + stack.tos()!;
    }
    return null;
  }
}

void main(){
    Stack stack = Stack(null);
  stack.push(1);
  stack.push(2);
  stack.push(3);
  stack.traverseStack();
  print('\n_____________________________');

  Stack stack2 = Stack([100, 200]);

  stack2.traverseStack();
  print('\n_____________________________');

  print('sum of tos= ${stack + stack2}');
}
/*output
1 ==> 2 ==> 3 ==> 
_____________________________
100 ==> 200 ==> 
_____________________________
sum of tos= 203
*/
```


## Generic Methods

Methods and functions also allow type generic arguments.

syntax:

```dart
T functionName<T>(parameters<T>){
  return T;
}
```

Example:
```dart
bool checkType<T>(T first, T second) {
  return first.runtimeType == second.runtimeType;
}

void main() {
 
  print(checkType(10, 25));  //true
  print(checkType(10, 'Ten')); // false
}
```

simlar to genric classes we can limit the generic methods to accept only certain type of elements

```dart
bool checkType<T extends num>(T first, T second) {
  return first.runtimeType == second.runtimeType;
}

void main() {
  print(checkType(10, 25)); // true
  print(checkType(10, 'Ten')); // will give error while wrinting the code
}
```