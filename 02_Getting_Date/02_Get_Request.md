Our first request to make is a get request that will fetch a list of books so we can display them in our app.

Create a function called `getBooks` that will return a future list of books:

```dart
  Future<List<Book>> getBooks() async {
  }
```

We will mark it as `async` because it yields a future, and we will initialize the list of books and we will see why in a bit:

```dart
  Future<List<Book>> getBooks() async {
    List<Book> books = [];
  }
```

Now we call call our `_dio` clint and make a call, let's take a look at our endpoint one last time:

```
Get, http://http://10.0.2.2:5000/books
```

What does that `Get` word means? it means the http method we should use is a `get` method, so we call tell `_dio` we want to make a `get` request like this:

```dart
  Future<List<Book>> getBooks() async {
  List<Book> books = [];
  await _dio.get(_baseUrl + '/books');
  }
```

Don't forget the `await` keyword because we need to wait for the server to send us the data back.

Now we need to store the response somewhere, `dio` gives us a date type responsible for this:

```dart
  Future<List<Book>> getBooks() async {
  List<Book> books = [];
  Response response = await _dio.get(_baseUrl + '/books');
  }
```

Our data now lays under `response.data` **AS** `JSON` and we need to convert them to `Book` objects.

We already created a factory constructor called `fromJson` that will handle this!

```dart
  Future<List<Book>> getBooks() async {
  List<Book> books = [];
  Response response = await _dio.get(_baseUrl + '/books');
  books = (response.data as List).map((book) => Book.fromJson(book)).toList();
  }
```

We stored the converted list in the `books` variable and now we can return it.

```dart
  Future<List<Book>> getBooks() async {
    List<Book> books = [];
      Response response = await _dio.get(_baseUrl + '/books');
      books =
          (response.data as List).map((book) => Book.fromJson(book)).toList();
    return books;
  }
```

Hold on, what if our call failed for some reason, maybe the server is down or the user doesn't have an internet connection, wrap you call with a `try-catch` block, but this time it will throw a special `DioError`.

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

That's it, we are done here, next we will go to our `books_provider`.

In there, we will create a similar function with a return type of future void:

```dart
class BooksProvider extends ChangeNotifier {
  List<Book> books = [];

  Future<void> getBooks() async {
  }
}
```

Then we will import our dio services file:

```dart
import 'package:books_app/services/books.dart';
```

And inside our function, we will call the function that we created in the services file:

```dart
  Future<void> getBooks() async {
    books = await DioClient().getBooks();
  }
```

As you see, we stored the response coming from `dio` to the `books` list we have in our provider.

Now let's go to our home page, which is currently rendering our list of books in the `Consumer` widget.

Still nothing.. where's the data? we need to call our `getBooks` function in our provider, and there's a special widget that will help us in that and provides us with some utilities and it's called: `FutureBuilder`.
