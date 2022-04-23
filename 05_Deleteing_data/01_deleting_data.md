I know you are exhausted, this is the last one, and the easiest.

Let's take a look at our endpoint:

```
Delete, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```

What we only need, is the `id` of the book we wanna delete.

So in your `dio` file, create a method that returns future void and takes `int` `id` as an argument:

```dart
  Future<void> deleteBook({required int bookId}) async {
    try {
      await _dio.delete(_baseUrl + '/books/${bookId}');
    } on DioError catch (error) {
      print(error);
    }
  }
```

And in our provider:

```dart
  void deleteBook(int bookId) async {
    await BooksServices().deleteBook(bookId: bookId);
    books.removeWhere((book) => book.id == bookId);
    notifyListeners();
  }
```

That's it!
