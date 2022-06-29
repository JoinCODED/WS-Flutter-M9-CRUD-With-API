To start interacting with the `API`, we need a package that helps us do so.

1. Install the `dio` package into your project:

```shell
flutter pub add dio
```

2. Create a new folder named `services`, and inside it, create a `books.dart` file.

3. Import the `dio` package in the `books.dart` file:

```dart
import "package:dio/dio.dart";
```

4. Inside the `books.dart` file, create a `BooksServices` class and initialize a new `dio` instance:

```dart
class BooksServices {

final Dio _dio = Dio();

}
```

**Note:** We made it private because it should not be accessed outside this file.

Take a look at the API endpoints again:

```
Get, https://coded-books-api-crud.herokuapp.com/books
Post, https://coded-books-api-crud.herokuapp.com/books
Put, https://coded-books-api-crud.herokuapp.com/books/{bookId}
Delete, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```

You may notice that they all start with the same `https://coded-books-api-crud.herokuapp.com`, and thus, it would be a smart idea to hold this value in a variable.

5. Create a private variable named `baseUrl`:

```dart
class BooksServices {
  final Dio _dio = Dio();

  final _baseUrl = 'https://coded-books-api-crud.herokuapp.com';
}
```

Now, we are ready to make our first API call, which is the following one:

```
Get, https://coded-books-api-crud.herokuapp.com/books
```

Try it in your browser first to see what the data looks like. It looks exactly like our model.
In fact, you should always take a look at the data before creating a model to match that data shape.
