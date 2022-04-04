I know you are exhausted, this is the last one, and the easiest.

Let's take a look at our endpoint:

```
Delete, http://http://10.0.2.2:5000/books/{bookId}
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
    await DioClient().deleteBook(bookId: bookId);
    books.removeWhere((book) => book.id == bookId);
    notifyListeners();
  }
```

That's it!
