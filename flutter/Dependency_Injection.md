# Dependecy Injection

lets try to <mark> understand </mark> the dependecy injection with an single example of **_car_** and **_engine_**.

```dart
class GasEngine{
  GasEngine(){
    print("Gas Engine Started: brum-brum-brum....");
  }
}
class Car{
  GasEngine engine=GasEngine();
}

void main() {
  Car car=Car();
}
```

**Output**

` Gas Engine Started: brum-brum-brum....`

---

In the above example Car class is dependent on engine class and in above code we have **_tightly coupled_** the engine class with car class.<br>
Now what if we want to change the GasEngine to an ElectricEngine. For doing this we will need to again update the Car class.
so because of one simple dependecy the entire class needs to be updated. And the bigger the application the more issues and headache we will have to add and use new type of engine

In other words with this approach is that our high level Car class is dependent on the lower level GasEngine class which violate Dependency **Inversion Principle(DIP)** from **_SOLID_**. DIP suggests that we should depend on abstractions, not concrete classes. So to satisfy this we introduce Engine interface and rewrite code like below

```dart
// an abstract class for all type of engine
abstract class Engine{
  void Start();
}

class GasEngine extends Engine{
  @override
  void Start() {
    print("Gas Engine Started: brum-brum-brum....");
  }
}

class ElectricEngine extends Engine{
  @override
  void Start() {
    print("Electric Engine Started: zoom-zoom-zoom....");
  }
}
class Car{
  Engine engine;

  Car({required this.engine}){
    engine.Start();
  }
}

void main() {
    //injectiong dependecy at run time
  Car GasCar=Car(engine:GasEngine());
  Car ElectricCar=Car(engine:ElectricEngine());
}
```

**Output**
`Gas Engine Started: brum-brum-brum....`

`Electric Engine Started: zoom-zoom-zoom....`

---

<br><br>

# Dependecy Injection In Flutter

for understanding Dependecy injection lets try to build the below screen in which multiple widgets are nested in one another

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const Building());
}

class Building extends StatelessWidget {
  const Building({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(
        body: Center(child: Level1())
        ),
      );
  }
}


class Level1 extends StatelessWidget {
  const Level1({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.greenAccent,
      width: 300,
      height: 500,
      child: const Center(child:Level2()),
    );
  }
}

class Level2 extends StatelessWidget {
  const Level2({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.redAccent,
      width: 250,
      height: 400,
      child: const Center(child:Level3()),
    );
  }
}

class Level3 extends StatelessWidget {
  const Level3({Key? key}) : super(key: key);

   @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.blueAccent,
      width: 200,
      height: 300,
      child: const Center(child:Level4()),
    );
  }
}

class Level4 extends StatelessWidget {
  const Level4({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.yellowAccent,
      width: 150,
      height: 200,
      child: const Center(child:Text("Level-4")),
    );
  }
}


```

**Output**
![Simulator Output](images/image1.png 'Builing class')

---

Now suppose we want the text that is displayed in Level-4 widget to change.
As we can see the root widget is **_Level1_** and we are calling this widget to build all other Levels.

so one way is to pass data in each widget constructor like below

```dart

class Building extends StatelessWidget {
  const Building({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      home: Scaffold(
        body: Center(child: Level1(data: "data for level 4",))
        ),
      );
  }
}


class Level1 extends StatelessWidget {
  final String data;
  const Level1({Key? key, required this.data}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.greenAccent,
      width: 300,
      height: 500,
      child: const Center(child:Level2(data: data,)),
    );
  }
}

class Level2 extends StatelessWidget {
  final String data;
  const Level2({Key? key,required this.data}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.redAccent,
      width: 250,
      height: 400,
      child: const Center(child:Level3()),
    );
  }
}
.
.
.

Level4{
  .
  .
  .
  Text(data);
}
```

But this approach is not an efficient approach because we need the data in lony level-4 wisget but for that wh have to unnecessarly add data to each level widget.

so here Dependecy Injection comes into action

---

# DI in Flutter using Get_It Package

To obtain clean code and to have loosely coupled system you need to use some kind of inversion of control (IOC). The get_it package is a service locator, in which you would have a central registry where you can register the classes and then obtain an instance of those class.

![Simulator Output](images/imag2.png 'Builing class')

As you can see in the above image, both dependency injection and service locator are a form of IOC.

Inversion of control means an external framework will create the needed object, for example we can use an xml file to configure the objects and then the framework will create them.

Dependency Injection, is a way to achieve IOC through constructor injection, setter injection or property injection.

Service Locator, also it is a way to achieve IOC but the main difference with DI is that every class will have a dependency on the service locator while using DI the class is given the dependencies without knowing from where they came from.

## **_Get_It_**

Get it is one of the more popular ways to handle DI in flutter apps. You can also register your dependencies as singletons, lazy singletons or factory.

1. A singleton will always return the same instance of that service.
2. A lazy Singleton will create the object on the first instance when it is called. This is useful when you have a service that takes time to start and should only start when it is needed.
3. A Factory will return a new instance of the service anytime it is called.

### **Steps to use Get_it**

1. First, add get_it to your pubspec.yaml file:

> `get_it: ^7.2.0`

2. create a Message class which we will register as service.

```dart
class Message{
  String message;
  Message({required this.message});

  void changeMessage(String msg){
    message=msg;
  }

  String getMessage()=>message;
}
```

3. Now, you can create a file to register all your objects, I’ll call mine service_locator.dart, and place a single function in it called getService().

```dart
import 'package:dependecy_injection_demo/message_class.dart';
import 'package:get_it/get_it.dart';

GetIt Di = GetIt.I;

void getServices() {
  Di.registerFactory(() => Message(message: 'Inner bloc'));
}

```

in the above case we register the class as Factory

**_Factory_**

> When you request an instance of the type from the service provider you'll get a new instance every time. Good for registering ViewModels that need to run the same logic on start or that has to be new when the view is opened.

4. now inject the dependecy in the **_Level_4_** widget only.

```dart
class Level4 extends StatelessWidget {
   Level4({Key? key}) : super(key: key);
  var msg=Di<Message>();
  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.yellowAccent,
      width: 150,
      height: 200,
      child: Center(child:Text(msg.getMessage())),
    );
  }
}
```

**Output**
![Simulator Output](images/image3.png 'Builing class')

in the above example we register the **_Message_** class as **_Factory_** b because of this we get new instance of Message class when we inject it.
so for creating a single resuable instance we need to register the Message class as **_Singelton_**.

**_ Singelton _**

> Singletons can be registered in two ways. Provide an implementation upon registration or provide a lamda that will be invoked the first time your instance is requested (LazySingleton). The Locator keeps a single instance of your registered type and will always return you that instance.

update **_get_service()_** function and register Message class as singelton.

```dart
import 'package:dependecy_injection_demo/message_class.dart';
import 'package:get_it/get_it.dart';

GetIt Di = GetIt.I;

void getServices(){
  Di.registerSingleton(Message(message: 'Inner bloc'));
}
```

> `we can also use Lazy singelton which will only initilize the object when it is injected. This can help cases when creating an object of a service is time consuming`

after this we again injected the Message in building class and updated the message by calling method **_changeMessage()_**.

```dart
import 'package:dependecy_injection_demo/service_locator.dart';
import 'package:flutter/material.dart';

import 'message_class.dart';

void main()async {

 getServices();
  runApp( Building());
}

class Building extends StatelessWidget {
   Building({Key? key}) : super(key: key);
var msg=Di<Message>();
  @override
  Widget build(BuildContext context) {
    msg.changeMessage('updated Message');
    return const MaterialApp(
      home: Scaffold(
        body: Center(child: Level1())
        ),
      );
  }
}


class Level1 extends StatelessWidget {

  const Level1({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.greenAccent,
      width: 300,
      height: 500,
      child: const Center(child:Level2()),
    );
  }
}

class Level2 extends StatelessWidget {

  const Level2({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.redAccent,
      width: 250,
      height: 400,
      child: const Center(child:Level3()),
    );
  }
}

class Level3 extends StatelessWidget {
  const Level3({Key? key}) : super(key: key);

   @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.blueAccent,
      width: 200,
      height: 300,
      child:  Center(child:Level4()),
    );
  }
}

class Level4 extends StatelessWidget {
   Level4({Key? key}) : super(key: key);
  var msg=Di<Message>();
  @override
  Widget build(BuildContext context) {
    return Container(
      color: Colors.yellowAccent,
      width: 150,
      height: 200,
      child: Center(child:Text(msg.getMessage())),
    );
  }
}

```

> In the below output one can see the message is updated when building widget is constructed and finally the updated message is painted in the level-4 widget, although we are using same instance.

**Output**
![Simulator Output](images/image4.png 'Builing class')

---

# Resources

1. [Medium Article on DI](https://levelup.gitconnected.com/dependency-injection-in-dart-flutter-apps-3332f1a61041)

2. [Dependecy Injection](https://www.filledstacks.com/post/flutter-dependency-injection-a-beginners-guide/)

3. [Dependecy Injection-2](https://medium.com/filledstacks/dependency-injection-in-flutter-2225d3081f61)

4. [Inversion Of Control (IOC)](https://medium.com/@aaron.chu/whats-inversion-of-control-ioc-fb09e2ad7b63)
