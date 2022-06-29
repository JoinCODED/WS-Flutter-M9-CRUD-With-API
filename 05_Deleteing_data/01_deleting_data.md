I know you are exhausted, but this is the last one, and the easiest.

The URL below is the endpoint for deleting a book:

```
Delete, https://coded-books-api-crud.herokuapp.com/books/{bookId}
```

**Note:** The delete method requires the id of the book we want to remove, and it does not require any additional data like the post and update methods.

1. In the `dio` file, create a future void method that takes `int id` as an argument:

```dart
  Future<void> deleteBook({required int bookId}) async {
    try {
      await _dio.delete(_baseUrl + '/books/${bookId}');
    } on DioError catch (error) {
      print(error);
    }
  }
```

2. In the provider file, create a void function for deleting a book, and use the `removeWhere` method to remove the chosen book:

```dart
  void deleteBook(int bookId) async {
    await BooksServices().deleteBook(bookId: bookId);
    books.removeWhere((book) => book.id == bookId);
    notifyListeners();
  }
```

That is it 😉
