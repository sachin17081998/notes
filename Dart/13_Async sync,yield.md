# **Asynchronous and synchronous workflows**

## Synchronous Programming

In synchronous programming the compiler will not move to next instruction until the current isntruction is completely executed.

A synchronous computation can have only two states:
- Unprocessed: compuataion has not been statered yet
- Completed With Value: computation comleted and results a value
- Completed with Error: coputation failed because of some reason and value is not obtained so theme operation completes with an error  


## Asynchronous Programming

In Asynchronous programming if a instruction takes long time to execute then compiler can execute remaing instruction and in parallel will wait for the instruction which takes longer to execute. 

An asynchronous computation cannot provide a result immediately when it is started, unlike a synchronous computation which does compute a result immediately by either returning a value or by throwing. An asynchronous computation may need to wait for something external to the program which takes time. Instead of blocking all computation until the result is available, the asynchronous computation immediately returns a Future which will eventually "complete" with the result.

Asynchronous computation can have following state:
- Unprocessed: compuataion has not been statered yet
- Uncompleted: Computation started but waiting for completing the execution
- Completed With Value: computation comleted and results a value
- Completed with Error: coputation failed because of some reason and value is not obtained so theme operation completes with an error 
  

Here are some common asynchronous operations:
Fetching data over a network.
- Writing to a database.
- Reading data from a file.

 Such asynchronous computations usually provide their result as a Future or, if the result has multiple parts, as a Stream.

## Future 

A 'future' (lower case "f") is an instance of the 'Future' (capitalized "F") class. A future represents the result of an asynchronous operation, and can have two states: uncompleted or completed.

Future is like promise. Suppose you are making a database call to fetch a number so in that case future promisies to return a number when the execution completes so in this case return type is Future<int>.

Suppose you call an api to fetch list of names of student then in this case return type is Future<List<String>>

```dart
//function to downlaod and view image

void downloadImage(){
  final image=http.get('https//myimg.url'); //this return an Future object: Future<Image>

  image.then((img)=> ShowImage(img)); // Bexause of above line a future object(Future<image>) sits in main event loop(or isolate) and once it is avilable the call back inside then executes and show the image
}

```

## Lets try to understand the how diffrent futer methods run by dart main-isolate

Event Queue: It contains all the operations that has to be performed

Microtask queue: this contain operations that has to be run before the element of event queue


### Future.Value
  Creates a future completed with value
  is handy when you already know the value for the future. This constructor is 
  useful when you’re building services that use caching.
   Sometimes you already have the value you need, so you can pop it right in there

### Future.sync
Returns a future containing the result of immediately calling computation.


```dart
void main() {
  print('start');

  Future(() => 1).then(print);
  Future(() => Future(() => 2)).then(print);

  Future.delayed(const Duration(seconds: 3), (() => 3)).then(print);
  Future.delayed(const Duration(seconds: 3), (() => Future(() => 4)))
      .then(print);

  Future.value(5).then(print);
  Future.value(Future(() => 6)).then(print);

  Future.sync(() => 7).then(print);
  Future.sync(() => Future(() => 8)).then(print);


  Future.microtask(() => 9).then(print);
  Future.microtask(() => Future(() => 10)).then(print);

  Future(() => 11).then(print);
  Future(() => Future(() => 12)).then(print);

  print('end');

  output:
  start
  end
  5
  7
  9
  1
  6
  8
  11
  10
  2
  12
  3
  4

// first time when dart start execution from start to end
  //microtask: F(10) 9 7 5
  //event: F(4) 3 F(12) 11 8 6 F(2) 1  [F(4) and 3 are placed at last in event quese because delay is of 1 second and remianing code will excute much before that]
  //output: start end

//second time: all the task s at micro task queue will execute first
// microtask:
//event: F(4) 3 10 F(12) 11 8 6 F(2) 1  [F(10) from microtask queue is put in event quesue because it is a normal future]
//output: start end 5 7 9 

//third time: even quesue will start execution
//microtask:
//event:f(4) 3 12 2  [ f(4) and 3 are still in delay state because of 3 seconds]
//output:start end 5 7 9 1 6 8 11 10

//fourth time
//microtask:
//event: f(4) 3 
//output: start end 5 7 9 1 6 8 11 10 2 12

//after 3 seconds
//microtask:
//event: 4
//output: start end 5 7 9 1 6 8 11 10 2 12 3 


//microtask:
//event: 4
//output: start end 5 7 9 1 6 8 11 10 2 12 3 4

}

```


> Future.value(x) and Future.sync(x) has similar [but no same] behavior:
- x is not future => x will be set as a new microtask event into the microtask queue
- x is Future => x will be set as a new event into the event queue

so 5 and 7 will be put in micro task quesue but 6 and 8 will be in event queue



 ![](images/async_sync_2.png '')

Output for above code is:1 15 2 9 14 4 5 6 8 7 10 13 11 12

 ![](images/async_sync_1.png '')






references:
https://pmatatias.medium.com/flutter-future-future-sync-future-microtask-future-value-etc-3f46aeae1210


```dart

async and await

async and await are keywords that provide a way to make asynchronous operations appear synchronous.

Let’s take an example for async and await keywords,

Future callAsyncFunction(String text) {
return Future.delayed(Duration(seconds: 1)).then((value) => text);
}
void main() async {
print('1st call');
await callAsyncFunction("waited for async to be executed").then((status){
 print(status);
});
print('after async call');
}
output:

1st call 
waited for async to be executed
after async call
Here we can see that the statement after the await call are executed once. The await call successfully returns a future.

yield

yield appends a value to the output stream of the async*function that surrounds it. It is similar to return, but it does not terminate the function.

Generator functions

It generates a sequence of values (Unlike regular functions, which only return a single value).

Generator functions can be:

Asynchronous (return a Stream of values)
Synchronous (return an Iterable with values)
async*

It offers some syntactical sugar to emit a value with the yield keyword and will always return a Stream.

Let’s take an example for async* and yield keywords,

Stream<int> foo() async* {
  for (int i = 0; i < 10; i++) {
    await Future.delayed(const Duration(seconds: 1));
    yield i;
  }
}
sync*

It will return the Iterable values

Let’s take an example for sync* keyword,

Iterable<int> getNumbers(int number) sync* {
 print(‘generator started’);
 for (int i = 0; i < number; i++) {
 yield i;
 }
 print(‘generator ended’);
}
yield*

After delegating the call to another generator, it continues creating its own values once that generator stops producing values.

Let’s take an example for yield* keyword,

Stream<int> word(int n) async* {
  if (n > 0) {  
    await Future.delayed(Duration(seconds: 1));
    yield n;
    yield* word(n - 1);
  }
}
void main() {
  word(3).forEach(print);
}
output:

3
2
1
```