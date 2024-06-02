# **Testing in Dart**

```dart
import 'dart:math';

enum TriangleType {
  equilateralTriangle, //all sides are qual
  isoscelesTriangle, // two sides are equal
  scalleneTriangle, //no sides are equal
  invalid,
  valid
}

class Triangle {
  final TriangleType type;
//sides
  num? a;
  num? b;
  num? c;

//base and height
  num? base;
  num? height;

  //intilize trinagle using side lengths
  Triangle.withSides({required this.a, required this.b, required this.c})
      : type = validTriangleTypeGiven(a!, b!, c!);

  //intilize using base and height
  Triangle.withBaseAndHeight({required this.base, required this.height})
      : type = validTriangleBaseAndHeight(base!, height!);

  static bool validTrinagle(num a, num b, num c) {
    return (a + b > c && a + c > b && b + c > a);
  }

  static TriangleType validTriangleTypeGiven(num a, num b, num c) {
    if (!validTrinagle(a, b, c)) {
      return TriangleType.invalid;
    }

    if (a == b && a == c) {
      return TriangleType.equilateralTriangle;
    } else if (a == b || a == c || b == c) {
      return TriangleType.isoscelesTriangle;
    }

    return TriangleType.scalleneTriangle;
  }

  static validTriangleBaseAndHeight(num? base, num? height) =>
      base! >= 0 && height! >= 0 ? TriangleType.valid : TriangleType.invalid;

  num get area {
    switch (type) {
      case TriangleType.equilateralTriangle:
      case TriangleType.isoscelesTriangle:
      case TriangleType.scalleneTriangle:
        final s = 1 / 2 * (a! + b! + c!);
        return sqrt(s * (s - a!) * (s - b!) * (s - c!));
      case TriangleType.valid:
        return (1 / 2) * base! * height!;
      case TriangleType.invalid:
        throw Exception('Trying to compute area of an Invalid Trinagle');
    }
  }
}


void main() {
  Triangle trinagle1 = Triangle.withSides(a: 5, b: 10, c: 7);
  print(trinagle1.type);
  print(trinagle1.area);

  Triangle trinagle2 = Triangle.withBaseAndHeight(base: 12, height: 6);
  print(trinagle2.area);
}

```



import 'package:test/test.dart';

void main(){
  
}