# **Stream in Dart**

Streams are similar to Futures in that they both work asynchronously.

One of the key differences is that once a Future is called and starts, it can either return a value or it errors out and then stops. A Stream, on the other hand, can deliver a series of values (data and errors) continuously (more on this later).

So it's technically correct to say Futures are single-value streams or one-time response stream operations. If a method or function needs to return more than one result at different time intervals, or requires continuous updates to be handled the same way, you should probably look into streams.

## Real life example of Streams

1. Suppose you want to connect with some wifi, so you turned on phone wifit and initally when no wifi is available then it shows you nothing but the moment somone turns on their hotspot and your device detects it then an event is emitted through a stream and connection is established 
2. It's similar to playing songs on Spotify and watching videos on platforms like YouTube and Netflix. YouTube or Spotify's music server cleverly breaks songs or videos into small, manageable chunks—a stream of bytes so you don't have to wait until the app finishes downloading. Hence, the name: Streaming.

![stream](images/stream_1.png)


For every stream their is one source that is emitting events in the stream. Now one or more than one listners can listen to this stream

Lets see a simple example in which we will print a number after every one second using stream

```dart
void main() {
  Stream.periodic(const Duration(seconds: 1), (x) => x).listen((event) {
    print(event);
  });
}
//output: 1 2 3 5 ...
```


## Functional Components of a Stream

Stream has 2 major components

1. Stream Controller: This is responsible to manage the stream and helps in controlling the emition of event in a stream. 
2. Stream Subscription: This helps us in subscribing to a stream and access the data that are emitted by controller in a stream by listening/subscribing to the stream.

Lets writ the above code using these components.

```dart
void main() {

  final StreamController streamController = StreamController();

  var value = 0;
  Timer.periodic(const Duration(seconds: 1), (timer) {
    streamController.add(value++);
  });

//stream subscription
  streamController.stream.listen((event) {
    print(event);
  });
}
//output: 1 2 3 4 5 6 ...
```

There are 2 major issues in above code:

1. In the above code our stream is generating value infinately
2. Our subscrition is not terminating at any point even if the stream is stopped, so it is mendator to close the subscription otherwise it will result in memory leaks

updated code:

```dart
//we will close the stream when value=5
void main() {
  final StreamController streamController = StreamController();
  final streamSubscription = streamController.stream.listen((event) {
    print(event);
  });

  var value = 0;
  Timer.periodic(const Duration(seconds: 1), (timer) {
    if (value == 5) {
      timer.cancel();
      streamController.close();
      streamSubscription.cancel();
    }else{

    streamController.add(value++);
    }
  });

//output: 1 2 3 4  
}
```


In the above code currently only one subscription can be attached to our stream. So inorder to attach multiple subscription/listner we need to make our stream broadcast the evens.
example:

```dart
void main() {
  // Stream.periodic(const Duration(seconds: 1), (x) => x).listen((event) {
  //   print(event);
  // });

  final StreamController streamController = StreamController.broadcast();
  final streamSubscription = streamController.stream.listen((event) {
    print('first stream: $event');
  });
  final streamSubscription1 = streamController.stream.listen((event) {
    print('second steram : $event');
  });

  var value = 0;
  Timer.periodic(const Duration(seconds: 1), (timer) {
    if (value == 5) {
      timer.cancel();
      streamController.close();
      streamSubscription.cancel();
      streamSubscription1.cancel();
    } else {
      streamController.add(value++);
    }
  });
}

/* output
first stream: 0
second steram : 0
first stream: 1
second steram : 1
first stream: 2
second steram : 2
first stream: 3
second steram : 3
first stream: 4
second steram : 4
*/
```

suppose you want to print only the largest value that is coming down the stream, so in this case you have first wait for all the element to come down the stream

it can be achived using await 

```dart
void main() async{
  final StreamController streamController = StreamController.broadcast();
  var max = 0;

  var value = 0;
  Timer.periodic(const Duration(seconds: 1), (timer) {
    if (value == 5) {
      timer.cancel();
      streamController.close();
    } else {
      streamController.add(value++);
    }
  });

  await streamController.stream.forEach((event) {
    max = (event > max) ? event : max;
  });

  print(max);
}

//output:4
```



Resources:

1. https://www.freecodecamp.org/news/how-to-use-and-create-streams-in-dart-and-flutter/
2. https://medium.com/dartlang/dart-asynchronous-programming-streams-2569a993324d