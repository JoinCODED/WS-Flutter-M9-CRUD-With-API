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

Then, we will create a new `BooksServices` class:

```dart
class BooksServices {
}
```

Inside this class, we will initialize a new dio instance:

```dart
  final Dio _dio = Dio();
```

We are making it private because it should'nt be accessed outside this file.

Now let's take a look at our api endpoints again:

```
Get, https://coded-books-api-crud.herokuapp.com/books
Post, https://coded-books-api-crud.herokuapp.com/books
Put, https://coded-books-api-crud.herokuapp.com/books/{bookId}
Delete, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```

You may have noticed, they are all start with the same `https://coded-books-api-crud.herokuapp.com`, so it would be a smart idea to hold this value in a variable and let's name it `baseUrl`:

```dart
class BooksServices {
  final Dio _dio = Dio();

  final _baseUrl = 'https://coded-books-api-crud.herokuapp.com';
}
```

Thats it, we are ready to make our first api call, which is this one:

```
Get, https://coded-books-api-crud.herokuapp.com/books
```

Try it in your browser first, see how the data looks, exactly like our model, in fact, we should always look at the data first then create out model to match that data shape.
