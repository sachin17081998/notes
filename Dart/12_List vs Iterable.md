# **List VS Iterable in Dart**

## What are collections?

A collection is an object that represents a group of objects, which are called elements.

A collection can be empty, or it can contain many elements. Depending on the purpose, collections can have different structures and implementations. These are some of the most common collection types:

- List: Used to read elements by their indexes.
- Set: Used to contain elements that can occur only once.
- Map: Used to read elements using a key.
  


## List

List is a collection that is constructed non-lazily, that means it is constructed the moment it is definedand items are stored at a specific index.
List allows you to use [] brackets to access its elements.

## Iterable

An Iterable is an abstract collection that is constructed lazily, that means it will not be constructed when you define it. It will construct only when you try to access its elements. 
In iterable every time you try to access an element it will generate the list until the point the accessed element is found.

> An Iterable is a collection of elements that can be accessed  ${\textsf{\color{red}sequentially}}$

In iterable you cannot access element using [] brackets. For accessing elements you have to use:
- first
- last
- where()
- firstWhere(): 
- every(): return true if all the elements of iterable satisfy the condition
- any(): Returns true if at least one element satisfies the condition.
- for-in loop
- for-each loop
- elementAt

ETC...


## to understand better the diffrence between list and generator lets take an example

```dart
void main() {

  final normalist = normalList(10);
}

//creates a list of integer upto n
List<int> normalList(int n) {
  print('normal list started');
  final list = <int>[];
  for (var i = 0; i < n; i++) {
    print('i -> $i');
    list.add(i);
  }
  return list;
}

//generates a list of integer upto n
Iterable<int> iterableList(int n) sync* {
  print('An iterable list started');
  for (var i = 0; i < n; i++) {
    print('i -> $i');
    yield (i);
  }
}
```
output of above code will be
```dart
normal list started
i -> 0
i -> 1
i -> 2
i -> 3
i -> 4
i -> 5
i -> 6
i -> 7
i -> 8
i -> 9
```

lets use the second functon now in main


```dart
void main() {

  final normalist = iterableList(10);
}

output:

```
in this case output will be nothing because here iterable will generate only when we are accessing some element of it

```dart

void main() {
  final list = iterableList(10);

  print('accessing last element: ${list.last}');
  print('accessing 4 the element of iterable: ${list.elementAt(4)}');
  print('accessing first element: ${list.first}');
}

Output:
An iterable list started
i -> 0
i -> 1
i -> 2
i -> 3
i -> 4
i -> 5
i -> 6
i -> 7
i -> 8
i -> 9
accessing last element: 9

An iterable list started
i -> 0
i -> 1
i -> 2
i -> 3
i -> 4
accessing 4 the element of iterable: 4

An iterable list started
i -> 0
accessing first element: 0
```

so for first print statement sice we are trying to access the last element so entire iterable was generated

In second print, since we are trying to access 4th element so the list was agian generated till 4th elemnt

In third print, since first element was accessed so only 1 lement was generated.

 >${\textsf{\color{red}Note:}}$ You can see every time we are trying to access an element Iterable starts generating from first element and goes on to generate until the required element is accessed.




