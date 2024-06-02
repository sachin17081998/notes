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