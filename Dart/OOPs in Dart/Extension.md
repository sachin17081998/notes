# **Dart Extensions**

The feature named Extension methods, Introduced in Dart 2.7 allows us to add new functionality to already available libraries. Although it does not allow us to modify existing code directly, It gives us the ability to customize a class in a way that we can read and use it easily.

We can also write an extension for getters, setters, and operators as well, these are called Extension members.

Syntax:
```dart
 extension <extension name> on <type> {
  (<member definition>)*
}
```

Example:
```dart
extension ExtendedString on String {
  bool get isValidName {
    return !this.contains(new RegExp(r'[0–9]'));
  }
}
main() {
  print('Pizza'.isValidName);
}
//Output: True
```

Above we have created our first extension named as ExtendedString on a String class. Inside this, we have written a getter which determines whether an instance of String contains a valid name or not using the given keyword.

>If you notice carefully we have actually written  ***Extended Getter***.

### **Extended Method**

What if you want to concatenate a string with some predefined string. Let’s write an extended method for it.

```dart
extension ExtendedString on String {
String prefixWith(String prefix) {
  return '$prefix $this';
 }
}
main() {
   print('Pizza'.prefixWith('I love'));
}
//Output: I love Pizza
```

### **Extended Operators**

```dart
extension ExtendedString on String {
 String operator &(String prefix) => '$prefix $this';
}
main() {
     print('Pizaa'&'I Love');
}
//Output: I love Pizza
```


### **How to use power of Extension in flutter**
Suppose you want different alignment of children in the column. Normally you would do something like this.

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    Align(
      alignment: AlignmentDirectional.centerStart,
      child: Text('I am aligned at start'),
    ),
    Align(
      alignment: AlignmentDirectional.centerEnd,
      child: Container(height: 20,width:50,color:Colors.amber),
    ),
  ],
)
```

 now with extension methods,
```dart 
extension ExtendedText on Widget {
  alignAtStart() {
    return Align(
      alignment: AlignmentDirectional.centerStart,
      child: this,
    );
  }

  alignAtEnd() {
    return Align(
      alignment: AlignmentDirectional.centerEnd,
      child: this,
    );
  }
}

//widget
Column(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    Text('I am aligned at the start').alignAtStart(),
    Container(height: 20,width:50,color:Colors.amber).alignAtEnd()
  ],
)

```

> once you create extension on widget we will be also able to see this extension method in autocomplete of IDEs





