Our first request is `get request`, which fetches a list of books to be displayed in the app.

1. In the `books.dart` file, create a function called `getBooks` that returns a future list of books:

```dart
  Future<List<Book>> getBooks() async {
  }
```

We marked it as `async` because it yields a future.

2. Initialize a list variable named `books` to store the response data in:

```dart
  Future<List<Book>> getBooks() async {
    List<Book> books = [];
  }
```

3. Use the `_dio` client to make a call to:

```
Get, https://coded-books-api-crud.herokuapp.com/books
```

What does the `Get` word mean?
It means we should tell `_dio` that the HTTP method is `get`. In other words, we want to make a `get request`:

```dart
  Future<List<Book>> getBooks() async {
  List<Book> books = [];
  await _dio.get(_baseUrl + '/books');
  }
```

Do not forget the `await` keyword because we have to wait for the server to send us the data back.

Now, we need to store the response somewhere, and `dio` gives us a data type responsible for this:

```dart
  Future<List<Book>> getBooks() async {
  List<Book> books = [];
  Response response = await _dio.get(_baseUrl + '/books');
  }
```

Our data now lays under `response.data` **_as_** `JSON`, and we need to convert it to `Book` objects.

We already created a factory constructor called `fromJson` which handles the converting process.

```dart
  Future<List<Book>> getBooks() async {
  List<Book> books = [];
  Response response = await _dio.get(_baseUrl + '/books');
  books = (response.data as List).map((book) => Book.fromJson(book)).toList(); //here
  }
```

Return the books variable:

```dart
  Future<List<Book>> getBooks() async {
    List<Book> books = [];
      Response response = await _dio.get(_baseUrl + '/books');
      books =
          (response.data as List).map((book) => Book.fromJson(book)).toList();
    return books;
  }
```

4. Wrap the response with a `try-catch` block that throws a special `DioError` to catch any error if the call fails for some reason, like if the server is down or if the user does not have an internet connection.

```dart
  Future<List<Book>> getBooks() async {
    List<Book> books = [];
    try {
      Response response = await _dio.get(_baseUrl + '/books');
      books =
          (response.data as List).map((book) => Book.fromJson(book)).toList();
    } on DioError catch (error) {
      print(error);
    }
    return books;
  }
```

5. In the `books_provider`, import the dio `services` file, and create a function with a return of the future void type:

```dart
import 'package:books_app/services/books.dart';

class BooksProvider extends ChangeNotifier {
  List<Book> books = [];

  Future<void> getBooks() async {
  }
}
```

6. Inside the get function, call the `getBooks()` function that we created in the services file:

```dart
  Future<void> getBooks() async {
    books = await BooksServices().getBooks();
  }
```

As you can see, we stored the response that came from `dio` in the `books` list in the provider.

Now, let's go to the home page, which is currently rendering our list of books in the `Consumer` widget.

Still nothing... where is the data? We need to call the `getBooks` function in the provider. There is a special widget that helps us with this and provides us with some utilities, it's called: `FutureBuilder`.
