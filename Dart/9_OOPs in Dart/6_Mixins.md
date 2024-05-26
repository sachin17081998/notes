# ** Mixins in Dart**

Mixin is special kind of class that allows you to mix the behaviour of one class with another.
Mixin can not be insitaied and does not have constructor.

Mixins are a way of reusing a class’s code in multiple class hierarchies.

lets understand this with an example


 ![Mixins](images/mixin1.png 'Mixins')

When complier start executing in the perforem()method then compiler will go to hierarchy tree ,  the first emement is Drummer so it will look for perform method inside Drummer if method is available then it it will execute the method otherwise compiler wil go up in the hierarhy tree and will found Guitarist and then compiler will look inside guitarist mixin and son on the process will continue until compiler does not find the perform method.


 >${\textsf{\color{red}Note:}}$ So the class hierarchy comletely depends on the order in which you are using the mixins

 So if you don't extend any class then automatically object class will come in inheritance hierarchy.

  ![Mixins](images/mixin2.png 'Mixins')


  ## On keyword with mixin

  The 'on' keyword restricts the use of a Mixin to only classes that extend or implement a specific class. When you use **'on'** keyword then the class that using the mix must also have to extend the inherited-class of mixin

Example:
```dart

class Performer {
  void perform() => print('Performer performing...');
}

mixin Guitarist on Performer {
  void playGuitar() => print('Guitarist is playing guitar');
  void test() => perform();
}

mixin Drummer {
  void playDrum() => print('Drummer is playing drums');
  void perform() => playDrum();
}

class Musician extends Performer with Guitarist, Drummer {}

void main() {
  Musician musician = Musician();

  musician.playDrum(); //Drummer is playing drums
  musician.playGuitar(); //Guitarist is playing guitar
  musician.perform(); //Drummer is playing drums
  musician.test(); //Drummer is playing drums
}
```

so in above example when compiler executes the test() method then it first goes to Drummer (not found)---> Guitarist(found). 
Compiler will execute the test() method of Guitarist class but inside here again perform() method is called so the compiler will again start from the bottom of Hierarchy tree and since it found the perform() of Drummer class first so it will execute it.


 >${\textsf{\color{red}Note:}} Compiler always start from the bottom of hierarchy tree whenever executing a method because all the methods of the Musician class is in same place so copiler can not directly identify.
