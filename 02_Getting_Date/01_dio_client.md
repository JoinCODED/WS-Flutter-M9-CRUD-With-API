To start interacting with our `api`, we need a package that will help us do so.

Install the `dio` package into your project:

```shell
flutter pub add dio
```

Then, Create a folder named `services` and inside it, create a `books.dart` file.

Now let's import the `dio` package in our new file:

```dart
import "package:dio/dio.dart";
```

Then, we will create a new `DioClient` class:

```dart
class DioClient {
}
```

Inside this class, we will initialize a new dio instance:

```dart
  final Dio _dio = Dio();
```

We are making it private because it should'nt be accessed outside this file.

Now let's take a look at our api endpoints again:

```
Get, http://http://10.0.2.2:5000/books
Post, http://http://10.0.2.2:5000/books
Put, http://http://10.0.2.2:5000/books/{bookId}
Delete, http://http://10.0.2.2:5000/books/{bookId}
```

You may have noticed, they are all start with the same `http://http://10.0.2.2:5000`, so it would be a smart idea to hold this value in a variable and let's name it `baseUrl`:

```dart
class DioClient {
  final Dio _dio = Dio();

  final _baseUrl = 'http://10.0.2.2:5000';
}
```

Thats it, we are ready to make our first api call, which is this one:

```
Get, http://http://10.0.2.2:5000/books
```

Try it in your browser first, see how the data looks, exactly like our model, in fact, we should always look at the data first then create out model to match that data shape.
