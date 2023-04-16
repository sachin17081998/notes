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


