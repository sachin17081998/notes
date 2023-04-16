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

Before we start digging into observables we first need to understand the two states in flutter

* Core State
* Derived State
  
Core state can be understood as the basic data that drives your business. for example if you are making a contact app then user first-name and last-name will be your core state.

Derived State is something that depends oncore state or other derived state. So from above example we can say full-name of customer will be your derived state because its value can be derived from first and last name.
> Derived-State is almost always read-only. In the MobX parlance, derived-state is also called computed properties, or just computed.

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

There are different kind of observables that are responsible to store different kind of data.

![Mobx](images/mobx2.png 'Mobx')

#### <u>**Observable list**</u>
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



### <u>**ObservableMap**</u>
ObservableMap({ReactiveContext context})

ReactiveContext context: the context to which this map is bound. By default, all ObservableMaps are bound to the singleton mainContext of the application.
An ObservableMap gives you a deeper level of observability on a map of values. It tracks when keys are added, removed or modified and notifies the observers. Use an ObservableMap when a change in the map matters. You can couple this with the @observable annotation to also track when the map reference changes, eg: going from null to a map with values.

### <u> **ObservableSet**</u>
ObservableSet({ReactiveContext context})

ReactiveContext context: the context to which this set is bound. By default, all ObservableSets are bound to the singleton mainContext of the application.

An ObservableSet gives you a deeper level of observability on a set of values. It tracks when values are added, removed or modified and notifies the observers. Use an ObservableSet when a change in the set matters. You can couple this with the @observable annotation to also track when the set reference changes, eg: going from null to a set with values.

### <u>**ObservableFuture**</u>
ObservableFuture(Future< T > future, {ReactiveContext context})

* Future< T > future: The future that is tracked for status changes.
* ReactiveContext context: the context to which this observable-future is bound.
 
By default, all ObservableFutures are bound to the singleton mainContext of the application.
The ObservableFuture is the reactive wrapper around a Future. You can use it to show the UI under various states of a Future, from pending to fulfilled or rejected. The status, result and error fields of an ObservableFuture are observable and can be consumed on the UI.

Here is a simple LoadingIndicator widget that uses the ObservableFuture to show a progress bar during a fetch operation:

``` dart

// github_store.dart
part 'github_store.g.dart';

class GithubStore = _GithubStore with _$GithubStore;

abstract class _GithubStore with Store {
  // ...

  static ObservableFuture<List<Repository>> emptyResponse =
      ObservableFuture.value([]);

  // We are starting with an empty future to avoid a null check
  @observable
  ObservableFuture<List<Repository>> fetchReposFuture = emptyResponse;

  // ...
}

// github_widgets.dart
class LoadingIndicator extends StatelessWidget {
  const LoadingIndicator(this.store);

  final GithubStore store;

  @override
  Widget build(BuildContext context) => Observer(
      builder: (_) => store.fetchReposFuture.status == FutureStatus.pending
          ? const LinearProgressIndicator()
          : Container());
}
```



## Computed
Computed form the derived state of your application. They depend on other observables or computed for their value. Any time the depending observables change, they will recompute their new value. Computed`s are also smart and cache their previous value. Only when the computed-value is different than the cached-value, will they fire notifications. This behavior is key to ensure the connected reactions don't execute unnecessarily


```dart
final first = Observable('Tony');
final last = Observable('Stark');

final fullName = Computed(() => '${first.value} ${last.value}');

print(fullName.value); // Tony Stark

runInAction(() => first.value = 'Mohan');

print(fullName.value); // Mohan Stark
```



# **Actions**

Actions are functions that encapsulate the mutations on observables. They are used to give a semantic name to the operation, such as incrementCounter() instead of simply doing counter++. These semantic names are how you would identify the various operations happening in the domain, as exposed on the UI.

Additionally, actions also provide few other guarantees:

* Changes to the observables are only notified at the end of the action. This ensures all mutations happen as an atomic unit. This also eliminates noisy notifications, especially if you are changing a lot of observables (say, in a loop).
* Actions can call other actions. For such nested actions, the change-notifications will be sent when the top-most action completes.
* All the linked reactions (ones that depend on the observables mutated inside the action) are run only at the end of the action. This also ensures there are no pre-mature reactions occurring in the system.


```dart
final counter = Observable(0);

// Create an action
final incrementCounter = Action((){
    counter.value++;
});

// Invoke the action
// The first argument is a list of args that are passed to the inner function, which is empty in this case
incrementCounter([]);
```


>By default, MobX enforces a rule that all mutations to observables should happen inside an action. This is part of the configuration for a ReactiveContext. If you mutate an observable that is being observed, outside of an action, MobX will throw an Exception. This is a safety measure to ensure there are no stray mutations happening in your application


### <u> **runInAction**</u>
T runInAction<T>(T Function() fn, {String name, ReactiveContext context})
* Function fn: A function that mutates some observables
* ReactiveContext context: An optional context within which this should be executed. Normally, you don't need to pass any value here, in which case it uses the singleton mainContext of the application.
String name: An optional name to identify this action
* T: the return value of the passed in function fn
Sometimes it can be excessive to have an Action created just to mutate some observables. It's much easier to just wrap your mutating function inside a runInAction().



# **Reactions**

The MobX triad is completed when we add Reactions into the mix. Having reactions is what triggers the reactivity in the system. A reaction implicitly tracks all the observables which are being used and then re-executes its logic whenever the depending observables change.

>Computed, a reaction?Technically, a computed is also a reaction, aka Derivation, as it depends on other observables or computeds. The only difference between a regular Reaction and Computed is that the former does not produce any value. Computeds are mostly read-only observables that derive their value from other observables.

Reactions, or Derivations come in few flavors: autorun, reaction, when and of course the Flutter Widget: Observer.

 All of these variations take a function that is tracked for any observables. When the tracked observables change, the function is re-executed. This simple behavior is the defining characteristic of a reaction. Note that there is no explicit subscription or wiring needed. Reactions also return a disposer-function (ReactionDisposer) that can be invoked to pre-maturely dispose a reaction.

 ### <u> **Autorun** </u>
 >ReactionDisposer autorun(Function(Reaction) fn)

Runs the reaction immediately and also on any change in the observables used inside fn.

```dart
import 'package:mobx/mobx.dart';

final greeting = Observable('Hello World');

final dispose = autorun((_){
  print(greeting.value);
});

greeting.value = 'Hello MobX';

// Done with the autorun()
dispose();


// Prints:
// Hello World
// Hello MobX
```

### <u> **Reaction** </u>
>ReactionDisposer reaction<T>(T Function(Reaction) fn, void Function(T) effect)

Monitors the observables used inside the fn() tracking function and runs the effect() when the tracking function returns a different value. Only the observables inside fn() are tracked.

```dart
import 'package:mobx/mobx.dart';

final greeting = Observable('Hello World');

final dispose = reaction((_) => greeting.value, (msg) => print(msg));

greeting.value = 'Hello MobX'; // Cause a change

// Done with the reaction()
dispose();


// Prints:
// Hello MobX
```
### <u> **When** </u>

>ReactionDisposer when(bool Function(Reaction) predicate, void Function() effect)

Monitors the observables used inside predicate() and runs the effect() when it returns true. After the effect() is run, when automatically disposes itself. So you can think of when as a one-time reaction. You can also dispose when() pre-maturely.
```dart
import 'package:mobx/mobx.dart';

final greeting = Observable('Hello World');

final dispose = when((_) => greeting.value == 'Hello MobX', () => print('Someone greeted MobX'));

greeting.value = 'Hello MobX'; // Causes a change, runs effect and disposes


// Prints:
// Someone greeted MobX
```
### <u> **AsyncReaction** </u>

> Future<void> asyncWhen(bool Function(Reaction) predicate)

Similar to when but returns a Future, which is fulfilled when the predicate() returns true. This is a convenient way of waiting for the predicate() to turn true.
```dart
final completed = Observable(false);

void waitForCompletion() async {
  await asyncWhen(() => completed.value == true);

  print('Completed');
}
```