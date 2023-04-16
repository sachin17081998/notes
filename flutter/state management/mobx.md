# **MobX**
![Mobx](images/mobx.png 'Mobx')

MobX has 3 important pillars

## **Observable**
Observables represent the reactive-state of your application.
The term reactive data means that a change to data causes a notification to be fired to every interested observer. It is the application of the classic Observer Design Pattern.

## **Actions**
Actions are how you mutate the observables. Rather than mutating them directly, actions add a semantic meaning to the mutations. For example, instead of just doing value++, firing an increment() action carries more meaning. Besides, actions also batch up all the notifications and ensure the changes are notified only after they complete. Thus, the observers are notified only upon the atomic completion of the action.
>Note that actions can also be nested, in which case the notifications go out when the top-most action has completed.

## **Reaction**
Reactions complete the MobX triad of observables, actions and reactions. They are the observers of the reactive-system and get notified whenever an observable they track is changed. 
Reaction can also fire action which completes the cycle of MobX

lets discuss each section pillar one by one

# **Observable**

Before we start digging into observables we irst need to understand the two states in flutter

* Core State
* Derived State
  
Core state can be understood as the basic data that drives your business. for example if you are making a contact app thene user first-name and last-name will be your core state.

Derived State is something that depends oncore state or other derived state. So from above example we can say full-name of customer will be your derived state because its value can be derived from first and last name.
> Derived-State is almost always read-only. In the MobX parlance, derived-state is also called computed properties, or just computeds.

> State in MobX = Core-State + Derived-State 

## Observables


Observable(T initialValue, {String name, ReactiveContext context})
* T initialValue: the initial value for the Observable or null otherwise.
* String name: a name to identify during debugging
* ReactiveContext context: the context to which this observable is bound. By default, all observables are bound to the singleton mainContext of the application.

An Observable is used to track a single value, either primitive or complex. Whenever it changes value, it will fire a notification so that all connected reactions will re-execute

Example:
```dart
final counter = Observable(0); // initially 0

final list = Observable<List<Todo>>(); // start with an initialValue of null
```

There are diffrent kind of observables that are responsible to store difrent kind of data.

![Mobx](images/mobx2.png 'Mobx')

#### **Observable list**
ObservableList({ReactiveContext context})

ReactiveContext context: the context to which this list is bound. By default, all ObservableLists are bound to the singleton mainContext of the application.

Example:
```dart
class Controller {
  final ObservableList<String> observableList = ObservableList<String>();
}
```

```dart
  Observer(builder: (_) {
            return SizedBox(
              width: 1024,
              height: 512,
              child: ChildWidget(
                list: controller.observableList.toList(), // Mobx will detect mutations to observableList
              ),
            );
          }),

class ChildWidget extends StatelessWidget {
  const ChildWidget({super.key, required this.list});

/// Don't use ObservableList here otherwise the context for parent widget
/// observer will change and it will not track these mutations.

  final List<String> list;

  @override
  Widget build(BuildContext context) {
    return ListView.builder(
        itemCount: list.length,
        itemBuilder: (context, index) {
          return SizedBox(
            width: 112,
            height: 48,
            child: ListTile(
              title: Text(list[index]),
            ),
          );
        });
  }
}          
```



### **ObservableMap**
ObservableMap({ReactiveContext context})

ReactiveContext context: the context to which this map is bound. By default, all ObservableMaps are bound to the singleton mainContext of the application.
An ObservableMap gives you a deeper level of observability on a map of values. It tracks when keys are added, removed or modified and notifies the observers. Use an ObservableMap when a change in the map matters. You can couple this with the @observable annotation to also track when the map reference changes, eg: going from null to a map with values.

### **ObservableSet**
ObservableSet({ReactiveContext context})

ReactiveContext context: the context to which this set is bound. By default, all ObservableSets are bound to the singleton mainContext of the application.

An ObservableSet gives you a deeper level of observability on a set of values. It tracks when values are added, removed or modified and notifies the observers. Use an ObservableSet when a change in the set matters. You can couple this with the @observable annotation to also track when the set reference changes, eg: going from null to a set with values.

### **ObservableFuture**
ObservableFuture(Future< T > future, {ReactiveContext context})

* Future< T > future: The future that is tracked for status changes.
* ReactiveContext context: the context to which this observable-future is bound.
 
By default, all ObservableFutures are bound to the singleton mainContext of the application.
The ObservableFuture is the reactive wrapper around a Future. You can use it to show the UI under various states of a Future, from pending to fulfilled or rejected. The status, result and error fields of an ObservableFuture are observable and can be consumed on the UI.

Here is a simple LoadingIndicator widget that uses the ObservableFuture to show a progress bar during a fetch operation:



